---
title: Detección de movimiento y objetos del análisis de vídeo de Azure IoT Central | Microsoft Docs
description: Aprenda a compilar una aplicación de IoT Central mediante la plantilla de la aplicación Video analytics - object and motion detection en IoT Central. Esta plantilla usa análisis de vídeo en directo y cámaras conectadas.
author: KishorIoT
ms.author: nandab
ms.date: 07/27/2020
ms.topic: conceptual
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
ms.openlocfilehash: 48808f762536390287bae40e8af3849da20b81c2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91874295"
---
# <a name="video-analytics---object-and-motion-detection-application-architecture"></a>Arquitectura de la aplicación Video analytics - object and motion detection

La plantilla de la aplicación de **detección de objetos y movimiento de Video Analytics** permite compilar soluciones de IoT entre las que se incluyen las funcionalidades de análisis de vídeo en directo.

:::image type="content" source="media/architecture-video-analytics/architecture.png" alt-text="Diagrama de información general sobre los componentes de detección de movimiento y objetos de análisis de vídeo.":::

Los componentes clave de la solución de análisis de vídeo incluyen:

## <a name="live-video-analytics-lva"></a>Live Video Analytics (LVA)

LVA ofrece una plataforma para compilar aplicaciones de vídeo inteligentes que abarquen el entorno perimetral y la nube. La plataforma permite compilar aplicaciones de vídeo inteligentes que abarquen el perímetro y la nube. La plataforma ofrece la funcionalidad de capturar, grabar, analizar vídeo en directo y publicar los resultados, que podrían ser análisis de vídeo o vídeo, en los servicios de Azure. Los servicios de Azure podrían ejecutarse en la nube o en el dispositivo perimetral. La plataforma se puede usar para mejorar las soluciones de IoT con análisis de vídeo.

Para más información, consulte [Live Video Analytics](https://github.com/Azure/live-video-analytics) en GitHub.

## <a name="iot-edge-lva-gateway-module"></a>Módulo de la puerta de enlace para LVA de IoT Edge

El módulo de la puerta de enlace para LVA de IoT Edge crea instancias de las cámaras como dispositivos nuevos y los conecta directamente a IoT Central mediante el SDK de cliente de dispositivo IoT.

En esta implementación de referencia, los dispositivos se conectan a la solución mediante claves simétricas del dispositivo perimetral. Para más información sobre la conectividad de los dispositivos, consulte [Conexión a Azure IoT Central](../core/concepts-get-connected.md).

## <a name="media-graph"></a>Grafo multimedia

El grafo multimedia permite definir dónde se debe capturar el contenido multimedia, cómo procesarlo y dónde se entregan los resultados. Conecte componentes o nodos de la manera deseada para configurar este grafo multimedia. Para más información, consulte [Grafos multimedia](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph) en GitHub.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso recomendado es [Implementación de una aplicación de IoT Central mediante el análisis de vídeo: plantilla de la aplicación de detección de objetos y movimiento](tutorial-video-analytics-deploy.md).
