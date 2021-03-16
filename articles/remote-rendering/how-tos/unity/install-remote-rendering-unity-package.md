---
title: Instalación del paquete de Remote Rendering para Unity
description: Explica cómo instalar los archivos DLL de cliente de Remote Rendering para Unity
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 9454bef52798650fc431f8df994e1a964662b453
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101720835"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Instalación del paquete de Remote Rendering para Unity

Azure Remote Rendering usa un paquete Unity para encapsular la integración en Unity.
Este paquete contiene la API C# completa, así como todos los archivos binarios de complemento necesarios para usar Azure Remote Rendering con Unity.
En el siguiente esquema de nomenclatura de Unity para los paquetes, el paquete se denomina **com.microsoft.azure.remote-rendering**.

Puede elegir una de las opciones siguientes para instalar el paquete de Unity.

## <a name="install-remote-rendering-package-using-the-mixed-reality-feature-tool"></a>Instalar un paquete de Remote Rendering con la herramienta de características de Mixed Reality

La [herramienta de características de Mixed Reality](https://aka.ms/MRFeatureToolDocs) ([descargar](https://aka.ms/mrfeaturetool)) es una herramienta que se usa para integrar los paquetes de características de Mixed Reality en proyectos de Unity. No forma parte del repositorio de ejemplos de [ARR](https://github.com/Azure/azure-remote-rendering) y no está disponible en el registro de paquetes internos de Unity.

Para agregar el paquete a un proyecto, debe:
1. [Descargar la herramienta de características de Mixed Reality](https://aka.ms/mrfeaturetool)
1. Seguir las [instrucciones completas](https://aka.ms/MRFeatureToolDocs) sobre cómo usar la herramienta.
1. En la página **Discover Features** (Detectar características), marque el cuadro correspondiente al paquete de **Microsoft Azure Remote Rendering** y seleccione la versión del paquete que desea agregar al proyecto.

![Mixed_Reality_feature_tool_package](media/mixed-reality-feature-tool-package.png)

Para actualizar el paquete local, solo tiene que seleccionar una versión más reciente de la herramienta de características de Mixed Reality e instalarla. La actualización del paquete puede provocar errores ocasionales en la consola. En ese caso, prueba a cerrar y volver a abrir el proyecto.

## <a name="install-remote-rendering-package-manually"></a>Instalación manual del paquete de Remote Rendering

Para instalar manualmente el paquete de Remote Rendering, debe hacer lo siguiente:

1. Descargue el paquete desde la fuente NPM de paquetes de Mixed Reality en `https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry`.
    * Puede usar [NPM](https://www.npmjs.com/get-npm) y ejecutar el siguiente comando para descargar el paquete en la carpeta actual.
      ```
      npm pack com.microsoft.azure.remote-rendering --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry
      ```

    * También puede usar el script de PowerShell en `Scripts/DownloadUnityPackages.ps1` desde el [repositorio de GitHub azure-remote-rendering](https://github.com/Azure/azure-remote-rendering).
        * Edite los contenidos de `Scripts/unity_sample_dependencies.json` a
          ```json
          {
            "packages": [
              {
                "name": "com.microsoft.azure.remote-rendering", 
                "version": "latest", 
                "registry": "https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry"
              }
            ]
          }
          ```

        * Ejecute el siguiente comando en PowerShell para descargar el paquete en el directorio de destino proporcionado.
          ```
          DownloadUnityPackages.ps1 -DownloadDestDir <destination directory>
          ```

1. [Instale el paquete descargado](https://docs.unity3d.com/Manual/upm-ui-tarball.html) con el administrador de paquetes de Unity.

Para actualizar el paquete local, solo tiene que volver a ejecutar el comando respectivo que usó y volver a importar el paquete. La actualización del paquete puede provocar errores ocasionales en la consola. En ese caso, prueba a cerrar y volver a abrir el proyecto.

## <a name="unity-render-pipelines"></a>Canalizaciones de representación de Unity

Remote Rendering funciona tanto con **:::no-loc text="Universal render pipeline":::** como con **:::no-loc text="Standard render pipeline":::** . Por motivos de rendimiento, se recomienda usar la canalización de representación universal.

Para usar **:::no-loc text="Universal render pipeline":::** , su paquete tiene que estar instalado en Unity. Esto se puede hacer en la interfaz de usuario de **Package Manager** de Unity (nombre del paquete **Universal RP**, versión 7.3.1 o más reciente) o con el archivo `Packages/manifest.json`, como se describe en el [tutorial de configuración del proyecto de Unity](../../tutorials/unity/view-remote-models/view-remote-models.md#include-the-azure-remote-rendering-package).

## <a name="next-steps"></a>Pasos siguientes

* [Interacción con componentes y objetos de juegos de Unity](objects-components.md)
* [Tutorial: Visualización de modelos remotos](../../tutorials/unity/view-remote-models/view-remote-models.md)
