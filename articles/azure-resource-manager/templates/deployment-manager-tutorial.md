---
title: Uso de Azure Deployment Manager para implementar plantillas
description: Aprenda a usar plantillas de Resource Manager con Azure Deployment Manager para implementar recursos de Azure.
author: mumian
ms.date: 08/25/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 95d5067eccff5c847588834061db8454f75e55d7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99627589"
---
# <a name="tutorial-use-azure-deployment-manager-with-resource-manager-templates-public-preview"></a>Tutorial: Uso de Azure Deployment Manager con plantillas de Resource Manager (versión preliminar pública)

Aprenda a usar [Azure Deployment Manager](./deployment-manager-overview.md) para implementar sus aplicaciones en varias regiones. Si prefiere un enfoque más rápido, el [inicio rápido de Azure Deployment Manager](https://github.com/Azure-Samples/adm-quickstart) crea las configuraciones necesarias en su suscripción y personaliza los artefactos para implementar una aplicación en varias regiones. El inicio rápido realiza las mismas tareas que en este tutorial.

Para usar Deployment Manager, deberá crear dos plantillas:

* **Una plantilla de topología**: describe los recursos de Azure que constituyen las aplicaciones y dónde implementarlos.
* **Una plantilla de lanzamiento**: describe los pasos que se deben seguir al implementar las aplicaciones.

> [!IMPORTANT]
> Si la suscripción está marcada para Canary para probar las nuevas características de Azure, Azure Deployment Manager solo se puede usar para realizar implementaciones en las regiones de Canary.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Descripción del escenario
> * Descarga de los archivos del tutorial
> * Preparación de los artefactos
> * Creación de la identidad administrada definida por el usuario
> * Creación de la plantilla de topología del servicio
> * Creación de la plantilla de lanzamiento
> * Implementación de las plantillas
> * Comprobar la implementación
> * Implementación de la versión más reciente
> * Limpieza de recursos

Recursos adicionales:

* [Referencia de la API REST de Azure Deployment Manager](/rest/api/deploymentmanager/).
* [Tutorial: Uso de la comprobación de estado en Azure Deployment Manager](./deployment-manager-tutorial-health-check.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* Suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Algo de experiencia con el desarrollo de [plantillas Azure Resource Manager](overview.md).
* Azure PowerShell. Para más información, consulte el artículo de [introducción a Azure PowerShell](/powershell/azure/get-started-azureps).
* Cmdlets de Deployment Manager. Para instalar estos cmdlets en versión preliminar, necesita la versión más reciente de PowerShellGet. Para obtener la versión más reciente, consulte [Instalación de PowerShellGet](/powershell/scripting/gallery/installing-psget). Después de instalar PowerShellGet, cierre la ventana de PowerShell. Abra una nueva ventana con privilegios elevados de PowerShell y ejecute el siguiente comando:

    ```powershell
    Install-Module -Name Az.DeploymentManager
    ```

## <a name="understand-the-scenario"></a>Descripción del escenario

La plantilla de topología del servicio describe los recursos de Azure que constituyen el servicio y la ubicación para implementarlos. La definición de topología del servicio tiene la siguiente jerarquía:

* Topología del servicio
  * Servicios
    * Unidades de servicio

En el diagrama siguiente se ilustra la topología de red usada en este tutorial:

![Diagrama del escenario del tutorial de Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-scenario-diagram.png)

Hay dos servicios asignados en las ubicaciones Oeste de EE. UU y Este de EE. UU. Cada servicio tiene dos unidades de servicio: un aplicación web de front-end y una cuenta de almacenamiento de back-end. Las definiciones de unidad de servicio contienen vínculos a la plantilla y los archivos de parámetros para crear las aplicaciones web y las cuentas de almacenamiento.

## <a name="download-the-tutorial-files"></a>Descarga de los archivos del tutorial

1. Descargue las [plantillas y los artefactos](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-adm/ADMTutorial.zip) usados en este tutorial.
1. Descomprima los archivos en el equipo de la ubicación.

En la carpeta raíz, hay dos carpetas:

* _ADMTemplates_: contiene las plantillas de Deployment Manager, que incluyen:
  * _CreateADMServiceTopology.json_
  * _CreateADMServiceTopology.Parameters.json_
  * _CreateADMRollout.json_
  * _CreateADMRollout.Parameters.json_
* _ArtifactStore_: contiene los artefactos de plantilla y los artefactos binarios. Consulte [Preparación de los artefactos](#prepare-the-artifacts).

Hay dos conjuntos de plantillas. Un conjunto son las plantillas de Deployment Manager que se usan para implementar la topología del servicio y el lanzamiento. Las unidades de servicio llaman al otro conjunto para crear los servicios web y las cuentas de almacenamiento.

## <a name="prepare-the-artifacts"></a>Preparación de los artefactos

La carpeta ArtifactStore de la descarga contiene dos carpetas:

![Diagrama de origen de artefacto del tutorial de Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-artifact-source-diagram.png)

* La carpeta _templates_ contiene los artefactos de plantilla. Las carpetas _1.0.0.0_ y _1.0.0.1_ representan las dos versiones de los artefactos binarios. Dentro de cada versión, hay una carpeta para cada servicio: _ServiceEUS_ (servicio en Este de EE. UU.) y _ServiceWUS_ (servicio en Oeste de EE. UU.). Cada servicio tiene un par de archivos de plantilla y parámetros: uno para crear una cuenta de almacenamiento y otro par para crear una aplicación web. La plantilla de aplicación web llama a un paquete comprimido que contiene los archivos de aplicación web. El archivo comprimido es un artefacto binario almacenado en la carpeta de archivos binarios.
* La carpeta _binaries_ contiene los artefactos binarios. Las carpetas _1.0.0.0_ y _1.0.0.1_ representan las dos versiones de los artefactos binarios. Dentro de cada versión, hay un archivo ZIP para crear la aplicación web en la ubicación Oeste de EE. UU. y otro archivo ZIP para crear la aplicación web en la ubicación Este de EE. UU.

Las dos versiones (1.0.0.0 y 1.0.0.1) son para la [implementación de revisiones](#deploy-the-revision). Aunque los artefactos de plantilla y los artefactos binarios tienen dos versiones, solo los artefactos binarios son diferentes entre las dos versiones. En la práctica, los artefactos binarios se actualizan con más frecuencia que los artefactos de plantilla.

1. Abra _\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateStorageAccount.json_ en un editor de texto. Se trata de una plantilla básica para crear una cuenta de almacenamiento.
1. Abra _\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplication.json_.

    ![Tutorial de Azure Deployment Manager: creación de una plantilla de aplicación web](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-packageuri.png)

    La plantilla llama a un paquete de implementación, que contiene los archivos de la aplicación web. En este tutorial, el paquete comprimido solo contiene un archivo _index.html_.
1. Abra _\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplicationParameters.json_.

    ![Tutorial de Azure Deployment Manager: creación de los parámetros de plantilla de aplicación web containerRoot](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-parameters-deploypackageuri.png)

    El valor de `deployPackageUri` es la ruta de acceso al paquete de implementación. El parámetro contiene una variable `$containerRoot`. El valor de `$containerRoot` se proporciona en la [plantilla de lanzamiento](#create-the-rollout-template) mediante la concatenación de la ubicación de SAS de origen del artefacto, la raíz del artefacto y el valor de `deployPackageUri`.
1. Abra _\ArtifactStore\binaries\1.0.0.0\helloWorldWebAppWUS.zip\index.html_.

    ```html
    <html>
      <head>
        <title>Azure Deployment Manager tutorial</title>
      </head>
      <body>
        <p>Hello world from west U.S.!</p>
        <p>Version 1.0.0.0</p>
      </body>
    </html>
    ```

    El código HTML muestra la ubicación y la información de versión. El archivo binario de la carpeta _1.0.0.1_ muestra _Version 1.0.0.1_. Después de implementar el servicio, puede examinar estas páginas.
1. Consulte otros archivos de artefacto. Le ayudará a comprender el mejor escenario.

Los artefactos de plantilla se usan en la plantilla de topología del servicio y los artefactos binarios se usan en la plantilla de lanzamiento. La plantilla de topología y la plantilla de lanzamiento definen un recurso de Azure de origen del artefacto, que es un recurso usado para que Resource Manager apunte a los artefactos binarios y de plantilla que se usan en la implementación. Para simplificar el tutorial, se usa una cuenta de almacenamiento para almacenar los artefactos de plantilla y los artefactos binarios. Ambos orígenes de artefacto apuntan a la misma cuenta de almacenamiento.

Ejecute el siguiente script de PowerShell para crear un grupo de recursos, cree un contenedor de almacenamiento, cree un contenedor de blobs, cargue los archivos descargados y cree un token de SAS.

> [!IMPORTANT]
> En el script de PowerShell, `projectName` se utiliza para generar nombres para los servicios de Azure que se implementan en este tutorial. Los distintos servicios de Azure tienen diferentes requisitos con respecto a los nombres. Para asegurarse de que la implementación se realice correctamente, elija un nombre con menos de 12 caracteres con solo letras minúsculas y números.
> Guarde una copia del nombre del proyecto. Use el mismo valor de `projectName` en todo el tutorial.

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$filePath = Read-Host -Prompt "Enter the folder that contains the downloaded files"


$resourceGroupName = "${projectName}rg"
$storageAccountName = "${projectName}store"
$containerName = "admfiles"
$filePathArtifacts = "${filePath}\ArtifactStore"

New-AzResourceGroup -Name $resourceGroupName -Location $location

$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2

$storageContext = $storageAccount.Context

$storageContainer = New-AzStorageContainer -Name $containerName -Context $storageContext -Permission Off


$filesToUpload = Get-ChildItem $filePathArtifacts -Recurse -File

foreach ($x in $filesToUpload) {
    $targetPath = ($x.fullname.Substring($filePathArtifacts.Length + 1)).Replace("\", "/")

    Write-Verbose "Uploading $("\" + $x.fullname.Substring($filePathArtifacts.Length + 1)) to $($storageContainer.CloudBlobContainer.Uri.AbsoluteUri + "/" + $targetPath)"
    Set-AzStorageBlobContent -File $x.fullname -Container $storageContainer.Name -Blob $targetPath -Context $storageContext | Out-Null
}

$token = New-AzStorageContainerSASToken -name $containerName -Context $storageContext -Permission rl -ExpiryTime (Get-date).AddMonths(1)

$url = $storageAccount.PrimaryEndpoints.Blob + $containerName + $token

Write-Host $url
```

Copie la dirección URL con el token de SAS. Esta dirección URL es necesaria para rellenar un campo en los dos archivos de parámetros: el archivo de parámetros de topología y el archivo de parámetros de lanzamiento.

Abra el contenedor desde Azure Portal y compruebe que se han cargado las carpetas _binaries_ y _templates_, así como los archivos.

## <a name="create-the-user-assigned-managed-identity"></a>Creación de la identidad administrada asignada por el usuario

Más adelante en el tutorial, implementará un lanzamiento. Para realizar las acciones de implementación se necesita una identidad administrada asignada por el usuario (por ejemplo, implementar las aplicaciones web y la cuenta de almacenamiento). Esta identidad debe tener acceso concedido a la suscripción de Azure en la que va a implementar el servicio y debe tener permisos suficientes para completar la implementación del artefacto.

Deberá crear una identidad administrada asignada por el usuario y configurar el control de acceso de la suscripción.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Cree una [identidad administrada asignada por el usuario](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).
1. En el portal, seleccione **Suscripciones** en el menú izquierdo y, luego, seleccione su suscripción.
1. Seleccione **Control de acceso (IAM)** y después **Agregar asignación de rol**.
1. Escriba o seleccione los siguientes valores:

    ![Tutorial de Azure Deployment Manager: control de acceso de identidad administrada asignada por el usuario](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-access-control.png)

    * **Rol**: proporcione permisos suficientes para completar la implementación de los artefactos (las aplicaciones web y las cuentas de almacenamiento). Para los fines de este tutorial, seleccione **Colaborador**. En la práctica, querrá restringir los permisos al mínimo.
    * **Asignar acceso a**: seleccione **Identidad administrada asignada por el usuario**.
    * Seleccione la identidad administrada asignada por el usuario que creó anteriormente en el tutorial.
1. Seleccione **Guardar**.

## <a name="create-the-service-topology-template"></a>Creación de la plantilla de topología del servicio

Abra _\ADMTemplates\CreateADMServiceTopology.json_.

### <a name="the-parameters"></a>Los parámetros

La plantilla contiene los parámetros siguientes:

* `projectName`: este nombre se usa para crear los nombres de los recursos del Administrador de implementaciones. Por ejemplo, si usa **demo**, el nombre de la topología del servicio será **demo** ServiceTopology. Los nombres de los recursos se definen en la sección `variables` de la plantilla.
* `azureResourcelocation`: para simplificar el tutorial, todos los recursos comparten esta ubicación, a menos que se especifique lo contrario.
* `artifactSourceSASLocation`: el URI de SAS del contenedor de blobs donde se almacenan los archivos de plantilla y parámetros de la unidad de servicio para la implementación. Consulte [Preparación de los artefactos](#prepare-the-artifacts).
* `templateArtifactRoot`: la ruta de desplazamiento del contenedor de blobs donde se almacenan las plantillas y los parámetros. El valor predeterminado es _templates/1.0.0.0_. No cambie este valor a menos que quiera cambiar la estructura de carpetas que se explica en [Preparación de los artefactos](#prepare-the-artifacts). En este tutorial, se usan rutas de acceso relativas. La ruta de acceso completa se construye mediante la concatenación de `artifactSourceSASLocation`, `templateArtifactRoot` y `templateArtifactSourceRelativePath` (o `parametersArtifactSourceRelativePath`).
* `targetSubscriptionID`: el identificador de suscripción con el que se van a implementar y facturar los recursos de Deployment Manager. Use el identificador de suscripción de este tutorial.

### <a name="the-variables"></a>Las variables

En la sección de variables se definen los nombres de los recursos, las ubicaciones de Azure de los dos servicios: `ServiceWUS` y `ServiceEUS` y las rutas de acceso de los artefactos:

![Tutorial de Azure Deployment Manager: variables de plantilla de topología](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-variables.png)

Compare las rutas de acceso de los artefactos con la estructura de carpetas que ha cargado en la cuenta de almacenamiento. Observe que las rutas de acceso de los artefactos son relativas. La ruta de acceso completa se construye mediante la concatenación de `artifactSourceSASLocation`, `templateArtifactRoot` y `templateArtifactSourceRelativePath` (o `parametersArtifactSourceRelativePath`).

### <a name="the-resources"></a>Los recursos

En el nivel raíz, se definen dos recursos: *un origen del artefacto* y *una topología del servicio*.

La definición de origen de artefacto es:

![Tutorial de Azure Deployment Manager: origen de artefacto de recursos de plantilla de topología](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-artifact-source.png)

En la captura de pantalla siguiente solo se muestran algunas partes de la topología del servicio, los servicios y las definiciones de unidades de servicio:

![Tutorial de Azure Deployment Manager: topología del servicio de recursos de plantilla de topología](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-service-topology.png)

* `artifactSourceId`: se usa para asociar el recurso de origen del artefacto con el recurso de la topología del servicio.
* `dependsOn`: todos los recursos de topología del servicio dependen del recurso de origen de artefacto.
* `artifacts`: apunta a los artefactos de la plantilla. Aquí se usan rutas de acceso relativas. La ruta de acceso completa se construye mediante la concatenación de los valores de `artifactSourceSASLocation` (definido en el origen del artefacto), `artifactRoot` (definido en el origen del artefacto) y `templateArtifactSourceRelativePath` (o `parametersArtifactSourceRelativePath`).

### <a name="topology-parameters-file"></a>Archivo de parámetros de topología

Creará un archivo de parámetros que se usa con la plantilla de la topología.

1. Abra el archivo _\ADMTemplates\CreateADMServiceTopology.Parameters.json_ en Visual Studio Code o cualquier editor de texto.
1. Escriba los valores de los parámetros:

    * `projectName`: escriba una cadena con cuatro o cinco caracteres. Este nombre se usa para crear nombres de recursos de Azure únicos.
    * `azureResourceLocation`: si no está familiarizado con las ubicaciones de Azure, use **centralus** en este tutorial.
    * `artifactSourceSASLocation`: el URI de SAS del directorio raíz (el contenedor de blobs) donde se almacenan los archivos de plantilla y parámetros de la unidad de servicio para la implementación.  Consulte [Preparación de los artefactos](#prepare-the-artifacts).
    * `templateArtifactRoot`: a menos que cambie la estructura de carpetas de los artefactos, use _templates/1.0.0.0_ en este tutorial.

> [!IMPORTANT]
> La plantilla de topología y la plantilla de lanzamiento comparten algunos parámetros comunes. Estos parámetros deben tener los mismos valores. Los parámetros son: `projectName`, `azureResourceLocation` y `artifactSourceSASLocation` (ambos orígenes de artefacto comparten la misma cuenta de almacenamiento en este tutorial).

## <a name="create-the-rollout-template"></a>Creación de la plantilla de lanzamiento

Abra _\ADMTemplates\CreateADMRollout.json_.

### <a name="the-parameters"></a>Los parámetros

La plantilla contiene los parámetros siguientes:

![Tutorial de Azure Deployment Manager: parámetros de plantilla de lanzamiento](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-parameters.png)

* `projectName`: este nombre se usa para crear los nombres de los recursos del Administrador de implementaciones. Por ejemplo, si se usa **demo**, el nombre del lanzamiento será **demo** Rollout. Los nombres se definen en la sección `variables` de la plantilla.
* `azureResourcelocation`: para simplificar el tutorial, todos los recursos de Deployment Manager comparten esta ubicación, a menos que se especifique lo contrario.
* `artifactSourceSASLocation`: el URI de SAS del directorio raíz (el contenedor de blobs) donde se almacenan los archivos de plantilla y parámetros de la unidad de servicio para la implementación. Consulte [Preparación de los artefactos](#prepare-the-artifacts).
* `binaryArtifactRoot`: el valor predeterminado es _binaries/1.0.0.0_. No cambie este valor a menos que quiera cambiar la estructura de carpetas que se explica en [Preparación de los artefactos](#prepare-the-artifacts). En este tutorial, se usan rutas de acceso relativas. La ruta de acceso completa se construye mediante la concatenación de los valores de `artifactSourceSASLocation`, `binaryArtifactRoot` y el valor de `deployPackageUri` especificado en el archivo _CreateWebApplicationParameters.json_. Consulte [Preparación de los artefactos](#prepare-the-artifacts).
* `managedIdentityID`: la identidad administrada asignada por el usuario que realiza las acciones de implementación. Consulte [Creación de la identidad administrada asignada por el usuario](#create-the-user-assigned-managed-identity).

### <a name="the-variables"></a>Las variables

En la sección `variables` se definen los nombres de los recursos. Asegúrese de que el nombre de la topología del servicio, los nombres de servicio y los nombres de unidad de servicio coincidan con los nombres definidos en la [plantilla de topología](#create-the-service-topology-template).

![Tutorial de Azure Deployment Manager: variables de plantilla de lanzamiento](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-variables.png)

### <a name="the-resources"></a>Los recursos

En el nivel raíz, hay tres recursos definidos: un origen de artefacto, un paso y un lanzamiento.

La definición del origen de artefacto es idéntica a la definida en la plantilla de topología. Para más información, consulte [Creación de la plantilla de topología de servicio](#create-the-service-topology-template).

En la siguiente captura de pantalla se muestra la definición del paso `wait`:

![Tutorial de Azure Deployment Manager: paso de espera de recursos de plantilla de lanzamiento](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-wait-step.png)

La duración usa el [estándar ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations). **PT1M** (se requieren letras mayúsculas) es un ejemplo de una espera de 1 minuto.

En la siguiente captura de pantalla solo se muestran algunas partes de la definición de lanzamiento:

![Tutorial de Azure Deployment Manager: lanzamiento de recursos de plantilla de lanzamiento](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-rollout.png)

* `dependsOn`: el recurso de lanzamiento depende el recurso de origen de artefacto y de cualquiera de los pasos definidos.
* `artifactSourceId`: se usa para asociar el recurso de origen del artefacto con el recurso de lanzamiento.
* `targetServiceTopologyId`: se usa para asociar el recurso de la topología del servicio con el recurso de lanzamiento.
* `deploymentTargetId`: es el identificador del recurso de la unidad de servicio del recurso de la topología del servicio.
* `preDeploymentSteps` y `postDeploymentSteps`: contiene los pasos de lanzamiento. En la plantilla, se llama al paso `wait`.
* `dependsOnStepGroups`: configura las dependencias entre los grupos de pasos.

### <a name="rollout-parameters-file"></a>Archivo de parámetros de lanzamiento

Creará un archivo de parámetros que se usa con la plantilla de lanzamiento.

1. Abra el archivo _\ADMTemplates\CreateADMRollout.Parameters.json_ en Visual Studio Code o en cualquier editor de texto.
1. Escriba los valores de los parámetros:

    * `projectName`: escriba una cadena con cuatro o cinco caracteres. Este nombre se usa para crear nombres de recursos de Azure únicos.
    * `azureResourceLocation`: seleccione una ubicación de Azure.
    * `artifactSourceSASLocation`: el URI de SAS del directorio raíz (el contenedor de blobs) donde se almacenan los archivos de plantilla y parámetros de la unidad de servicio para la implementación.  Consulte [Preparación de los artefactos](#prepare-the-artifacts).
    * `binaryArtifactRoot`: a menos que cambie la estructura de carpetas de los artefactos, use _binaries/1.0.0.0_ en este tutorial.
    * `managedIdentityID`: introduzca la identidad administrada asignada por el usuario. Consulte [Creación de la identidad administrada asignada por el usuario](#create-the-user-assigned-managed-identity). La sintaxis es:

        ```json
        "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userassignedidentities/<ManagedIdentityName>"
        ```

> [!IMPORTANT]
> La plantilla de topología y la plantilla de lanzamiento comparten algunos parámetros comunes. Estos parámetros deben tener los mismos valores. Los parámetros son: `projectName`, `azureResourceLocation` y `artifactSourceSASLocation` (ambos orígenes de artefacto comparten la misma cuenta de almacenamiento en este tutorial).

## <a name="deploy-the-templates"></a>Implementación de las plantillas

Azure PowerShell puede usarse para implementar las plantillas.

1. Ejecute el script para implementar la topología del servicio.

    ```azurepowershell
    # Create the service topology
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMServiceTopology.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMServiceTopology.Parameters.json"
    ```

    Si ejecuta este script desde una sesión de PowerShell diferente de la que usó para ejecutar el script de [Preparación de los artefactos](#prepare-the-artifacts), debe volver a rellenar primero las variables, que incluyen `$resourceGroupName` y `$filePath`.

    > [!NOTE]
    > `New-AzResourceGroupDeployment` es una llamada asincrónica. El mensaje de operación correcta solo significa que la implementación ha comenzado correctamente. Para comprobar la implementación, consulte los pasos 2 y 4 de este procedimiento.

1. Compruebe la topología del servicio y que los recursos subyacentes se hayan creado correctamente mediante Azure Portal:

    ![Tutorial de Azure Deployment Manager: recursos de topología del servicio implementados](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

    Se debe seleccionar **Mostrar tipos ocultos** para ver los recursos.

1. <a id="deploy-the-rollout-template"></a>Implemente la plantilla de lanzamiento:

    ```azurepowershell
    # Create the rollout
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMRollout.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMRollout.Parameters.json"
    ```

1. Compruebe el progreso del lanzamiento mediante el siguiente script de PowerShell:

    ```azurepowershell
    # Get the rollout status
    $rolloutname = "${projectName}Rollout" # "adm0925Rollout" is the rollout name used in this tutorial
    Get-AzDeploymentManagerRollout `
        -ResourceGroupName $resourceGroupName `
        -Name $rolloutName `
        -Verbose
    ```

    Antes de ejecutar este cmdlet se deben instalar los cmdlets de PowerShell de Deployment Manager. Consulte [Requisitos previos](#prerequisites). El parámetro `-Verbose` se puede utilizar para ver la salida completa.

    En el ejemplo siguiente se muestra el estado de ejecución:

    ```Output
    VERBOSE:

    Status: Succeeded
    ArtifactSourceId: /subscriptions/<AzureSubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    BuildVersion: 1.0.0.0

    Operation Info:
        Retry Attempt: 0
        Skip Succeeded: False
        Start Time: 03/05/2019 15:26:13
        End Time: 03/05/2019 15:31:26
        Total Duration: 00:05:12

    Service: adm0925ServiceEUS
        TargetLocation: EastUS
        TargetSubscriptionId: <AzureSubscriptionID>

        ServiceUnit: adm0925ServiceEUSStorage
            TargetResourceGroup: adm0925ServiceEUSrg

            Step: Deploy
                Status: Succeeded
                StepGroup: stepGroup3
                Operation Info:
                    DeploymentName: 2F535084871E43E7A7A4CE7B45BE06510adm0925ServiceEUSStorage
                    CorrelationId: 0b6f030d-7348-48ae-a578-bcd6bcafe78d
                    Start Time: 03/05/2019 15:26:32
                    End Time: 03/05/2019 15:27:41
                    Total Duration: 00:01:08
                Resource Operations:

                    Resource Operation 1:
                    Name: txq6iwnyq5xle
                    Type: Microsoft.Storage/storageAccounts
                    ProvisioningState: Succeeded
                    StatusCode: OK
                    OperationId: 64A6E6EFEF1F7755

    ...

    ResourceGroupName       : adm0925rg
    BuildVersion            : 1.0.0.0
    ArtifactSourceId        : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    TargetServiceTopologyId : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/serviceTopologies/adm0925ServiceTopology
    Status                  : Running
    TotalRetryAttempts      : 0
    OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
    Services                : {adm0925ServiceEUS, adm0925ServiceWUS}
    Name                    : adm0925Rollout
    Type                    : Microsoft.DeploymentManager/rollouts
    Location                : centralus
    Id                      : /subscriptions/<SubscriptionID>/resourcegroups/adm0925rg/providers/Microsoft.DeploymentManager/rollouts/adm0925Rollout
    Tags                    :
    ```

    Una vez que el lanzamiento se ha implementado correctamente, verá que se han creado dos grupos de recursos más, uno para cada servicio.

## <a name="verify-the-deployment"></a>Comprobar la implementación

1. Abra [Azure Portal](https://portal.azure.com).
1. Vaya a las aplicaciones web recién creadas en los nuevos grupos de recursos creados mediante la implementación del lanzamiento.
1. Abra la aplicación web en un explorador web. Compruebe la ubicación y la versión en el archivo _index.html_.

## <a name="deploy-the-revision"></a>Implementación de la revisión

Cuando tenga una nueva versión (1.0.0.1) de la aplicación web, puede usar el procedimiento siguiente para volver a implementar dicha aplicación.

1. Abra el archivo _CreateADMRollout.Parameters.json_.
1. Actualice `binaryArtifactRoot` a _binaries/1.0.0.1_.
1. Vuelva a implementar el lanzamiento como se indica en [Implementación de las plantillas](#deploy-the-rollout-template).
1. Compruebe la implementación como se indica en [Comprobación de la implementación](#verify-the-deployment). La página web muestra la versión 1.0.0.1.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
1. Use el campo **Filtrar por nombre** para limitar los grupos de recursos creados en este tutorial.

    * **&lt;projectName>rg**: contiene los recursos del Administrador de implementaciones.
    * **&lt;projectName>ServiceWUSrg**: contiene los recursos definidos por ServiceWUS.
    * **&lt;projectName>ServiceEUSrg**: contiene los recursos definidos por ServiceEUS.
    * El grupo de recursos de la identidad administrada definida por el usuario.
1. Seleccione el nombre del grupo de recursos.
1. Seleccione **Eliminar grupo de recursos** del menú superior.
1. Repita los dos últimos pasos para eliminar otros grupos de recursos creados en este tutorial.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a usar Azure Deployment Manager. Para integrar la supervisión del estado en Azure Deployment Manager, consulte [Tutorial: Uso de la comprobación de estado en Azure Deployment Manager](./deployment-manager-tutorial-health-check.md).
