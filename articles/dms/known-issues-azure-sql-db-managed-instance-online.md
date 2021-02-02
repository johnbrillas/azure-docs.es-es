---
title: Problemas conocidos y limitaciones con las migraciones en línea a Instancia administrada de Azure SQL
description: Información sobre problemas conocidos o limitaciones de migración asociadas a las migraciones en línea a Instancia administrada de Azure SQL.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 02/20/2020
ms.openlocfilehash: 46c5f5995c7a1d4eb074f6c1b25ecaad7e2da37e
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98695547"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-managed-instance"></a>Problemas conocidos o limitaciones con las migraciones en línea a Instancia administrada de Azure SQL

A continuación se describen los problemas conocidos y las limitaciones relacionadas con las migraciones en línea de SQL Server a Instancia administrada de Azure SQL.

> [!IMPORTANT]
> Con las migraciones en línea de SQL Server a Azure SQL Database, no se admite la migración de los tipos de datos SQL_variant.

## <a name="backup-requirements"></a>Requisitos de la copia de seguridad

- **Copias de seguridad con suma de comprobación**

    Azure Database Migration Service usa el método de copia de seguridad y restauración para migrar las bases de datos locales a Instancia administrada de SQL. Actualmente, Azure Database Migration Service solo admite las copias de seguridad creadas mediante la suma de comprobación.

    [Habilitación o deshabilitación de sumas de comprobación de copia de seguridad durante copia de seguridad o restauración (SQL Server)](/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).

    > [!NOTE]
    > Si realiza copias de seguridad de base de datos con compresión, la suma de comprobación es un comportamiento predeterminado, a menos que se deshabilite explícitamente.

    Con las migraciones sin conexión, si elige **I will let Azure Database Migration Service…** (Permitiré a Azure Database Migration Service...), entonces Azure Database Migration Service realizará la copia de seguridad de base de datos con la opción de suma de comprobación habilitada.

- **Medio de copia de seguridad**

    Asegúrese de realizar todas las copias de seguridad en un medio de copia de seguridad independiente (archivos de copia de seguridad). Azure Database Migration Service no admite las copias de seguridad que se anexan a un único archivo de copia de seguridad. Realice copias de seguridad completas y regístrelas en archivos de copia de seguridad independientes.

## <a name="data-and-log-file-layout"></a>Diseño del archivo de registro y datos

- **Número de archivos de registro**

    Azure Database Migration Service no admite bases de datos con varios archivos de registro. Si tiene varios archivos de registro, puede reducirlos y reorganizarlos en un único archivo de registro de transacciones. Dado que no es posible controlar de forma remota los archivos de registro que no están vacíos, primero debe realizar una copia de seguridad del archivo de registro.

## <a name="sql-server-features"></a>características de SQL Server

- **FileStream/FileTables**

    En la actualidad, Instancia administrada de SQL no admite FileStream ni FileTables. En el caso de las cargas de trabajo que dependen de estas características, se recomienda que opte por los servidores SQL Server que se ejecutan en máquinas virtuales de Azure como destino de Azure.

- **Tablas en memoria**

    OLTP en memoria está disponible en los niveles Prémium y Crítico para la empresa para Instancia administrada de SQL. El nivel De uso general no admite OLTP en memoria.

## <a name="migration-resets"></a>Restablecimientos de migración

- **Implementaciones**

    Instancia administrada de SQL es un servicio PaaS con revisiones y actualizaciones de versiones automáticas. Durante la migración de SQL Managed Instance, las actualizaciones no críticas se conservan por hasta 36 horas. Después (y para las actualizaciones críticas), si se interrumpe la migración, el proceso restablece el entorno a un estado de restauración completa.

    Solo se puede llamar a la migración total una vez se restaura la copia de seguridad completa y se pone al día con todas las copias de seguridad de registros. Si las migraciones totales de producción se ven afectadas, póngase en contacto con el [alias de comentarios de Azure DMS](mailto:dmsfeedback@microsoft.com).

## <a name="smb-file-share-connectivity"></a>Conectividad de recursos compartidos de archivos SMB

Los problemas para conectarse al recurso compartido de archivos SMB probablemente se deban a un problema de permisos. 

Para probar la conectividad de recursos compartidos de archivos SMB, siga estos pasos: 

1. Guarde una copia de seguridad en el recurso compartido de archivos SMB. 
1. Compruebe la conectividad de red entre la subred de Azure Database Migration Service y el servidor SQL Server de origen. La forma más fácil de hacerlo es implementar una máquina virtual SQL Server en la subred DMS y conectarse al servidor SQL Server de origen mediante SQL Server Management Studio. 
1. Restaure el encabezado en el servidor SQL Server de origen desde la copia de seguridad en el recurso compartido de archivos: 

   ```sql
   RESTORE HEADERONLY   
   FROM DISK = N'\\<SMB file share path>\full.bak'
   ```

Si no puede conectarse al recurso compartido de archivos, configure los permisos con estos pasos: 

1. Vaya al recurso compartido de archivos mediante el Explorador de archivos. 
1. Haga clic con el botón derecho en el recurso compartido de archivos y seleccione Propiedades. 
1. Elija la pestaña **Compartir** y seleccione **Uso compartido avanzado**. 
1. Agregue la cuenta de Windows usada para la migración y asígnele acceso de control total. 
1. Agregue la cuenta de servicio de SQL Server y asígnele acceso de control total. Compruebe el **Administrador de configuración de SQL Server** para la cuenta de servicio de SQL Server si no está seguro de qué cuenta se está utilizando. 

   :::image type="content" source="media/known-issues-azure-sql-db-managed-instance-online/assign-fileshare-permissions.png" alt-text="Conceda el acceso de control total a las cuentas de Windows usadas para la migración y para la cuenta de servicio de SQL Server. ":::

