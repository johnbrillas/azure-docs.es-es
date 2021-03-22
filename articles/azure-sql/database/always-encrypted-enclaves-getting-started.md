---
title: 'Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database'
description: En este tutorial aprenderá a crear un entono básico de Always Encrypted con enclaves seguros en Azure SQL Database, a cifrar los datos en contexto y a emitir consultas confidenciales enriquecidas en columnas cifradas mediante SQL Server Management Studio (SSMS).
keywords: cifrar datos, cifrado sql, cifrado de base de datos, datos confidenciales, Always Encrypted, enclaves seguros, SGX, atestación
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: 809ac72977b670faff984ad39effb1c70767e141
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102120953"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-azure-sql-database"></a>Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted con enclaves seguros para Azure SQL Database está actualmente en **versión preliminar pública**.

En este tutorial se explica cómo empezar a trabajar con [Always Encrypted con enclaves seguros](/sql/relational-databases/security/encryption/always-encrypted-enclaves) en Azure SQL Database. En él encontrará:

> [!div class="checklist"]
> - Cómo crear un entorno para probar y evaluar Always Encrypted con enclaves seguros.
> - Cómo cifrar datos en contexto y emitir consultas confidenciales enriquecidas en columnas cifradas mediante SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Prerrequisitos

Para realizar este tutorial, es necesario Azure PowerShell y [SSMS](/sql/ssms/download-sql-server-management-studio-ssms).

### <a name="powershell-requirements"></a>Requisitos de PowerShell

Consulte [Introducción a Azure PowerShell](/powershell/azure) para obtener información sobre cómo instalar y ejecutar Azure PowerShell. 

Versión mínima de los módulos Az necesarios para admitir las operaciones de atestación:

- Az 4.5.0
- Az.Accounts 1.9.2
- Az.Attestation 0.1.8

Ejecute el siguiente comando para comprobar la versión instalada de todos los módulos Az:

```powershell
Get-InstalledModule
```

Si las versiones no coinciden con el requisito mínimo, ejecute el comando `Update-Module`.

La Galería de PowerShell ha dejado de usar las versiones 1.0 y 1.1 de Seguridad de la capa de transporte (TLS). Se recomienda usar TLS 1.2 o una versión posterior. Puede recibir los siguientes errores si usa una versión de TLS inferior a la 1.2:

- `WARNING: Unable to resolve package source 'https://www.powershellgallery.com/api/v2'`
- `PackageManagement\Install-Package: No match was found for the specified search criteria and module name.`

Para seguir interactuando con la Galería de PowerShell, ejecute el siguiente comando antes que los comandos Install-Module:

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

### <a name="ssms-requirements"></a>Requisitos de SSMS

Para obtener información acerca de cómo descargar SSMS, consulte [Descarga de SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms).

La versión mínima requerida de SSMS es la 18.8.


## <a name="step-1-create-and-configure-a-server-and-a-dc-series-database"></a>Paso 1: Creación y configuración de un servidor y una base de datos de la serie DC

