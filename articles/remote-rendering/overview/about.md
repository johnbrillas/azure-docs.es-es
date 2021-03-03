---
title: Información
description: Introducción a Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: overview
ms.openlocfilehash: 6001fff54b86b7c18684092aad6c7fdf64f81990
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101729953"
---
# <a name="about-azure-remote-rendering"></a>Acerca de Azure Remote Rendering

*Azure Remote Rendering* (ARR) es un servicio que permite representar contenido 3D interactivo de alta calidad en la nube y transmitirlo en tiempo real a los dispositivos, como HoloLens 2.

![Diagrama que muestra un ejemplo de un motor de automóvil 3D interactivo de alta calidad.](../media/arr-engine.png)

Los dispositivos desconectados tienen una potencia de cálculo limitada para la representación de modelos complejos. Sin embargo, en muchas aplicaciones, sería inaceptable reducir la fidelidad visual de algún modo. En la captura de pantalla siguiente se compara el modelo con todos los detalles con un modelo que se ha simplificado mediante una herramienta de creación de contenido común:

![Modelo de ejemplo](./media/engine-model-decimated.png)

El modelo reducido consta de aproximadamente 200 000 triángulos (incluidos los elementos interiores detallados), frente a los más de 18 millones de triángulos del modelo original.

Para resolver este problema, *Remote Rendering* traslada la carga de trabajo de representación a las GPU de perfil alto en la nube. Un motor de gráficos hospedado en la nube representa la imagen, la codifica como una secuencia de vídeo y la transmite al dispositivo de destino.

## <a name="hybrid-rendering"></a>Representación híbrida

En la mayoría de las aplicaciones, no basta con representar solo un modelo complejo. También se necesita una interfaz de usuario personalizada para proporcionar funcionalidad al usuario. Azure Remote Rendering no obliga a usar un marco de interfaz de usuario dedicado, sino que admite *representación híbrida*. Esto significa que puede representar elementos en el dispositivo con el método que prefiera, como [MRTK](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/GettingStartedWithTheMRTK.html).

Al final de un marco, Azure Remote Rendering combina automáticamente el contenido representado localmente con la imagen remota. Incluso puede hacerlo con la oclusión correcta.

## <a name="multi-gpu-rendering"></a>Representación de varias GPU

Algunos modelos son demasiado complejos como para representarlos a velocidades de fotogramas interactivas, incluso con una GPU de perfil alto. Este es un problema común, en especial en la visualización industrial. Para aumentar el límite aún más, Azure Remote Rendering puede distribuir la carga de trabajo a varias GPU. Los resultados se combinan en una sola imagen, lo que hace que el proceso sea completamente transparente para el usuario.

## <a name="high-level-architecture"></a>Arquitectura de alto nivel

En este diagrama se ilustra la arquitectura de representación remota:

![Architecture](./media/arr-high-level-architecture.png)

Un ciclo completo de generación de imágenes consta de los siguientes pasos:

1. Lado cliente: Configuración de fotogramas
    1. El código: se procesa la entrada del usuario, se actualiza el gráfico de escena.
    1. Código ARR: el gráfico de escena se actualiza y la posición de la cabeza prevista se envía al servidor.
1. Lado servidor: Representación remota
    1. El motor de representación distribuye la representación entre las GPU disponibles.
    1. La salida de varias GPU consta de una sola imagen.
    1. La imagen se codifica como secuencia de vídeo y se devuelve al cliente.
1. Lado cliente: Finalización
    1. El código: se representa el contenido local opcional (interfaz de usuario, marcadores, etc.)
    1. Código ARR: al "presentarlo", el contenido representado localmente se combina automáticamente con la secuencia de vídeo

La latencia de red es el problema principal. El tiempo de respuesta entre el envío de una solicitud y la recepción del resultado suele ser demasiado largo para las velocidades de fotogramas interactivas. Por lo tanto, puede haber más de un fotograma en tránsito en cualquier momento.

## <a name="next-steps"></a>Pasos siguientes

* [Requisitos del sistema](system-requirements.md)
* [Inicio rápido: Representación de un modelo con Unity](../quickstarts/render-model.md)
