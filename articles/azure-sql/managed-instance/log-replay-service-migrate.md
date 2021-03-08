---
title: Migración de bases de datos a SQL Managed Instance mediante el servicio de reproducción de registros
description: Aprenda a migrar bases de datos de SQL Server a SQL Managed Instance mediante el servicio de reproducción de registros.
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.reviewer: sstein
ms.date: 03/01/2021
ms.openlocfilehash: bc0dc72c7547c8f74aec53b7153fc5384c6b634b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101690794"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service-preview"></a>Migración de bases de datos de SQL Server a SQL Managed Instance mediante el servicio de reproducción de registros (versión preliminar)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En este artículo se explica cómo configurar manualmente la migración de bases de datos de SQL Server 2008-2019 a SQL Managed Instance mediante el servicio de reproducción de registros (LRS) actualmente en versión preliminar pública. Se trata de un servicio en la nube habilitado para Managed Instance basado en la tecnología de trasvase de registros de SQL Server. LRS debe usarse en aquellos casos en los que existan migraciones personalizadas y arquitecturas híbridas complejas, cuando se necesite más control, cuando exista poca tolerancia a tiempo de inactividad o cuando no se pueda usar Azure Data Migration Service (DMS).

Tanto DMS como LRS usan la misma tecnología de migración subyacente y las mismas API. Con el lanzamiento de LRS, se permiten migraciones personalizadas complejas y arquitectura híbrida entre locales. SQL Server y SQL Managed Instance.

## <a name="when-to-use-log-replay-service"></a>Cuándo usar el servicio de reproducción de registros

En casos en los que no se pueda usar [Azure DMS](/azure/dms/tutorial-sql-server-to-managed-instance.md) para la migración, el servicio en la nube LRS se puede utilizar directamente con PowerShell, los cmdlets de la CLI o las API para compilar y organizar manualmente las migraciones de bases de datos a SQL Managed Instance. 

Puede considerar el uso del servicio en la nube LRS en algunos de los siguientes casos:
- Se necesita más control para el proyecto de migración de bases de datos
- Existe una pequeña tolerancia a tiempo de inactividad en la migración total
- No se puede instalar el archivo ejecutable de DMS en el entorno
- El archivo ejecutable de DMS no tiene acceso a los archivos de copia de seguridad de la base de datos
- No hay acceso al sistema operativo host disponible o no tiene privilegios de administrador
- No se pueden abrir los puertos de red entre el entorno y Azure
- Las copias de seguridad se almacenan directamente en Azure Blob Storage con la opción TO URL (EN URL)
- Existe la necesidad de usar copias de seguridad diferenciales

> [!NOTE]
> La manera automatizada de migrar las bases de datos de SQL Server a SQL Managed Instance es usar Azure DMS. Este servicio emplea el mismo servicio en la nube LRS en el back-end con el trasvase de registros en modo NORECOVERY. En casos en los que Azure DMS no sea totalmente compatible con sus escenarios, debe considerar la posibilidad de usar LRS manualmente para orquestar las migraciones.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Para crear una solución personalizada mediante LRS para migrar bases de datos a la nube, se necesitan varios pasos de orquestación que se muestran en el diagrama y se describen en la tabla siguiente.

La migración consiste en realizar copias de seguridad completas de la base de datos en SQL Server con CHECKSUM habilitado y copiar los archivos de copia de seguridad en Azure Blob Storage. LRS se usa para restaurar archivos de copia de seguridad de Azure Blob Storage a SQL Managed Instance. Azure Blob Storage se utiliza como almacenamiento intermedio entre SQL Server y SQL Managed Instance.

LRS supervisa Azure Blob Storage para comprobar si se han agregado nuevas copias de seguridad diferenciales o de registros después de haber restaurado la copia de seguridad completa, y restaura automáticamente los archivos nuevos agregados. El progreso de los archivos de copia de seguridad que se restauran en SQL Managed Instance se puede supervisar mediante el servicio, y el proceso también se puede anular si es necesario.