En este paso, creará un nuevo servidor lógico y una nueva base de datos de Azure SQL Database con la generación de hardware de la serie DC, requerida para Always Encrypted con enclaves seguros. Para más información, consulte [Serie DC](service-tiers-vcore.md#dc-series).

1. Abra una consola de PowerShell e importe la versión necesaria de Az.

  ```PowerShell
  Import-Module "Az" -MinimumVersion "4.5.0"
  ```
  
2. Inicie sesión en Azure. Si es necesario, [cambie a la suscripción](/powershell/azure/manage-subscriptions-azureps) que está utilizando para este tutorial.

  ```PowerShell
  Connect-AzAccount
  $subscriptionId = "<your subscription ID>"
  Set-AzContext -Subscription $subscriptionId
  ```

3. Crear un grupo de recursos. 

  > [!IMPORTANT]
  > Debe crear el grupo de recursos en una región (ubicación) que admita la generación de hardware de la serie DC y Microsoft Azure Attestation. Para obtener la lista de regiones que admiten la serie DC, consulte [Disponibilidad de la serie DC](service-tiers-vcore.md#dc-series-1). [Esta](https://azure.microsoft.com/global-infrastructure/services/?products=azure-attestation) es la disponibilidad regional de Microsoft Azure Attestation.

  ```powershell
  $resourceGroupName = "<your new resource group name>"
  $location = "<Azure region supporting DC-series and Microsoft Azure Attestation>"
  New-AzResourceGroup -Name $resourceGroupName -Location $location
  ```

4. Cree un servidor lógico de Azure SQL. Cuando se le solicite, escriba el nombre del administrador del servidor y una contraseña. Asegúrese de recordar el nombre de administrador y la contraseña, ya que los necesitará más adelante para conectarse al servidor.

  ```powershell
  $serverName = "<your server name>" 
  New-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName -Location $location 
  ```

5. Cree una regla de firewall de servidor que permita el acceso desde el intervalo IP especificado.
  
  ```powershell
  $startIp = "<start of IP range>"
  $endIp = "<end of IP range>"
  $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
  ```

6. Asigne una identidad del sistema administrada al servidor. 

  ```PowerShell
  $server = Set-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName -AssignIdentity
  $serverObjectId = $server.Identity.PrincipalId
  ```

7. Cree una base de datos de la serie DC.

  ```powershell
  $databaseName = "ContosoHR"
  $edition = "GeneralPurpose"
  $vCore = 2
  $generation = "DC"
  New-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -Edition $edition `
    -Vcore $vCore `
    -ComputeGeneration $generation
  ```

8. Recupere y guarde la información sobre el servidor y la base de datos. Necesitará esta información, así como el nombre de administrador y la contraseña del paso 4 de esta sección, en secciones posteriores.

  ```powershell
  Write-Host 
  Write-Host "Fully qualified server name: $($server.FullyQualifiedDomainName)" 
  Write-Host "Server Object Id: $serverObjectId"
  Write-Host "Database name: $databaseName"
  ```
  
## <a name="step-2-configure-an-attestation-provider"></a>Paso 2: Configuración de un proveedor de atestación 

En este paso, creará y configurará un proveedor de atestación en Microsoft Azure Attestation. Esto es necesario para la atestación del enclave seguro que utiliza la base de datos.

1. Copie la directiva de atestación siguiente y guárdela en un archivo de texto (txt). Para más información sobre la directiva siguiente, vea [Creación y configuración de un proveedor de atestación](always-encrypted-enclaves-configure-attestation.md#create-and-configure-an-attestation-provider).

  ```output
  version= 1.0;
  authorizationrules 
  {
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
  };
  ```

2. Importe las versiones necesarias de `Az.Attestation`.  

  ```powershell
  Import-Module "Az.Attestation" -MinimumVersion "0.1.8"
  ```
  
3. Cree un proveedor de atestación. 

  ```powershell
  $attestationProviderName = "<your attestation provider name>" 
  New-AzAttestation -Name $attestationProviderName -ResourceGroupName $resourceGroupName -Location $location
  ```

4. Configure la directiva de atestación.
  
  ```powershell
  $policyFile = "<the pathname of the file from step 1 in this section>"
  $teeType = "SgxEnclave"
  $policyFormat = "Text"
  $policy=Get-Content -path $policyFile -Raw
  Set-AzAttestationPolicy -Name $attestationProviderName `
    -ResourceGroupName $resourceGroupName `
    -Tee $teeType `
    -Policy $policy `
    -PolicyFormat  $policyFormat
  ```

5. Conceda al servidor lógico de Azure SQL acceso al proveedor de atestación. En este paso, va a usar el identificador de objeto de la identidad de servicio administrada que asignó al servidor anteriormente.

  ```powershell
  New-AzRoleAssignment -ObjectId $serverObjectId `
    -RoleDefinitionName "Attestation Reader" `
    -ResourceName $attestationProviderName `
    -ResourceType "Microsoft.Attestation/attestationProviders" `
    -ResourceGroupName $resourceGroupName  
  ```

6. Recupere la dirección URL de atestación que apunta a una directiva de atestación que configuró para el enclave de SGX. Guarde la dirección URL porque la necesitará más adelante.

  ```powershell
  $attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $resourceGroupName 
  $attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
  Write-Host
  Write-Host "Your attestation URL is: $attestationUrl"
  ```
  
  La dirección URL de atestación debe tener el siguiente aspecto: `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`

## <a name="step-3-populate-your-database"></a>Paso 3: Relleno de la base de datos

En este paso, creará una tabla y la rellenará con algunos datos que más adelante cifrará y consultará.

1. Abra SSMS y conéctese a la base de datos **ContosoHR** en el servidor lógico de Azure SQL que creó **sin** Always Encrypted habilitado en la conexión de base de datos.
    1. En el cuadro de diálogo **Conectar con el servidor**, especifique el nombre completo del servidor (por ejemplo, *myserver123.database.windows.net*) y escriba el nombre de usuario del administrador y la contraseña que especificó al crear el servidor.
    2. Haga clic en **Opciones >>** y seleccione la pestaña **Propiedades de conexión**. Asegúrese de seleccionar la base de datos **ContosoHR** (no la base de datos maestra predeterminada). 
    3. Seleccione la pestaña **Always Encrypted**.
    4. Asegúrese de que la casilla **Habilitar Always Encrypted (cifrado de columna)** **no** esté activada.

        ![Conexión sin Always Encrypted](media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    5. Haga clic en **Conectar**.

2. Cree una tabla denominada **Empleados**.

    ```sql
    CREATE SCHEMA [HR];
    GO

    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    GO
    ```

3. Agregue a esta tabla algunos registros de **empleados**.

    ```sql
    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```


## <a name="step-4-provision-enclave-enabled-keys"></a>Paso 4: Aprovisionar claves habilitadas para el enclave

En este paso, creará una clave de columna maestra y una clave de cifrado de columna que admiten los cálculos de enclave.

1. Con la instancia de SSMS del paso anterior, en el **Explorador de objetos**, expanda la base de datos y vaya a **Seguridad** > **Claves de Always Encrypted**.
1. Aprovisione una nueva clave maestra de columna habilitada para el enclave:
    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna…** .
    2. Seleccione el nombre de clave maestra de columna: **CMK1**.
    3. Seleccione **Almacén de certificados de Windows (usuario actual o equipo local)** o **Azure Key Vault**.
    4. Seleccione **Permitir cálculos de enclave**.
    5. Si ha seleccionado Azure Key Vault, inicie sesión en Azure y seleccione su almacén de claves. Para obtener más información sobre cómo crear un almacén de claves para Always Encrypted, consulte [Administrar sus almacenes de claves desde Azure Portal](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Seleccione su certificado o su clave de Azure Key Vault si ya existe, o bien haga clic en el botón **Generar certificado** para crear uno.
    7. Seleccione **Aceptar**.

        ![Permitir cálculos de enclave](media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Cree una nueva clave de cifrado de columnas habilitada para el enclave:

    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna**.
    2. Escriba un nombre para la nueva clave de cifrado de columnas: **CEK1**.
    3. En la lista desplegable **Clave maestra de columna**, seleccione la clave maestra de columna que creó en los pasos anteriores.
    4. Seleccione **Aceptar**.

## <a name="step-5-encrypt-some-columns-in-place"></a>Paso 5: Cifrar algunas columnas en contexto

En este paso, va a cifrar los datos almacenados en las columnas **SSN** y **Salario** dentro del enclave del lado servidor y después podrá probar una consulta SELECT de los datos.

1. Abra una nueva instancia de SSMS y conéctese a la base de datos **con** Always Encrypted habilitado para la conexión de base de datos.
    1. Inicie una nueva instancia de SSMS.
    2. En el cuadro de diálogo **Conectar con el servidor**, especifique el nombre completo del servidor (por ejemplo, *myserver123.database.windows.net*) y escriba el nombre de usuario del administrador y la contraseña que especificó al crear el servidor.
    3. Haga clic en **Opciones >>** y seleccione la pestaña **Propiedades de conexión**. Asegúrese de seleccionar la base de datos **ContosoHR** (no la base de datos maestra predeterminada). 
    4. Seleccione la pestaña **Always Encrypted**.
    5. Asegúrese de que la casilla **Habilitar Always Encrypted (cifrado de columna)** esté activada.
    6. Especifique la dirección URL de atestación de enclave que ha obtenido siguiendo los pasos descritos en [Paso 2: Configuración de un proveedor de atestación](#step-2-configure-an-attestation-provider). Vea la siguiente captura de pantalla.

        ![Conexión con atestación](media/always-encrypted-enclaves/connect-to-server-configure-attestation.png)

    7. Seleccione **Conectar**.
    8. Si se le pide que habilite la parametrización para las consultas Always Encrypted, haga clic en **Habilitar**.



1. Con la misma instancia de SSMS (con Always Encrypted habilitado), abra una nueva ventana de consulta y cifre las columnas **SSN** y **Salario** mediante la ejecución de las instrucciones siguientes.

    ```sql
    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Observe la instrucción ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE para borrar la memoria caché del plan de consulta para la base de datos en el script anterior. Después de modificar la tabla, deberá borrar los planes de todos los lotes y procedimientos almacenados que tengan acceso a la tabla con el fin de actualizar la información de cifrado de los parámetros. 

1. Para comprobar que las columnas **SSN** y **Salario** ahora están cifradas, abra una nueva ventana de consulta en la instancia de SSMS **sin** Always Encrypted habilitado para la conexión de base de datos y ejecute la siguiente instrucción. La ventana de consulta debe devolver valores cifrados de las columnas **SSN** y **Salario**. Si ejecuta la misma consulta mediante la instancia SSMS con Always Encrypted habilitado, verá los datos descifrados.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-6-run-rich-queries-against-encrypted-columns"></a>Paso 6: Ejecutar consultas completas sobre columnas cifradas

Puede ejecutar consultas completas sobre columnas cifradas. Se realizará algún procesamiento de consulta dentro del enclave del lado del servidor. 

1. En la instancia de SSMS **con** Always Encrypted habilitado, asegúrese de que también está habilitada la parametrización para Always Encrypted.
    1. Seleccione **Herramientas** en el menú principal de SSMS.
    2. Seleccione **Opciones...** .
    3. Vaya a **Ejecución de consulta** > **SQL Server** > **Avanzadas**.
    4. Asegúrese de que la opción **Habilitar parametrización para Always Encrypted** esté activada.
    5. Seleccione **Aceptar**.
2. Abra una nueva ventana de consulta y pegue y ejecute la siguiente consulta. La consulta debe devolver los valores de texto no cifrado y las filas que cumplan los criterios de búsqueda especificados.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Pruebe de nuevo la misma consulta en la instancia de SSMS que no tiene habilitado Always Encrypted. Debe producirse un error.
 
## <a name="next-steps"></a>Pasos siguientes

Después de completar este tutorial, puede ir a uno de los siguientes tutoriales:
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](/sql/connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps)
- [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](/sql/relational-databases/security/tutorial-always-encrypted-enclaves-develop-net-framework-apps)
- [Tutorial: Creación y uso de índices en columnas basadas en enclave mediante cifrado aleatorio](/sql/relational-databases/security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption)

## <a name="see-also"></a>Consulte también

- [Configuración y uso de Always Encrypted con enclaves seguros](/sql/relational-databases/security/encryption/configure-always-encrypted-enclaves)
