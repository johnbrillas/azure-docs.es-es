---
title: Uso de PowerShell para registrar y examinar Power BI (versión preliminar)
description: Aprenda a usar PowerShell para registrar y examinar un inquilino de Power BI en Azure Purview.
author: darrenparker
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/12/2020
ms.openlocfilehash: f0b541baf49307006c18f07d92bd409763a29e3a
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550611"
---
# <a name="use-powershell-to-register-and-scan-power-bi-in-azure-purview-preview"></a>Uso de PowerShell para registrar y examinar Power BI en Azure Purview (versión preliminar) 

En este artículo se muestra cómo usar PowerShell para configurar un examen de un inquilino de Power BI en un catálogo de Azure Purview.

## <a name="power-bi-authentication-background"></a>Contexto de autenticación de Power BI

El catálogo de Purview debe conectarse a la API de administración de Power BI para examinar los artefactos de un inquilino de Power BI. Actualmente, la API de administración de Power BI admite dos tipos de autenticación:

- Identidad administrada (MSI).
- Autenticación de usuario delegada.

> [!Note]
> Se recomienda MSI, a menos que se requiera la autenticación de usuario delegada.

## <a name="create-a-security-group"></a>Creación de un grupo de seguridad

Cada catálogo de Purview tiene su propia identidad administrada asignada por el sistema a la que se debe conceder acceso al inquilino de Power BI para permitir el examen. El nombre del catálogo se puede usar para encontrar su identidad en Azure Portal.