En LRS no hace falta una convención de nomenclatura de archivos de copia de seguridad específica, ya que se examinan todos los archivos colocados en Azure Blob Storage y se construye la cadena de copia de seguridad solo a partir de la lectura de los encabezados de archivo. Las bases de datos se encuentran en estado de restauración durante el proceso de migración, ya que se restauran en modo [NORECOVERY](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql#comparison-of-recovery-and-norecovery) y no se pueden usar para la lectura o la escritura hasta que el proceso de migración haya finalizado completamente. 

En la migración de varias bases de datos, las copias de seguridad de cada base de datos deben colocarse en una carpeta independiente en Azure Blob Storage. LRS debe iniciarse por separado para cada base de datos y se deben especificar diferentes rutas de acceso a las distintas carpetas de Azure Blob Storage. 

LRS se puede iniciar en modo autocompletar o continuo. Cuando se inicia en modo autocompletar, la migración se completa automáticamente cuando se ha restaurado el último archivo de copia de seguridad especificado. Cuando se inicia en modo continuo, el servicio restaura continuamente los nuevos archivos de copia de seguridad agregados y la migración solo se completa en la migración total manual. Se recomienda ejecutar la migración total manual solo después de haber realizado la copia de seguridad del final del registro y que se muestre como restaurada en SQL Managed Instance. En el paso final de la migración total, la base de datos pasará a estar en línea y disponible para la lectura y la escritura en SQL Managed Instance.

Una vez detenido LRS, ya sea automáticamente en autocompletar o manualmente en la migración total, el proceso de restauración no se puede reanudar para las bases de datos que se pusieron en línea en SQL Managed Instance. Para restaurar archivos de copia de seguridad adicionales una vez finalizada la migración mediante autocompletar, o manualmente en la migración total, es necesario reiniciar LRS para eliminar la base de datos y restaurar la cadena de copia de seguridad completa desde el principio.

![Pasos de orquestación del servicio de reproducción de registros explicados para SQL Managed Instance](./media/log-replay-service-migrate/log-replay-service-conceptual.png)
    
| Operación | Detalles |
| :----------------------------- | :------------------------- |
| **1. Realice la copia de seguridad de las bases de datos de SQL Server a Azure Blob Storage**. | -Realice copias de seguridad completas, diferenciales y de registros de SQL Server a un contenedor de Azure Blob Storage mediante [Azcopy](/azure/storage/common/storage-use-azcopy-v10) o el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/). <br />-Use cualquier nombre de archivo, ya que con LRS no hace falta una convención de nomenclatura de archivos específica.<br />-En la migración de varias bases de datos, se necesita una carpeta distinta para cada base de datos. |
| **2. Inicie el servicio LRS en la nube**. | -El servicio se puede iniciar con varios cmdlets: <br /> PowerShell: [start-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> CLI: [az_sql_midb_log_replay_start](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start). <br /> -Inicie LRS por separado para cada base de datos diferente que apunte a una carpeta de copia de seguridad distinta en Azure Blob Storage. <br />-Una vez iniciado, el servicio realizará copias de seguridad del contenedor de Azure Blob Storage y comenzará a restaurarlas en SQL Managed Instance.<br /> -En el caso de que se haya iniciado LRS en modo continuo, una vez que se restauren todas las copias de seguridad inicialmente cargadas, el servicio inspeccionará los nuevos archivos cargados en la carpeta y aplicará continuamente registros según la cadena de LSN, hasta que se detenga el servicio. |
| **2.1. Supervise el progreso de la operación**. | -El progreso de la operación de restauración se puede supervisar con varios cmdlets: <br /> PowerShell: [get-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> CLI: [az_sql_midb_log_replay_show](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show). |
| **2.2. Detenga o anule la operación si es necesario**. | -En caso de que sea necesario anular el proceso de migración, la operación se puede detener con varios cmdlets: <br /> PowerShell: [stop-azsqlinstancedatabaselogreplay]/powershell/module/az.sql/stop-azsqlinstancedatabaselogreplay) <br /> CLI: [az_sql_midb_log_replay_stop](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop). <br /><br />-Como resultado, se elimina la base de datos que se restaura en SQL Managed Instance. <br />-Una vez detenido, no se puede reanudar LRS para una base de datos. El proceso de migración debe reiniciarse desde el principio. |
| **3. Migración total a la nube cuando esté preparado**. | -Detenga la aplicación y la carga de trabajo. Realice la última copia de seguridad del final del registro y cárguela en Azure Blob Storage.<br /> -Para finalizar la migración total, inicie la operación de finalización de LRS con varios cmdlets: <br />PowerShell: [complete-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> CLI: [az_sql_midb_log_replay_complete](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete). <br /><br />-Como resultado, el servicio LRS se detendrá y la base de datos se pondrá en línea para la lectura y la escritura en SQL Managed Instance.<br /> -Vuelva a apuntar la cadena de conexión de la aplicación de SQL Server a SQL Managed Instance. |

## <a name="requirements-for-getting-started"></a>Requisitos para comenzar

### <a name="sql-server-side"></a>En lo que respecta a SQL Server
- SQL Server 2008-2019
- Copia de seguridad completa de las bases de datos (uno o varios archivos)
- Copia de seguridad diferencial (uno o varios archivos)
- Copia de seguridad de registros (sin dividir para el archivo de registro de transacciones)
- **CHECKSUM debe estar habilitado** para las copias de seguridad (obligatorio)

### <a name="azure-side"></a>En lo que respecta a Azure
- El módulo Az.SQL de PowerShell versión 2.16.0 o superior ([puede instalarlo](https://www.powershellgallery.com/packages/Az.Sql/) o usar Azure [Cloud Shell](/azure/cloud-shell/))
- Versión de la CLI 2.19.0 o posterior ([instalar](/cli/azure/install-azure-cli))
- Contenedor de Azure Blob Storage aprovisionado
- Token de seguridad de SAS solo con permisos de **lectura** y **enumeración** generados para el contenedor de almacenamiento de blobs

### <a name="migrating-multiple-databases"></a>Migración de varias bases de datos
- Los archivos de copia de seguridad de diferentes bases de datos se deben colocar en distintas carpetas en Azure Blob Storage.
- LRS debe iniciarse por separado para cada base de datos que apunte a la carpeta adecuada en Azure Blob Storage.
- LRS puede admitir hasta 100 procesos de restauración simultáneos por instancia administrada de SQL.

### <a name="azure-rbac-permissions-required"></a>Se requieren permisos de RBAC de Azure
Para ejecutar LRS mediante los clientes proporcionados, se necesita uno de los siguientes roles de Azure:
- Rol Propietario de la suscripción, o
- Rol [Colaborador de Managed Instance](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) o
- Rol personalizado con el permiso siguiente:
  - `Microsoft.Sql/managedInstances/databases/*`

## <a name="best-practices"></a>Procedimientos recomendados

Se recomiendan los siguientes procedimientos:
- Ejecute [Data Migration Assistant](/sql/dma/dma-overview) para validar que las bases de datos están listas para migrarse a SQL Managed Instance. 
- Divida las copias de seguridad completas y diferenciales en varios archivos, en lugar de uno solo.
- Habilite la compresión de copias de seguridad.
- Use Cloud Shell para ejecutar scripts, ya que siempre se actualizarán con los últimos cmdlets publicados.
- Planee realizar la migración en 47 horas desde el inicio del servicio LRS. Este período de gracia impide las revisiones de software administradas por el sistema una vez que se ha iniciado LRS.

> [!IMPORTANT]
> - La base de datos que se restaura con LRS no se puede usar hasta que el proceso de migración haya finalizado.
> - LRS no admite el acceso de solo lectura a las bases de datos durante la migración.
> - Después de que finaliza la migración, termina el proceso de migración, ya que LRS no admite la reanudación de la restauración.

## <a name="steps-to-execute"></a>Pasos para ejecutar

### <a name="make-backups-on-the-sql-server"></a>Creación de copias de seguridad en SQL Server

Las copias de seguridad en SQL Server se pueden crear con cualquiera de las dos opciones siguientes:

- Copia de seguridad en el almacenamiento en disco local y, luego, carga de los archivos en Azure Blob Storage, en caso de que el entorno impida la copia de seguridad directa en Azure Blob Storage.
- Copia de seguridad directa en Azure Blob Storage con la opción "TO URL" (EN URL) de T-SQL, en caso de que el entorno y los procedimientos de seguridad le permitan hacerlo. 

Establezca las bases de datos que quiere migrar al modo de recuperación completa para permitir copias de seguridad de registros.

```SQL
-- To permit log backups, before the full database backup, modify the database to use the full recovery
USE master
ALTER DATABASE SampleDB
SET RECOVERY FULL
GO
```

Para crear manualmente una copia de seguridad completa, diferencial y de registros de la base de datos en el almacenamiento local, use los siguientes scripts T-SQL de ejemplo proporcionados. Asegúrese de que la opción CHECKSUM está habilitada, ya que se trata de un requisito obligatorio de LRS.

```SQL
-- Example on how to make full database backup to the local disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to the locak disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_diff.bak'
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to the local disk
BACKUP LOG [SampleDB]
TO DISK='C:\BACKUP\SampleDB_log.trn'
WITH COMPRESSION, CHECKSUM
GO
```

### <a name="create-azure-blob-storage"></a>Creación de una instancia de Azure Blob Storage

Azure Blob Storage se utiliza como almacenamiento intermedio para los archivos de copia de seguridad entre SQL Server y SQL Managed Instance. Para crear una cuenta de almacenamiento y un contenedor de blobs dentro de la cuenta de almacenamiento, siga estos pasos:

1. [Cree una cuenta de almacenamiento](../../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Cree un contenedor de blobs](../../storage/blobs/storage-quickstart-blobs-portal.md) dentro de la cuenta de almacenamiento.

### <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>Creación de copias de seguridad de SQL Server a Azure Blob Storage

Para cargar las copias de seguridad en el almacenamiento de blobs para migrar las bases de datos a una instancia administrada mediante LRS, se pueden usar algunos de los enfoques siguientes:
- Utilizar [Azcopy](/azure/storage/common/storage-use-azcopy-v10) o el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer) para cargar las copias de seguridad en un contenedor de blobs.
- Usar el Explorador de Storage en Azure Portal.

### <a name="make-backups-from-sql-server-directly-to-azure-blob-storage"></a>Creación de copias de seguridad de SQL Server directamente en Azure Blob Storage

En caso de que la directiva corporativa y de red lo permita, la alternativa es crear copias de seguridad de SQL Server directamente en Azure Blob Storage mediante la opción [BACKUP TO URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url) (COPIA DE SEGURIDAD EN URL) nativa de SQL Server. Si puede seguir esta opción, no es necesario crear copias de seguridad en el almacenamiento local y cargarlas en Azure Blob Storage.

Como primer paso, en esta operación es necesario generar el token de autenticación de SAS para Azure Blob Storage, que luego debe importarse en SQL Server. El segundo paso consiste en crear copias de seguridad con la opción "TO URL" (EN URL) de T-SQL. Asegúrese de que todas las copias de seguridad se crean con la opción CHEKSUM habilitada.

Como referencia, a continuación, se proporciona código de ejemplo para crear copias de seguridad en Azure Blob Storage. En este ejemplo no se incluyen instrucciones sobre cómo importar el token de SAS. En el tutorial siguiente se proporcionan instrucciones detalladas que incluyen cómo generar e importar el token de SAS en SQL Server: [Uso del servicio Azure Blob Storage con SQL Server](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#1---create-stored-access-policy-and-shared-access-storage). 

```SQL
-- Example on how to make full database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_diff.bak'  
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to URL
BACKUP LOG [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_log.trn'  
WITH COMPRESSION, CHECKSUM
```

### <a name="generate-azure-blob-storage-sas-authentication-for-lrs"></a>Generación de la autenticación de SAS de Azure Blob Storage para LRS

Azure Blob Storage se utiliza como almacenamiento intermedio para los archivos de copia de seguridad entre SQL Server y SQL Managed Instance. El token de autenticación de SAS con permisos de enumeración y solo lectura debe generarse para su uso en el servicio LRS. Este token permite al servicio LRS acceder a Azure Blob Storage y usar los archivos de copia de seguridad para restaurarlos en SQL Managed Instance. Para generar la autenticación de SAS para su uso en LRS, siga estos pasos:

1. Acceda al Explorador de Storage desde Azure Portal.
2. Expanda Contenedores de blobs.
3. Haga clic con el botón derecho en el contenedor de blobs y seleccione Obtener firma de acceso compartido. ![El servicio de reproducción de registros genera el token de autenticación de SAS](./media/log-replay-service-migrate/lrs-sas-token-01.png)
4. Seleccione el período de expiración del token. Asegúrese de que el token sea válido mientras dura la migración.
5. Seleccione la zona horaria del token (UTC o la hora local).
    - La zona horaria del token y la de SQL Managed Instance podrían no coincidir. Asegúrese de que el token de SAS tenga la validez temporal adecuada teniendo en cuenta las zonas horarias. Si es posible, establezca la zona horaria en una hora anterior y posterior a la ventana de migración planeada.
6. Seleccione únicamente permisos de lectura y enumeración.
    - No se deben seleccionar otros permisos, o LRS no podrá iniciarse. Este requisito de seguridad es así por naturaleza.
7. Haga clic en el botón Crear. ![El servicio de reproducción de registros genera el token de autenticación de SAS](./media/log-replay-service-migrate/lrs-sas-token-02.png)

La autenticación de SAS se generará con la validez temporal que haya especificado anteriormente. Necesitará la versión del URI del token generado, como se muestra en la captura de pantalla siguiente.

![Ejemplo de URI de autenticación de SAS generado por el servicio de reproducción de registros](./media/log-replay-service-migrate/lrs-generated-uri-token.png)

### <a name="copy-parameters-from-sas-token-generated"></a>Copia de parámetros del token de SAS generado

Para poder usar correctamente el token de SAS para iniciar LRS, es necesario comprender su estructura. El URI del token de SAS generado consta de dos partes:
- StorageContainerUri y 
- StorageContainerSasToken, separadas por un signo de interrogación (?), como se muestra en la imagen siguiente.

    ![Ejemplo de URI de autenticación de SAS generado por el servicio de reproducción de registros](./media/log-replay-service-migrate/lrs-token-structure.png)

- La primera parte que empieza por "https://" hasta el signo de interrogación (?) se usa con el parámetro StorageContainerURI que se suministra como entrada a LRS. Proporciona a LRS información sobre la carpeta donde se almacenan los archivos de copia de seguridad de base de datos.
- La segunda parte, que empieza después del signo de interrogación (?), en el ejemplo "sp=" y hasta el final de la cadena es el parámetro StorageContainerSasToken. Este es el token de autenticación firmado real, válido durante el tiempo especificado. Esta parte no debe comenzar necesariamente con "sp=", como se muestra, y cada caso podría ser diferente.

Copie los parámetros de la siguiente manera:

1. Copie la primera parte del token desde https:// hasta el signo de interrogación (?) y úsela como parámetro StorageContainerUri en PowerShell o la CLI para iniciar LRS, tal como se muestra en la captura de pantalla siguiente.

    ![Parámetro StorageContainerUri de la copia del servicio de reproducción de registros](./media/log-replay-service-migrate/lrs-token-uri-copy-part-01.png)

2. Copie la segunda parte del token desde el signo de interrogación (?) hasta el final de la cadena, y úsela como parámetro StorageContainerSasToken en PowerShell o la CLI para iniciar LRS, tal como se muestra en la captura de pantalla siguiente.

    ![Parámetro StorageContainerSasToken de la copia del servicio de reproducción de registros](./media/log-replay-service-migrate/lrs-token-uri-copy-part-02.png)

> [!IMPORTANT]
> - Los permisos para el token de SAS en Azure Blob Storage deben ser únicamente los de lectura y enumeración. Si se conceden otros permisos, se producirá un error al iniciar el servicio LRS. Estos requisitos de seguridad son así por naturaleza.
> - El token debe tener la validez temporal adecuada. Asegúrese de que se tienen en cuenta las zonas horarias entre el token y la instancia administrada.
> - Asegúrese de que el parámetro StorageContainerUri de PowerShell o la CLI se copia del URI del token generado, desde https:// hasta el signo de interrogación (?). No incluya el signo de interrogación.
> Asegúrese de que el parámetro StorageContainerSasToken para PowerShell o la CLI se copia del URI del token generado, desde el signo de interrogación (?) hasta el final de la cadena. No incluya el signo de interrogación.

### <a name="log-in-to-azure-and-select-subscription"></a>Inicio de sesión en Azure y selección de la suscripción

Use el siguiente cmdlet de PowerShell para iniciar sesión en Azure:

```powershell
Login-AzAccount
```

Seleccione la suscripción adecuada en la que se encuentra SQL Managed Instance con el siguiente cmdlet de PowerShell:

```powershell
Select-AzSubscription -SubscriptionId <subscription ID>
```

## <a name="start-the-migration"></a>Inicio de la migración

La migración se inicia iniciando el servicio LRS. El servicio se puede iniciar en modo autocompletar o continuo. Cuando se inicia en modo autocompletar, la migración se completa automáticamente cuando se ha restaurado el último archivo de copia de seguridad especificado. Esta opción requiere el comando de inicio para especificar el nombre de archivo del último archivo de copia de seguridad. Cuando LRS se inicia en modo continuo, el servicio restaura continuamente los nuevos archivos de copia de seguridad agregados y la migración finaliza solo con la migración total manual. 

### <a name="start-lrs-in-autocomplete-mode"></a>Inicio de LRS en modo autocompletar

Para iniciar el servicio LRS en modo autocompletar, use los siguientes comandos de PowerShell o de la CLI. Especifique el nombre del último archivo de copia de seguridad con el parámetro -LastBackupName. Tras restaurar el último archivo de copia de seguridad con ese nombre especificado, el servicio iniciará automáticamente una migración total.

Inicio de LRS en modo autocompletar, ejemplo de PowerShell:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoCompleteRestore `
    -LastBackupName "last_backup.bak"
```

Inicio de LRS en modo autocompletar, ejemplo de la CLI:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>Inicio de LRS en modo continuo

Inicio de LRS en modo continuo, ejemplo de PowerShell:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

Inicio de LRS en modo continuo, ejemplo de la CLI:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="scripting-lrs-start-in-continuous-mode"></a>Creación de un script de inicio de LRS en modo continuo

Los clientes de PowerShell y de la CLI para iniciar LRS en modo continuo son sincrónicos. Esto significa que esperarán a que la respuesta de API informe sobre si el trabajo se ha iniciado o no correctamente. Durante esta espera, el comando no devolverá el control al símbolo del sistema. En caso de que se cree un script de la experiencia de migración y que sea necesario que el comando de inicio de LRS devuelva el control inmediatamente para continuar con el resto del script, puede ejecutar PowerShell como trabajo en segundo plano con el modificador -AsJob. Por ejemplo:

```PowerShell
$lrsjob = Start-AzSqlInstanceDatabaseLogReplay <required parameters> -AsJob
```

Cuando se inicia un trabajo en segundo plano, se devuelve inmediatamente un objeto de trabajo, incluso aunque el trabajo tarde mucho en finalizar. Puede seguir trabajando en la sesión sin interrupción mientras se ejecuta el trabajo. Para más información sobre la ejecución de PowerShell como un trabajo en segundo plano, consulte la documentación sobre el [cmdlet Start-Job de PowerShell](/powershell/module/microsoft.powershell.core/start-job#description).

Del mismo modo, para iniciar un comando de la CLI en Linux como un proceso en segundo plano, use el signo de "y" comercial (&) al final del comando de inicio de LRS.

```CLI
az sql midb log-replay start <required parameters> &
```

> [!IMPORTANT]
> Una vez que se haya iniciado LRS, se detendrán las revisiones de software administradas por el sistema durante las próximas 47 horas. Tras pasar esta ventana, la siguiente revisión de software automatizada detendrá automáticamente el LRS en curso. En tal caso, no se puede reanudar la migración y será necesario reiniciarla desde el principio. 

## <a name="monitor-the-migration-progress"></a>Supervisión del progreso de la migración

Para supervisar el progreso de la operación de migración, use el siguiente comando de PowerShell:

```PowerShell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Para supervisar el progreso de la operación de migración, use el siguiente comando de la CLI:

```CLI
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>Detención de la migración

En caso de que necesite detener la migración, use los siguientes cmdlets. Al detener la migración, se elimina la base de datos de restauración en SQL Managed Instance debido a que no será posible reanudarla.

Para detener o anular el proceso de migración, use el siguiente comando de PowerShell:

```PowerShell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Para detener o anular el proceso de migración, use el siguiente comando de la CLI:

```CLI
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>Finalización de la migración (modo continuo)

En caso de que LRS se inicie en modo continuo, una vez que se haya asegurado de que se han restaurado todas las copias de seguridad, el inicio de la migración total finalizará el proceso. Tras completar la operación de migración total, la base de datos se migrará y estará lista para el acceso de lectura y escritura.

Para finalizar el proceso de migración en modo continuo de LRS, use el siguiente comando de PowerShell:

```PowerShell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" `
-LastBackupName "last_backup.bak"
```

Para completar el proceso de migración en modo continuo de LRS, use el siguiente comando de la CLI:

```CLI
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="functional-limitations"></a>Limitaciones funcionales

Las limitaciones funcionales del servicio de reproducción de registros (LRS) son:
- La base de datos que se restaura no se puede usar para el acceso de solo lectura durante el proceso de migración.
- Las revisiones de software administradas por el sistema se bloquearán durante 47 horas desde el inicio de LRS. Tras la expiración de esta ventana de tiempo, la siguiente actualización de software detendrá el LRS. En tal caso, este servicio debe reiniciarse desde el principio.
- LRS requiere que se realicen copias de seguridad de las bases de datos de SQL Server con la opción CHECKSUM habilitada.
- El token de SAS que LRS usa debe generarse para todo el contenedor de Azure Blob Storage y solo debe tener permisos de lectura y enumeración.
- Los archivos de copia de seguridad de diferentes bases de datos se deben colocar en distintas carpetas en Azure Blob Storage.
- LRS debe iniciarse de forma independiente para cada base de datos que apunte a distintas carpetas con archivos de copia de seguridad en Azure Blob Storage.
- LRS puede admitir hasta 100 procesos de restauración simultáneos por instancia administrada de SQL.

## <a name="troubleshooting"></a>Solución de problemas

Una que se inicia el LRS, use los cmdlets de supervisión (get-azsqlinstancedatabaselogreplay o az_sql_midb_log_replay_show) para ver el estado de la operación. Si después de algún tiempo el LRS produce un error al iniciarse, busque algunos de los problemas más comunes:
- ¿Existe ya una base de datos con el mismo nombre en MI de SQL que intenta migrar desde SQL Server? Para resolver este conflicto, cambie el nombre de una de las bases de datos.
- ¿Se creó la copia de seguridad de base de datos en SQL Server mediante la opción **CHECKSUM**?
- ¿Los permisos del token de SAS son solo de **lectura** y **enumeración** para el servicio LRS?
- ¿Se copió el token de SAS de LRS a partir del signo de interrogación "?" y el contenido comenzaba de forma similar a "sv=2020-02-10..."? 
- ¿Es el tiempo de **validez del token** de SAS aplicable a la ventana de tiempo de inicio y finalización de la migración? Podría haber discrepancias debido a las distintas **zonas horarias** que se usan en SQL Managed Instance y el token de SAS. Intente volver a generar el token de SAS extendiendo la validez del token de la ventana de tiempo antes y después de la fecha actual.
- ¿Se han escrito correctamente el nombre de la base de datos, el nombre del grupo de recursos y el nombre de la instancia administrada?
- Si LRS se inició en modo autocompletar, ¿era válido el nombre del último archivo de copia de seguridad especificado?

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Migración de SQL Server a SQL Managed Instance](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md).
- Más información sobre [Diferencias entre SQL Server y Azure SQL Managed Instance](transact-sql-tsql-differences-sql-server.md).
- Más información sobre [Procedimientos recomendados para estimar los costos y el tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs).
