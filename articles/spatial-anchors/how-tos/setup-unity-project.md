---
title: Instalación de Azure Spatial Anchors para Unity
description: Configure un proyecto de Unity para que use Azure Spatial Anchors.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 2/3/2021
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 56532c17c91e6c703a6acd7990bbae47cd248165
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99576572"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>Configuración de Azure Spatial Anchors en un proyecto de Unity

En esta guía se muestra cómo empezar a trabajar con el SDK de Azure Spatial Anchors en el proyecto de Unity.

## <a name="project-requirements"></a>Requisitos de proyecto

[!INCLUDE [Unity Project Requirements](../../../includes/spatial-anchors-unity-project-requirements.md)]

## <a name="configuring-a-project"></a>Configuración de un proyecto

Antes de incluir el SDK de Azure Spatial Anchors en el proyecto de Unity, asegúrese de instalar los paquetes [necesarios](#project-requirements) mediante el administrador de paquetes de Unity.

### <a name="download-asa-packages"></a>Descarga de paquetes de ASA
[!INCLUDE [Download Unity Packages](../../../includes/spatial-anchors-unity-download-packages.md)]

### <a name="import-asa-packages"></a>Importación de paquetes de ASA
[!INCLUDE [Import Unity Packages](../../../includes/spatial-anchors-unity-import-packages.md)]

### <a name="android-only-configure-the-maintemplategradle-file"></a>Solo Android: configuración del archivo mainTemplate.gradle

1. Vaya a **Edit** > **Project Settings** > **Player** (Editar > Configuración del proyecto > Reproductor).
2. En **Inspector Panel** (Panel de inspector) de **Player Settings** (Configuración del Reproductor), seleccione el icono de **Android**.
3. En la sección **Build**, active la casilla **Custom Main Gradle Template** (Plantilla principal de Gradle personalizada) para generar una plantilla personalizada de Gradle en `Assets\Plugins\Android\mainTemplate.gradle`.
4. Abre el archivo `mainTemplate.gradle` en un editor de texto.
5. En la sección `dependencies`, pegue las siguientes dependencias:

    ```gradle
    implementation('com.squareup.okhttp3:okhttp:[3.11.0]')
    implementation('com.microsoft.appcenter:appcenter-analytics:[1.10.0]')
    ```

Cuando haya terminado, la sección `dependencies` debe tener un aspecto similar al siguiente:

[!code-gradle[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-android-gradle-setup.md?range=9-13&highlight=3-4)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Creación y localización de delimitadores en Unity](./create-locate-anchors-unity.md)