1. En [Azure Portal](https://portal.azure.com), busque Azure Active Directory.
1. Cree un grupo de seguridad en Azure Active Directory siguiendo las indicaciones de [Creación de un grupo básico e incorporación de miembros con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

    > [!Tip]
    > Si ya tiene un grupo de seguridad para usar, puede omitir este paso.

1. Asegúrese de seleccionar Seguridad como **Tipo de grupo**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/security-group.png" alt-text="Tipo de grupo de seguridad":::

1. Agregue la identidad administrada del catálogo a este grupo de seguridad seleccionando Miembros y, luego, **+ Agregar miembros**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-group-member.png" alt-text="Incorporación de la instancia administrada del catálogo al grupo":::

1. Busque el catálogo y selecciónelo.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-catalog-to-group-by-search.png" alt-text="Búsqueda del catálogo para incorporarlo":::

1. Verá una notificación de operación correcta que le muestra que se ha agregado.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/success-add-catalog-msi.png" alt-text="Incorporación de MSI del catálogo":::

## <a name="associate-the-security-group-with-power-bi"></a>Asociación del grupo de seguridad con Power BI

1. Inicie sesión en el [portal de administración de Power BI](https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1). Anexe esta marca de característica: `allowServicePrincipalsUseReadAdminAPIsUI=1`. Esta marca habilita la característica que le permite asociar el grupo de seguridad. Por ejemplo,

    ```http
    https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1`
    ```

    > [!Important]
    > Debe ser administrador de Power BI para ver la página de configuración de inquilinos.

1. Seleccione **Configuración de desarrollador** > **Allow service principals to use read-only Power BI APIs (Preview)** (Permitir que las entidades de servicio usen API de Power BI de solo lectura [versión preliminar]).
1. Seleccione **Grupos de seguridad específicos**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/allow-service-principals-power-bi-admin.png" alt-text="Imagen que muestra cómo permitir a las entidades de servicio obtener permisos de API de administración de Power BI de solo lectura ":::

    > [!Caution]
    > Cuando permite que el grupo de seguridad que ha creado (que tiene como miembro la identidad administrada del catálogo de datos) use las API de administración de Power BI de solo lectura, también le permite acceder a los metadatos (por ejemplo, los nombres de los paneles y los informes, los propietarios, las descripciones, etc.) de todos los artefactos de Power BI de este inquilino. Una vez que los metadatos se han extraído en Azure Purview, los permisos de Purview, no los permisos de Power BI, determinan quién puede ver esos metadatos.

    > [!Note]
    > Puede quitar el grupo de seguridad de la configuración del desarrollador, pero los metadatos extraídos previamente no se quitarán de la cuenta de Purview. Puede eliminarlos por separado.

## <a name="register-power-bi-and-set-up-a-scan"></a>Registro de Power BI y configuración de un examen

Ahora que le han concedido los permisos del catálogo para conectarse a la API de administración de su inquilino de Power BI, debe configurar el examen en el catálogo. Para ello, configure y ejecute un script de PowerShell.

1. Descargue y extraiga los cmdlets de PowerShell de ADC.
1. Configure el script proporcionando valores para las asignaciones en la parte superior.

    ```PowerShell
    #Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false #Change to true if you need a new catalog to be created
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-msi-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    If ($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId  -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI  -Tenant $azuretenantId

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIManagedInstanceMsi
    ```

    > [!Note]
    > Debe ser colaborador o propietario en la suscripción en la que se ejecutan los comandos.

1. Ejecute el siguiente script para iniciar el examen:

    ```PowerShell
    Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
    ```

## <a name="register-and-scan-power-bi"></a>Registro y examen de Power BI

El método de autenticación recomendado es MSI. Sin embargo, para examinar un inquilino de Power BI que se encuentra en un inquilino de Azure diferente al del catálogo, se usa la autenticación delegada.

Para realizar la autenticación delegada, debe tener credenciales de usuario administrador, así como credenciales de administrador de Power BI. También debe crear una aplicación de Azure y concederle los permisos Tenant.ReadAll de Power BI.

1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Registros de aplicaciones**.

1. En el panel **Registros de aplicaciones**, seleccione **+ Nuevo registro**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/new-app-registration.png" alt-text="Imagen que muestra cómo crear un registro de aplicaciones de Azure":::

1. Especifique un nombre para su aplicación.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/register-new-app.png" alt-text="Registro de una nueva aplicación":::

1. Una vez creada la aplicación, seleccione **Permisos de la API** y, luego, **+ Add a permission** (+ Agregar un permiso).

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/app-permissions.png" alt-text="Imagen que muestra cómo agregar permisos a la aplicación":::

1. Seleccione **Servicio Power BI** en **Solicitud de permisos de API**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/select-power-bi-service.png" alt-text="Imagen que muestra cómo seleccionar el servicio Power BI":::

1. Seleccione **Permisos delegados** y **Tenant.Read.All**. Seleccione **Agregar permisos**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/request-api-permissions.png" alt-text="Imagen que muestra cómo solicitar permisos de API":::

1. Seleccione **Conceder consentimiento de administrador**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/grant-admin-perms.png" alt-text="Imagen que muestra cómo conceder el consentimiento del administrador":::

1. Copie los valores de **Id. de aplicación (cliente)** e **Id. de directorio (inquilino)** .  Usará estos valores cuando configure el examen.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/copy-client-and-tenantid.png" alt-text="Imagen que muestra la copia de los identificadores del cliente y del inquilino":::

1. Configure el examen en PowerShell. El script solicitará unas credenciales. Debe tener como mínimo el rol de colaborador y el rol de administrador de orígenes de datos de Purview en la suscripción de Azure que va a usar.

    ```PowerShell
    # Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-delegated-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    # Power BI Tenant Info
    $powerBITenantIdToScan = '<Power BI Tenant ID copied from above steps>'
    $ServicePrincipalId = '<Client ID copied from above steps>'
    $UserName = '<Power BI Admin emil ex: admin@firsttomarket.onmicrosoft.com>'
    $Password = '<Power BI Admin Password>'

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    #Commands To Create catalog, Create DataSource, Create Datascan, Start Scan
    If($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId   -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI -Tenant $powerBITenantIdToScan

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIDelegated -ServicePrincipalId $ServicePrincipalId -UserName $UserName -Password $Password
    ```

1. Ejecute el examen.

      ```PowerShell
      Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
      ```

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con Azure Purview, consulte [Inicio rápido: Creación de una cuenta de Azure Purview](create-catalog-portal.md).
