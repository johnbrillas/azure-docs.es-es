---
title: Publicación automatizada para la integración y entrega continuas
description: Obtenga información sobre cómo publicar de forma automática la integración y la entrega continuas.
ms.service: data-factory
author: nabhishek
ms.author: abnarain
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: b5becd8ddaf74ab2acd059054a095ce9d21c178f
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100366847"
---
# <a name="automated-publishing-for-continuous-integration-and-delivery"></a>Publicación automatizada para la integración y entrega continuas

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Información general

La integración continua es la práctica de probar automáticamente cada cambio realizado en el código base y, tan pronto como sea posible, la entrega continua sigue las pruebas que se producen durante la integración continua y envía los cambios a un sistema de almacenamiento provisional o de producción.

En Azure Data Factory, la integración y la entrega continuas (CI/CD) implican el traslado de canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro. Azure Data Factory usa las [plantillas de Azure Resource Manager](../azure-resource-manager/templates/overview.md) para almacenar la configuración de las distintas entidades de ADF (canalizaciones, conjuntos de datos, flujos de datos, etc.). Se sugieren dos métodos para promover una factoría de datos a otro entorno:

- Implementación automatizada mediante la integración de Data Factory con [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines).
- Carga manual de una plantilla de Resource Manager mediante la integración de la experiencia de usuario de Data Factory con Azure Resource Manager.

Para obtener más información, consulte [Integración y entrega continuas en Azure Data Factory](continuous-integration-deployment.md).

En este artículo, nos centramos en las mejoras de implementación continua y en la característica de publicación automatizada para CI/CD.

## <a name="continuous-deployment-improvements"></a>Mejoras de implementación continua

