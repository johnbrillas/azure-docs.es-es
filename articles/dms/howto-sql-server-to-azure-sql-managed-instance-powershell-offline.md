---
title: 'PowerShell: Migración de SQL Server a SQL Managed Instance sin conexión'
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre cómo migrar sin conexión de SQL Server a Azure SQL Managed Instance mediante Azure PowerShell y Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019,fasttrack-edit, devx-track-azurepowershell
ms.topic: how-to
ms.date: 12/16/2020
ms.openlocfilehash: 90663b6beb4f1e3f7ade32e603a53c8b9d9158f5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98697760"
---
# <a name="migrate-sql-server-to-sql-managed-instance-offline-with-powershell--azure-database-migration-service"></a>Migración de SQL Server a SQL Managed Instance sin conexión con PowerShell y Azure Database Migration Service

En este artículo, se va a migrar sin conexión la base de datos **Adventureworks2016** restaurada en una instancia local de SQL Server 2005, o una versión superior, a una instancia de Azure SQL Managed Instance mediante Microsoft Azure PowerShell. Se pueden migrar bases de datos desde una instancia de SQL Server a una Instancia administrada de SQL mediante el módulo `Az.DataMigration` en Microsoft Azure PowerShell.

En este artículo aprenderá a:
> [!div class="checklist"]
>
> * Cree un grupo de recursos.
> * Crear una instancia de Azure Database Migration Service.
> * Crear un proyecto de migración en una instancia de Azure Database Migration Service.
> * Ejecute la migración sin conexión.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

En este artículo se proporcionan los pasos para realizar una migración sin conexión, pero también es posible realizarla [en línea](howto-sql-server-to-azure-sql-managed-instance-powershell-online.md).


## <a name="prerequisites"></a>Requisitos previos

Para completar estos pasos, necesitará lo siguiente:

* [SQL Server 2016 o posterior](https://www.microsoft.com/sql-server/sql-server-downloads) (cualquier edición).
* Una copia local de la base de datos **AdventureWorks2016**, que está disponible para su descarga [aquí](/sql/samples/adventureworks-install-configure).
* Para habilitar el protocolo TCP/IP, que se deshabilita de forma predeterminada con la instalación de SQL Server Express. Habilite el protocolo TCP/IP siguiendo el artículo [Habilitar o deshabilitar un protocolo de red de servidor](/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* Configurar [Firewall de Windows para el acceso al motor de base de datos](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una Instancia administrada de SQL. Se puede crear una Instancia administrada de SQL mediante los pasos que se describen en el artículo [Creación de una Instancia administrada de ASQL](../azure-sql/managed-instance/instance-create-quickstart.md).
* Descargar e instalar [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) versión 3.3 o posterior.
* Una instancia de Microsoft Azure Virtual Network creada con el modelo de implementación de Azure Resource Manager, que proporciona a Azure Database Migration Service conectividad de sitio a sitio a los servidores de origen locales a través de [ExpressRoute](../expressroute/expressroute-introduction.md) o [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* Una evaluación completada de la base de datos local y la migración del esquema con Data Migration Assistant, como se describe en el artículo [Realizar una evaluación de migración de SQL Server](/sql/dma/dma-assesssqlonprem).
* Descargar e instalar el módulo `Az.DataMigration` (versión 0.7.2 o posterior) de la Galería de PowerShell con el [cmdlet Install-Module de PowerShell](/powershell/module/powershellget/Install-Module).
* Asegurarse de que las credenciales usadas para conectarse a la instancia de SQL Server de origen tengan el permiso [CONTROL SERVER](/sql/t-sql/statements/grant-server-permissions-transact-sql).
* Asegurarse de que las credenciales usadas para conectarse a la Instancia administrada de SQL de destino tengan el permiso CONTROL DATABASE en las bases de datos de la Instancia administrada de SQL de destino.


## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Inicio de sesión en la suscripción de Microsoft Azure

Inicie sesión en su suscripción de Azure mediante PowerShell. Para más información, consulte el artículo [Inicio de sesión con Azure PowerShell](/powershell/azure/authenticate-azureps).

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos mediante el comando [`New-AzResourceGroup`](/powershell/module/az.resources/new-azresourcegroup).

En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en la región *Este de EE. UU.*

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Creación de una instancia de Azure Database Migration Service

Puede crear una instancia nueva de Azure Database Migration Service mediante el cmdlet `New-AzDataMigrationService`.
Este cmdlet espera los siguientes parámetros requeridos:

* *Nombre del grupo de recursos de Azure*. Puede usar el comando [`New-AzResourceGroup`](/powershell/module/az.resources/new-azresourcegroup) para crear un grupo de recursos de Azure como se mostró anteriormente y proporcionar su nombre como un parámetro.
* *Nombre del servicio*. Cadena que se corresponde con el nombre de servicio único deseado para Azure Database Migration Service.
* *Ubicación*. Especifica la ubicación del servicio. Especifique una ubicación de centro de datos de Azure, por ejemplo, Oeste de EE. UU. o Sudeste de Asia.
* *SKU*. Este parámetro corresponde al nombre de SKU de DMS. Los nombres de SKU admitidos actualmente son *Basic_1vCore*, *Basic_2vCores* y *GeneralPurpose_4vCores*.
* *Identificador de subred virtual*. Puede usar el cmdlet [`New-AzVirtualNetworkSubnetConfig`](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) para crear una subred.

En el ejemplo siguiente se crea un servicio denominado *MyDMS* en el grupo de recursos *MyDMSResourceGroup* que se encuentra en la región *Este de EE. UU.* con una red virtual denominada *MyVNET* y una subred llamada *MySubnet*.

```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Cree un proyecto de migración después de crear una instancia de Azure Database Migration Service. Un proyecto de Azure Database Migration Service requiere información de conexión para las instancias de origen y de destino, así como una lista de bases de datos que desee migrar como parte del proyecto.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Creación de un objeto de información de conexión de base de datos para las conexiones de origen y de destino

Puede crear un objeto de información de conexión de base de datos mediante el cmdlet `New-AzDmsConnInfo`, que espera los siguientes parámetros:

* *ServerType*. El tipo de conexión de base de datos solicitado, por ejemplo, SQL, Oracle o MySQL. Utilice SQL para SQL Server y Azure SQL.
* *DataSource*. Nombre o dirección IP de una instancia de SQL Server o de Azure SQL Database.
* *AuthType*. El tipo de autenticación para la conexión, que puede ser SqlAuthentication o WindowsAuthentication.
* *TrustServerCertificate*. Este parámetro establece un valor que indica si el canal se cifra mientras se omite recorrer la cadena de certificados para validar la confianza. El valor puede ser `$true` o `$false`.

En el ejemplo siguiente se crea un objeto de información de conexión para el servidor SQL Server de origen denominado *MySourceSQLServer* con autenticación de SQL:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

En el ejemplo siguiente se muestra la creación de un objeto de información de conexión para una Instancia administrada de Azure SQL Managed Instance denominado "targetmanagedinstance":

```powershell
$targetResourceId = (Get-AzSqlInstance -Name "targetmanagedinstance").Id
$targetConnInfo = New-AzDmsConnInfo -ServerType SQLMI -MiResourceId $targetResourceId
```

### <a name="provide-databases-for-the-migration-project"></a>Ofrecimiento de bases de datos para el proyecto de migración

Cree una lista de objetos `AzDataMigrationDatabaseInfo` que especifique las bases de datos como parte del proyecto de Azure Database Migration Service, que puede proporcionarse como parámetro para la creación del proyecto. Puede usar el cmdlet `New-AzDataMigrationDatabaseInfo` para crear `AzDataMigrationDatabaseInfo`.

En el ejemplo siguiente se crea el proyecto `AzDataMigrationDatabaseInfo` para la base de datos **AdventureWorks2016** y se agrega a la lista que se va a proporcionar como parámetro para la creación del proyecto.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Creación de un objeto del proyecto

Por último, puede crear el proyecto de Azure Database Migration Service denominado *MyDMSProject* ubicado en el *Este de EE. UU.* con `New-AzDataMigrationProject` y agregar las conexiones de origen y de destino creadas anteriormente y la lista de bases de datos que se van a migrar.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Creación e inicio de una tarea de migración

Después, cree e inicie una tarea de Azure Database Migration Service. Esta tarea requiere información de credenciales de conexión de origen y destino, así como una lista de las tablas de base de datos que se van a migrar, además de la información que ya se proporcionó con el proyecto creado como requisito previo.

### <a name="create-credential-parameters-for-source-and-target"></a>Creación de parámetros de credenciales de origen y de destino

Cree las credenciales de seguridad de conexión como un objeto [PSCredential](/dotnet/api/system.management.automation.pscredential).

En el ejemplo siguiente se muestra la creación de objetos *PSCredential* para las conexiones de origen y destino, para lo que se proporcionan contraseñas como variables de cadena *$sourcePassword* y *$targetPassword*.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-backup-fileshare-object"></a>Creación de un objeto FileShare para copias de seguridad

Cree ahora un objeto FileShare que representa el recurso compartido de red SMB local en el que Azure Database Migration Service puede realizar las copias de seguridad de la base de datos de origen con el cmdlet `New-AzDmsFileShare`.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Creación del objeto de la base de datos seleccionada

El siguiente paso consiste en seleccionar las bases de datos de origen y de destino mediante el cmdlet `New-AzDmsSelectedDB`.

El ejemplo siguiente es para migrar una base de datos única de SQL Server a una Instancia administrada de Azure SQL:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

Si toda una instancia de SQL Server necesita una migración mediante lift-and-shift en una Instancia administrada de Azure SQL, encontrará más abajo un bucle para llevar todas las bases de datos del origen. En el ejemplo siguiente, proporcione los detalles de la instancia de SQL Server de origen para $Server, $SourceUserName y $SourcePassword.

```powershell
$Query = "(select name as Database_Name from master.sys.databases where Database_id>4)";
$Databases= (Invoke-Sqlcmd -ServerInstance "$Server" -Username $SourceUserName
-Password $SourcePassword -database master -Query $Query)
$selectedDbs=@()
foreach($DataBase in $Databases.Database_Name)
    {
      $SourceDB=$DataBase
      $TargetDB=$DataBase
      
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
                                              -Name $SourceDB `
                                              -TargetDatabaseName $TargetDB `
                                              -BackupFileShare $backupFileShare
      }
```

### <a name="sas-uri-for-azure-storage-container"></a>URI de SAS del contenedor de Azure Storage

Cree una variable que contenga el URI de SAS que proporciona a Azure Database Migration Service acceso al contenedor de cuentas de almacenamiento en el que el servicio carga los archivos de copia de seguridad.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

> [!NOTE]
> Azure Database Migration Service no admite el uso de un token de SAS de nivel de cuenta. Debe usar un URI de SAS para el contenedor de la cuenta de almacenamiento. [Más información sobre cómo obtener el identificador URI de SAS para el contenedor de blobs](../vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container).

### <a name="additional-configuration-requirements"></a>Requisitos de configuración adicionales

Hay algunos requisitos adicionales que debe tener en cuenta:


* **Selección de inicios de sesión**. Cree una lista de inicios de sesión que se van a migrar tal y como se muestra en el ejemplo siguiente:

    ```powershell
    $selectedLogins = @("user1", "user2")
    ```

    > [!IMPORTANT]
    > Actualmente, Azure Database Migration Service solo admite la migración de inicios de sesión de SQL.

* **Selección de trabajos del agente**. Cree una lista de trabajos del agente que se van a migrar tal y como se muestra en el ejemplo siguiente:

    ```powershell
    $selectedAgentJobs = @("agentJob1", "agentJob2")
    ```

    > [!IMPORTANT]
    > Actualmente, Azure Database Migration Service solo admite trabajos con los pasos de trabajo del subsistema T-SQL.



### <a name="create-and-start-the-migration-task"></a>Creación e inicio de la tarea de migración

Utilice el cmdlet `New-AzDataMigrationTask` para crear e iniciar una tarea de migración.

#### <a name="specify-parameters"></a>Especificar parámetros

El cmdlet `New-AzDataMigrationTask` espera los siguientes parámetros:

* *TaskType*. Tipo de tarea de migración que se va a crear para SQL Server con el tipo de migración de Instancia administrada de Azure SQL *MigrateSqlServerSqlDbMi*. 
* *Nombre del grupo de recursos*. Nombre del grupo de recursos de Azure en el que se crea la tarea.
* *ServiceName*. Instancia de Azure Database Migration Service en la que crear la tarea.
* *ProjectName*. Nombre del proyecto de Azure Database Migration Service en el que se va a crear la tarea. 
* *TaskName*. Nombre de la tarea que se va a crear. 
* *SourceConnection*. Objeto AzDmsConnInfo que representa la conexión de origen de SQL Server.
* *TargetConnection*. Objeto AzDmsConnInfo que representa la conexión de la Instancia administrada de destino de Azure SQL.
* *SourceCred*. Objeto [PSCredential](/dotnet/api/system.management.automation.pscredential) para la conexión al servidor de origen.
* *TargetCred*. Objeto [PSCredential](/dotnet/api/system.management.automation.pscredential) para la conexión al servidor de destino.
* *SelectedDatabase*. Objeto AzDataMigrationSelectedDB que representa la asignación de base de datos de origen y de destino.
* *BackupFileShare*. Objeto FileShare que representa el recurso compartido de red local en el que Azure Database Migration Service puede realizar las copias de seguridad de la base de datos de origen.
* *BackupBlobSasUri*. URI de SAS que proporciona a Azure Database Migration Service acceso al contenedor de cuentas de almacenamiento en el que el servicio carga los archivos de copia de seguridad. Aprenda a obtener el identificador URI de SAS del contenedor de blobs.
* *SelectedLogins*. Lista de los inicios de sesión seleccionados para la migración.
* *SelectedAgentJobs*. Lista de los trabajos de agente seleccionados para la migración.
* *SelectedLogins*. Lista de los inicios de sesión seleccionados para la migración.
* *SelectedAgentJobs*. Lista de los trabajos de agente seleccionados para la migración.



#### <a name="create-and-start-a-migration-task"></a>Creación e inicio de una tarea de migración

En el ejemplo siguiente se crea y se inicia una tarea de migración sin conexión denominada **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```


## <a name="monitor-the-migration"></a>Supervisión de la migración

Para supervisar la migración, realice las tareas siguientes.

1. Consolide todos los detalles de la migración en una variable denominada $CheckTask.

    Para combinar los detalles de la migración (como las propiedades, el estado y la información de la base de datos asociada a la migración), use el siguiente fragmento de código:

    ```powershell
    $CheckTask= Get-AzDataMigrationTask     -ResourceGroupName myResourceGroup `
                                            -ServiceName $service.Name `
                                        -ProjectName $project.Name `
                                            -Name myDMSTask `
                                            -ResultType DatabaseLevelOutput `
                        -Expand 
    Write-Host ‘$CheckTask.ProjectTask.Properties.Output’
    ```

2. Use la variable `$CheckTask` para obtener el estado actual de la tarea de migración.

    Para usar la variable `$CheckTask` a fin de obtener el estado actual de la tarea de migración, puede supervisar la tarea de migración que se ejecuta mediante la consulta de la propiedad de estado de la tarea, como se muestra en el ejemplo siguiente:

    ```powershell
    if (($CheckTask.ProjectTask.Properties.State -eq "Running") -or ($CheckTask.ProjectTask.Properties.State -eq "Queued"))
    {
      Write-Host "migration task running"
    }
    else if($CheckTask.ProjectTask.Properties.State -eq "Succeeded")
    { 
      Write-Host "Migration task is completed Successfully"
    }
    else if($CheckTask.ProjectTask.Properties.State -eq "Failed" -or $CheckTask.ProjectTask.Properties.State -eq "FailedInputValidation" -or $CheckTask.ProjectTask.Properties.State -eq "Faulted")
    { 
      Write-Host "Migration Task Failed"
    }
    ```


## <a name="delete-the-instance-of-azure-database-migration-service"></a>Eliminación de la instancia de Azure Database Migration Service

Una vez completada la migración, puede eliminar la instancia de Azure Database Migration Service:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Azure Database Migration Service en el artículo [¿Qué es Azure Database Migration Service?](./dms-overview.md)

Para obtener información sobre escenarios de migración adicionales (pares de origen/destino), consulte la [guía de Database Migration](https://datamigration.microsoft.com/) de Microsoft.