---
title: 'Tutorial: Implementación de la plantilla de la aplicación Video analytics - object and motion detection de Azure IoT Central'
description: 'Tutorial: en esta guía se resumen los pasos necesarios para implementar una aplicación de Azure IoT Central mediante la plantilla de aplicación de detección de movimiento y objetos de análisis de vídeo.'
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: f7f1952ab8c98553a447c3ed1955fb556ba48124
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99831950"
---
# <a name="tutorial-how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Tutorial: Implementación de una aplicación de IoT Central mediante la plantilla de la aplicación Video analytics - object and motion detection

Para información general sobre los componentes de la aplicación clave *Video analytics - object and motion detection*, consulte [Arquitectura de la aplicación Video analytics - object and motion detection](architecture-video-analytics.md).

En el siguiente vídeo se ofrece un tutorial sobre cómo usar la _plantilla de aplicación de análisis de vídeo y detección de movimiento y objetos_ para implementar una solución IoT Central:

> [!VIDEO https://www.youtube.com/embed/Bo3FziU9bSA]

## <a name="prerequisites"></a>Prerrequisitos

Se recomienda una suscripción de Azure. Como alternativa, puede usar una versión de prueba gratuita de 7 días. Si no tiene una suscripción de Azure, puede crear una en la [página de suscripción a Azure](https://aka.ms/createazuresubscription).

## <a name="deploy-the-application"></a>Implementación de la aplicación

Para implementar una aplicación de IoT Central mediante la plantilla de análisis de vídeo, complete los pasos siguientes:

1. Complete el tutorial [Creación de una aplicación de análisis de vídeo en Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) o [Creación de un análisis de vídeo en Azure IoT Central (OpenVINO&trade;)](tutorial-video-analytics-create-app-openvino.md) para:
    - Creación de una cuenta de Azure Media Services.
    - Crear la aplicación de IoT Central a partir de la plantilla de la aplicación Video analytics - object and motion detection.
    - Configurar un dispositivo de puerta de enlace a la aplicación de IoT Central. La puerta de enlace permite que los dispositivos de cámara se conecten a la aplicación.

1. Complete el tutorial [Creación de una instancia de IoT Edge para análisis de vídeo (máquina virtual Linux)](tutorial-video-analytics-iot-edge-vm.md) o [: Creación de una instancia de IoT Edge para análisis de vídeo (Intel NUC)](tutorial-video-analytics-iot-edge-nuc.md) para:
    - Crear una máquina virtual de Azure con el entorno de ejecución de Azure IoT Edge instalado. Prepare la instalación de IoT Edge para hospedar el módulo de análisis de vídeo.
    - Conecte el dispositivo de IoT Edge a la aplicación de IoT Central.

1. Complete el tutorial [Supervisión y administración de una aplicación de análisis de vídeo](tutorial-video-analytics-manage.md) para:
    - Agregar cámaras de detección de objetos y movimiento a la puerta de enlace de la aplicación de IoT Central.
    - Iniciar el procesamiento de la cámara.
    - Instalar un reproductor multimedia local para ver el vídeo capturado en AMS.
    - Ver el vídeo capturado que muestra los objetos detectados.
    - Ordenar.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la aplicación, puede eliminar todos los recursos que haya creado de la siguiente manera:

1. En la aplicación de IoT Central, vaya a la página **Your application** (Su aplicación) en la sección **Administration** (Administración). A continuación, seleccione **Eliminar**.
1. En Azure Portal, elimine el grupo de recursos **lva-rg**.
1. En el equipo local, detenga el contenedor de Docker **amp-viewer**.

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene una visión general de los pasos necesarios para implementar y usar la plantilla de aplicación de análisis de vídeo. Consulte:

> [!div class="nextstepaction"]
> [Creación de una aplicación de análisis de vídeo en Azure IoT Central (YOLO V3)](tutorial-video-analytics-create-app-yolo-v3.md) o

> [!div class="nextstepaction"]
> [Creación de un análisis de vídeo en Azure IoT Central (OpenVINO&trade;)](tutorial-video-analytics-create-app-openvino.md) para comenzar.
