---
title: Solución de problemas de imágenes del entorno
titleSuffix: Azure Machine Learning
description: Aprenda a solucionar problemas con las compilaciones de imágenes del entorno y las instalaciones de paquetes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: saachigopal
ms.author: sagopal
ms.date: 12/3/2020
ms.topic: troubleshooting
ms.custom: devx-track-python
ms.openlocfilehash: b452c24b4b2ed6021910f267b9941f3829acccd8
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733237"
---
# <a name="troubleshoot-environment-image-builds"></a>Solución de problemas en las compilaciones de imágenes
Aprenda a solucionar problemas con las compilaciones de imágenes del entorno de Docker y las instalaciones de paquetes.

## <a name="prerequisites"></a>Prerrequisitos

* Una **suscripción de Azure**. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* El [SDK de Azure Machine Learning](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* La[CLI de Azure](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* La [extensión de la CLI para Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Para depurar localmente, debe tener una instalación de Docker en funcionamiento en el sistema local.

## <a name="docker-image-build-failures"></a>Error en la compilación de imágenes de Docker
 
Para la mayoría de los errores de compilación de imágenes, la causa principal puede encontrarse en el mismo registro de compilación.
Puede encontrar el registro de compilación de la imagen en el portal de Azure Machine Learning (20\_image\_build\_log.txt) o en los registros de ejecución de las tareas de ACR.
 
En la mayoría de los casos, es más fácil reproducir los errores localmente. Compruebe el tipo de error y pruebe una de las siguientes `setuptools`:

- Instalación de la dependencia de Conda localmente `conda install suspicious-dependency==X.Y.Z`
- Instalación local de la dependencia de PIP `pip install suspicious-dependency==X.Y.Z`
- Prueba de materialización de todo el entorno `conda create -f conda-specification.yml`

> [!IMPORTANT]
> Asegúrese de que la plataforma y el intérprete del proceso local coinciden con los del remoto. 

### <a name="timeout"></a>Tiempo de espera 
 
Pueden producirse problemas de tiempo de espera con varios problemas de red:
- Bajo ancho de banda de Internet
- Problemas del servidor
- Una gran dependencia que no se puede descargar con la configuración de tiempo de espera de Conda o PIP especificada
 
Aparecerán los siguientes mensajes para indicar el problema:
 
```
('Connection broken: OSError("(104, \'ECONNRESET\')")', OSError("(104, 'ECONNRESET')"))
```
```
ReadTimeoutError("HTTPSConnectionPool(host='****', port=443): Read timed out. (read timeout=15)",)
```

Posibles soluciones:
 
- Pruebe un origen diferente para la dependencia si está disponible, como reflejos, almacenamiento de blobs u otras fuentes de Python.
- Actualice Conda o PIP. Si se usa un archivo de Docker personalizado, actualice la configuración del tiempo de espera.
- Algunas versiones de PIP tienen problemas conocidos. Considere la posibilidad de agregar una versión específica de PIP a las dependencias del entorno.

### <a name="package-not-found"></a>No se encuentra el paquete

Este es el caso más habitual en los errores de compilación de imágenes.

- No se pudo encontrar el paquete de Conda
        ```
        ResolvePackageNotFound: 
          - not-existing-conda-package
        ```

- No se encontró el paquete de PIP o la versión especificados
        ```
        ERROR: Could not find a version that satisfies the requirement invalid-pip-package (from versions: none)
        ERROR: No matching distribution found for invalid-pip-package
        ```

- Dependencia de PIP anidada incorrecta
        ```
        ERROR: No matching distribution found for bad-backage==0.0 (from good-package==1.0)
        ```

Compruebe que el paquete existe en los orígenes especificados. Use la [búsqueda de PIP](https://pip.pypa.io/en/stable/reference/pip_search/) para comprobar las dependencias de PIP.

`pip search azureml-core`

En el caso de las dependencias de Conda, use la [búsqueda de Conda](https://docs.conda.io/projects/conda/en/latest/commands/search.html).

`conda search conda-forge::numpy`

Para otras opciones:
- `pip search -h`
- `conda search -h`

#### <a name="installer-notes"></a>Notas del instalador

Asegúrese de que existe la distribución necesaria para la plataforma especificada y la versión del intérprete de Python.

En el caso de las dependencias de PIP, vaya a `https://pypi.org/project/[PROJECT NAME]/[VERSION]/#files` para ver si la versión necesaria está disponible. Por ejemplo: https://pypi.org/project/azureml-core/1.11.0/#files

En el caso de las dependencias de Conda, compruebe el paquete en el repositorio del canal.
En el caso de los canales que mantiene Anaconda, Inc., obtenga más información [aquí](https://repo.anaconda.com/pkgs/).

### <a name="pip-package-update"></a>Actualización del paquete PIP

Durante la instalación o actualización de un paquete PIP, es posible que el solucionador tenga que actualizar un paquete ya instalado para satisfacer los nuevos requisitos.
Se puede producir un error en la desinstalación por varias razones relacionadas con la versión de PIP o la manera en que se instaló la dependencia.
El escenario más común es que una dependencia instalada por Conda no se pudo desinstalar mediante PIP.
En este escenario, considere la posibilidad de desinstalar la dependencia mediante `conda remove mypackage`.

```
  Attempting uninstall: mypackage
    Found existing installation: mypackage X.Y.Z
ERROR: Cannot uninstall 'mypackage'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.
``` 
### <a name="installer-issues"></a>Problemas del instalador

Ciertas versiones del instalador tienen problemas en los solucionadores de paquetes que pueden provocar un error de compilación.

Si se usa una imagen base personalizada o un archivo dockerfile, se recomienda usar la versión 4.5.4 o superior de Conda.

Se requiere el paquete PIP para instalar las dependencias de PIP y, si no se especifica una versión en el entorno, se usará la versión más reciente.
Se recomienda usar una versión conocida de PIP para evitar problemas transitorios o cambios importantes que puedan deberse a la versión más reciente de la herramienta.

Considere la posibilidad de anclar la versión de PIP en su entorno si ve alguno de los mensajes siguientes:

`Warning: you have pip-installed dependencies in your environment file, but you do not list pip itself as one of your conda dependencies. Conda may not use the correct pip to install your packages, and they may end up in the wrong place. Please add an explicit pip dependency. I'm adding one for you, but still nagging you.`

`Pip subprocess error:
ERROR: THESE PACKAGES DO NOT MATCH THE HASHES FROM THE REQUIREMENTS FILE. If you have updated the package versions, update the hashes as well. Otherwise, examine the package contents carefully; someone may have tampered with them.`

Además, la instalación de PIP se puede bloquear en un bucle infinito si hay conflictos que no se pueden resolver en las dependencias. Si trabaja de forma local, cambie a una versión de PIP anterior a la 20.3. En un entorno de Conda creado a partir de un archivo YAML, este problema solo se verá si conda-forge es el canal de prioridad más alta. Para mitigar el problema, especifique explícitamente una versión de PIP anterior a 20.3 (20.3 o 20.2.4 u otra versión) como dependencia de Conda en el archivo de especificación de Conda.

## <a name="service-side-failures"></a>Errores en el lado de servicio

### <a name="unable-to-pull-image-from-mcraddress-could-not-be-resolved-for-container-registry"></a>No se puede extraer la imagen de MCR o la dirección no se pudo resolver para Container Registry.
Problemas posibles:
- Es posible que el nombre de la ruta de acceso al registro de contenedor no se resuelva correctamente. Compruebe que los nombres de las imágenes usan barras diagonales dobles y la dirección de las barras diagonales en los hosts de Linux y Windows es la correcta.
- Si la instancia de ACR detrás de una red virtual usa un punto de conexión privado en [una región no admitida](https://docs.microsoft.com/azure/private-link/private-link-overview#availability), configure la instancia detrás de una red virtual mediante el punto de conexión de servicio (acceso público) desde el portal y vuelva a intentarlo.
- Después de colocar la instancia de ACR detrás de una red virtual, asegúrese de que se ejecuta la [plantilla de ARM](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry). Esto permite que el área de trabajo se comunique con la instancia de ACR.

### <a name="401-error-from-workspace-acr"></a>Error 401 de la instancia de ACR del área de trabajo
Vuelva a sincronizar las claves de almacenamiento mediante [ws.sync_keys()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#sync-keys--)

### <a name="environment-keeps-throwing-waiting-for-other-conda-operations-to-finish-error"></a>El entorno sigue produciendo el error "Esperando a que finalicen otras operaciones de Conda..."
Si hay una compilación de imagen en curso, el cliente SDK bloquea Conda. Si el usuario bloqueó o canceló incorrectamente el proceso, Conda permanecerá en el estado bloqueado. Para solucionarlo, elimine manualmente el archivo de bloqueo. 

### <a name="custom-docker-image-not-in-registry"></a>La imagen personalizada de Docker no está en el registro
Compruebe si se usa la [etiqueta correcta](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments#create-an-environment) y que `user_managed_dependencies = True`. `Environment.python.user_managed_dependencies = True` deshabilita Conda y usa los paquetes instalados del usuario.

### <a name="common-vnet-issues"></a>Problemas habituales de red virtual

1. Compruebe que la cuenta de almacenamiento, el clúster de proceso y Azure Container Registry están en la misma subred de la red virtual.
2. Cuando ACR está detrás de una red virtual, no se puede usar directamente para compilar imágenes. El clúster de proceso debe usarse para compilar imágenes.
3. Es posible que sea necesario colocar almacenamiento detrás de una red virtual cuando:
    - Se usan inferencias o Private Wheel
    - Se ven errores de servicio 403 (no autorizado)
    - No se pueden obtener los detalles de la imagen de ACR o MCR

### <a name="image-build-fails-when-trying-to-access-network-protected-storage"></a>Se produce un error en la compilación de la imagen al intentar acceder al almacenamiento protegido por la red
- Las tareas de ACR no funcionan detrás de la red virtual. Si el usuario tiene la instancia de ACR detrás de la red virtual, debe usar el clúster de proceso para compilar una imagen.
- El almacenamiento debe estar detrás de la red virtual para poder extraer las dependencias de él. 

### <a name="cannot-run-experiments-when-storage-has-network-security-enabled"></a>No se pueden ejecutar experimentos cuando el almacenamiento tiene habilitada la seguridad de red
Al usar imágenes de Docker predeterminadas y habilitar las dependencias administradas por el usuario, debe usar las [etiquetas de servicio](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network) MicrosoftContainerRegistry y AzureFrontDoor.FirstParty para agregar MCR y sus dependencias a la lista de permitidos.

 Para más información, consulte cómo [habilitar VNET](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry).

### <a name="creating-an-icm"></a>Creación de un ICM

Al crear o asignar un ICM a MetaStore, incluya la incidencia de soporte de CSS para que podamos comprender mejor el problema.

## <a name="next-steps"></a>Pasos siguientes

- [Entrenamiento de un modelo de Machine Learning para clasificar flores](how-to-train-scikit-learn.md)
- [Entrenamiento de un modelo de Machine Learning con una imagen personalizada de Docker](how-to-train-with-custom-image.md)