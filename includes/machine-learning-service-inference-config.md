---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: a03f71adc99063fee4374b1436b08adf5bab783d
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102511101"
---
Las entidades del documento `inferenceconfig.json` se asignan a los parámetros de la clase [InferenceConfig](/python/api/azureml-core/azureml.core.model.inferenceconfig). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Description |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Ruta de acceso al archivo local que contiene el código que se ejecuta para la imagen. |
| `sourceDirectory` | `source_directory` | Opcional. Ruta de acceso a carpetas que contienen todos los archivos para crear la imagen, lo que facilita el acceso a los archivos de esta carpeta o subcarpeta. Puede cargar una carpeta completa desde la máquina local como dependencias para el servicio web. Nota: Las rutas de acceso entry_script, conda_file y extra_docker_file_steps son rutas de acceso relativas a la ruta de acceso source_directory. |
| `environment` | `environment` | Opcional.  [Entorno](/python/api/azureml-core/azureml.core.environment.environment) de Azure Machine Learning.|

Puede incluir especificaciones completas de un [entorno](/python/api/azureml-core/azureml.core.environment.environment) de Azure Machine Learning en el archivo de configuración de inferencia. Si este entorno no existe en el área de trabajo, Azure Machine Learning lo creará. De lo contrario, Azure Machine Learning actualizará el entorno si es necesario. El siguiente código JSON es un ejemplo:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "enabled": false,
            "sharedVolumes": true,
            "shmSize": null
        },
        "environmentVariables": {
            "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
        },
        "name": "my-deploy-env",
        "python": {
            "baseCondaEnvironment": null,
            "condaDependencies": {
                "channels": [
                    "conda-forge"
                ],
                "dependencies": [
                    "python=3.6.2",
                    {
                        "pip": [
                            "azureml-defaults",
                            "azureml-telemetry",
                            "scikit-learn==0.22.1",
                            "inference-schema[numpy-support]"
                        ]
                    }
                ],
                "name": "project_environment"
            },
            "condaDependenciesFile": null,
            "interpreterPath": "python",
            "userManagedDependencies": false
        },
        "version": "1"
    }
}
```

También puede usar un [entorno](/python/api/azureml-core/azureml.core.environment.environment) de Azure Machine Learning existente en parámetros de la CLI distintos y quitar la clave de "entorno" del archivo de configuración de inferencia. Use -e como nombre del entorno y --ev como versión del entorno. Si no especifica --ev, se usará la versión más reciente. Este es un ejemplo de un archivo de configuración de inferencia:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

El siguiente comando muestra cómo implementar un modelo mediante el archivo de configuración de inferencia anterior (denominado myInferenceConfig.json). 

También usa la versión más reciente de un [entorno](/python/api/azureml-core/azureml.core.environment.environment) existente de Azure Machine Learning (llamado AzureML-Minimal).

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```