La característica "Publicación automatizada" toma las características *Validar todo* y Exportar *plantilla de Azure Resource Manager (ARM)* de la experiencia de usuario de ADF y hace que la lógica se consuma a través de un paquete de npm [@microsoft/azure-data-factory-utilities](https://www.npmjs.com/package/@microsoft/azure-data-factory-utilities) disponible públicamente. Esto le permite desencadenar mediante programación estas acciones en lugar de tener que ir a la interfaz de usuario de ADF y hacer clic en un botón. Esto proporcionará a las canalizaciones de CI/CD una experiencia de integración continua verdadera.

### <a name="current-cicd-flow"></a>Flujo de CI/CD actual

1. Cada usuario realiza cambios en sus ramas privadas.
2. La inserciones en la rama principal están prohibidas. Los usuarios deben crear una solicitud de incorporación de cambios a la rama principal para realizar cambios.
3. Los usuarios deben cargar la interfaz de usuario de ADF y hacer clic en Publicar para implementar los cambios en Data Factory y generar las plantillas de ARM en la rama de publicación.
4. La canalización de versión de DevOps está configurada para crear una nueva versión e implementar la plantilla de ARM cada vez que se inserta un cambio nuevo en la rama de publicación.

![Flujo de CI/CD actual](media/continuous-integration-deployment-improvements/current-ci-cd-flow.png)

### <a name="manual-step"></a>Paso manual

En el flujo de CI/CD actual, la experiencia de usuario es el intermediario para crear la plantilla de ARM, por lo que un usuario debe ir a la interfaz de usuario de ADF y hacer clic manualmente en Publicar para iniciar la generación de la plantilla de ARM y colocarla en la rama de publicación, que es parte de un truco.

### <a name="the-new-cicd-flow"></a>Nuevo flujo de CI/CD

1. Cada usuario realiza cambios en sus ramas privadas.
2. La inserciones en la rama principal están prohibidas. Los usuarios deben crear una solicitud de incorporación de cambios a la rama principal para realizar cambios.
3. **La compilación de canalización de Azure DevOps se desencadena cada vez que se realiza una nueva confirmación en la rama principal, valida los recursos y genera una plantilla de ARM como artefacto si la validación se realiza correctamente.**
4. La canalización de versión de DevOps está configurada para crear una nueva versión e implementar la plantilla de ARM cada vez que hay una nueva compilación disponible. 

![Nuevo flujo de CI/CD](media/continuous-integration-deployment-improvements/new-ci-cd-flow.png)

### <a name="what-changed"></a>¿Qué ha cambiado?

- Ahora tenemos un proceso de compilación que usa una canalización de compilación de DevOps.
- La canalización de compilación usa el paquete de npm de ADFUtilities, que validará todos los recursos y generará las plantillas de ARM (plantillas únicas y vinculadas).
- La canalización de compilación será responsable de la validación de los recursos de ADF y de la generación de la plantilla de ARM en lugar de la interfaz de usuario de ADF (botón Publicar).
- La definición de versión de DevOps consumirá ahora esta nueva canalización de compilación en lugar del artefacto de Git.

> [!NOTE]
> Puede seguir usando el mecanismo existente (rama adf_publish) o usar el nuevo flujo. Se admiten ambos. 

## <a name="package-overview"></a>Introducción al paquete

Hay dos comandos disponibles actualmente en el paquete:
- Exportación de una plantilla de Resource Manager
- Validación

### <a name="export-arm-template"></a>Exportación de una plantilla de Resource Manager

Ejecute la exportación de inicio de ejecución de npm <rootFolder> <factoryId> [outputFolder] para exportar la plantilla de ARM con los recursos de una carpeta determinada. Este comando también ejecuta una comprobación de validación antes de generar la plantilla de ARM. Aquí tiene un ejemplo:

```
npm run start export C:\DataFactories\DevDataFactory /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/DevDataFactory ArmTemplateOutput
```

- RootFolder es un campo obligatorio que representa dónde se encuentran los recursos de Data Factory.
- FactoryId es un campo obligatorio que representa el identificador de recurso de Data Factory con el formato: "/subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.DataFactory/factories/<dfName>".
- OutputFolder es un parámetro opcional que especifica la ruta de acceso relativa para guardar la plantilla de ARM generada.
 
> [!NOTE]
> La plantilla de ARM generada no se publica en la versión `Live` del generador. La implementación debe realizarse mediante una canalización de CI/CD. 
 

### <a name="validate"></a>Validación

Ejecute la validación de inicio de ejecución de npm <rootFolder> <factoryId> para validar todos los recursos de una carpeta determinada. Aquí tiene un ejemplo:
    
```
npm run start validate C:\DataFactories\DevDataFactory /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/DevDataFactory
```

- RootFolder es un campo obligatorio que representa dónde se encuentran los recursos de Data Factory.
- FactoryId es un campo obligatorio que representa el identificador de recurso de Data Factory con el formato: "/subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.DataFactory/factories/<dfName>".


## <a name="create-an-azure-pipeline"></a>Creación de una canalización de Azure

Aunque los paquetes de npm se pueden consumir de varias maneras, una de las ventajas principales su consumo a través de una [canalización de Azure](https://nam06.safelinks.protection.outlook.com/?url=https:%2F%2Fdocs.microsoft.com%2F%2Fazure%2Fdevops%2Fpipelines%2Fget-started%2Fwhat-is-azure-pipelines%3Fview%3Dazure-devops%23:~:text%3DAzure%2520Pipelines%2520is%2520a%2520cloud%2Cit%2520available%2520to%2520other%2520users.%26text%3DAzure%2520Pipelines%2520combines%2520continuous%2520integration%2Cship%2520it%2520to%2520any%2520target.&data=04%7C01%7Cabnarain%40microsoft.com%7C5f064c3d5b7049db540708d89564b0bc%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C637423607000268277%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=jo%2BkIvSBiz6f%2B7kmgqDN27TUWc6YoDanOxL9oraAbmA%3D&reserved=0). En cada fusión mediante combinación en la rama de colaboración, se puede desencadenar una canalización que primero valide todo el código y, a continuación, exporte la plantilla de ARM a un [artefacto de compilación](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2F%2Fazure%2Fdevops%2Fpipelines%2Fartifacts%2Fbuild-artifacts%3Fview%3Dazure-devops%26tabs%3Dyaml%23how-do-i-consume-artifacts&data=04%7C01%7Cabnarain%40microsoft.com%7C5f064c3d5b7049db540708d89564b0bc%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C637423607000278113%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=dN3t%2BF%2Fzbec4F28hJqigGANvvedQoQ6npzegTAwTp1A%3D&reserved=0), que una canalización de versión puede consumir. **La diferencia entre el proceso actual de CI/CD es que apuntará a la canalización de versión en este artefacto, en lugar de a la rama de `adf_publish` existente.**

Siga los pasos que se indican a continuación para empezar:

1.  Abra un proyecto de Azure DevOps y vaya a "Canalizaciones". Seleccione "Nueva canalización".

    ![Nueva canalización](media/continuous-integration-deployment-improvements/new-pipeline.png)
    
2.  Seleccione el repositorio en el que quiere guardar el script de YAML de la canalización. Se recomienda guardarlo en una carpeta de *compilación* dentro del mismo repositorio de los recursos de ADF. Asegúrese de que hay un archivo **package.json** en el repositorio que contiene el nombre del paquete (como se muestra en el ejemplo siguiente).

    ```json
    {
        "scripts":{
            "build":"node node_modules/@microsoft/azure-data-factory-utilities/lib/index"
        },
        "dependencies":{
            "@microsoft/azure-data-factory-utilities":"^0.1.2"
        }
    } 
    ```
    
3.  Seleccione *Canalización inicial*. Si ha cargado o combinado el archivo YAML (como se muestra en el ejemplo siguiente), también puede apuntar directamente a él y editarlo. 

    ![Canalización inicial](media/continuous-integration-deployment-improvements/starter-pipeline.png) 

    ```yaml
    # Sample YAML file to validate and export an ARM template into a Build Artifact
    # Requires a package.json file located in the target repository
    
    trigger:
    - main #collaboration branch
    
    pool:
      vmImage: 'ubuntu-latest'
    
    steps:
    
    # Installs Node and the npm packages saved in your package.json file in the build
    
    - task: NodeTool@0
      inputs:
        versionSpec: '10.x'
      displayName: 'Install Node.js'
    
    - task: Npm@1
      inputs:
        command: 'install'
        verbose: true
      displayName: 'Install npm package'
    
    # Validates all of the ADF resources in the repository. You will get the same validation errors as when "Validate All" is clicked
    # Enter the appropriate subscription and name for the source factory 
    
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build validate $(Build.Repository.LocalPath) /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/yourFactoryName'
      displayName: 'Validate'
    
    # Validate and then generate the ARM template into the destination folder. Same as clicking "Publish" from UX
    # The ARM template generated is not published to the ‘Live’ version of the factory. Deployment should be done using a CI/CD pipeline. 
    
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build export $(Build.Repository.LocalPath) /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/yourFactoryName "ArmTemplate"'
      displayName: 'Validate and Generate ARM template'
    
    # Publish the Artifact to be used as a source for a release pipeline
    
    - task: PublishPipelineArtifact@1
      inputs:
        targetPath: '$(Build.Repository.LocalPath)/ArmTemplate'
        artifact: 'ArmTemplates'
        publishLocation: 'pipeline'
    ```

4.  Escriba en el código de YAML. Se recomienda tomar el archivo YAML y usarlo como punto inicial.
5.  Guárdelo y ejecútelo. Si usa el archivo YAML, se desencadenará cada vez que se actualice la rama "principal".

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la integración y entrega continuas en Azure Data Factory: 

- [Integración y entrega continuas en Azure Data Factory](continuous-integration-deployment.md).
