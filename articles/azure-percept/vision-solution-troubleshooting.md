---
title: Solución de problemas de Azure Percept Vision y de los módulos de visión
description: Vea sugerencias para solucionar algunos de los problemas más comunes que se encuentran en las experiencias de creación de prototipos de inteligencia artificial de visión.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: e7351079e7325aa7750dc0d10f0923bc847ccc3c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678570"
---
# <a name="vision-solution-troubleshooting"></a>Solución de problemas de las soluciones de visión

Consulte la siguiente guía para obtener información sobre cómo solucionar problemas de las soluciones de visión sin código en Azure Percept Studio.

## <a name="delete-a-vision-project"></a>Eliminación de un proyecto de visión

1. Ir a https://www.customvision.ai/projects.

1. Mantenga el mouse sobre el proyecto que desea eliminar y haga clic en el icono de la papelera para eliminar el proyecto.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-project.png" alt-text="Página Proyectos de Custom Vision con el icono Eliminar resaltado.":::

## <a name="check-which-modules-are-on-a-device"></a>Comprobación de qué módulos hay en un dispositivo

1. Vaya a [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home).

1. Haga clic en el icono de **IoT Hub**.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub-2-inline.png" alt-text="Página principal de Azure Portal con el icono de IoT Hub resaltado." lightbox= "./media/vision-solution-troubleshooting/vision-iot-hub-2.png":::

1. Seleccione el icono de IoT Hub al que está conectado el dispositivo de destino.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub.png" alt-text="Lista de instancias de IoT Hub.":::

1. Seleccione **IoT Edge** y haga clic en el dispositivo en la pestaña **Id. de dispositivo**.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-edge.png" alt-text="Página principal de IoT Edge.":::

1. Los módulos del dispositivo aparecerán en la pestaña **Módulos**.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-device-modules-inline.png" alt-text="Página de IoT Edge del dispositivo seleccionado que muestra el contenido de la pestaña Módulos." lightbox= "./media/vision-solution-troubleshooting/vision-device-modules.png":::

## <a name="delete-a-device"></a>Eliminar un dispositivo

1. Vaya a [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home).

1. Haga clic en el icono de **IoT Hub**.

1. Seleccione el icono de IoT Hub al que está conectado el dispositivo de destino.

1. Seleccione **IoT Edge** y active la casilla situada junto al identificador del dispositivo de destino. Haga clic en el icono de la papelera para eliminar el dispositivo.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-device.png" alt-text="Icono Eliminar resaltado en la página principal de IoT Edge.":::

## <a name="eye-module-troubleshooting-tips"></a>Sugerencias para la solución de problemas del módulo de visión

Si hay un problema con **WebStreamModule**, asegúrese de que **azureeyemodule**, que realiza la inferencia de modelos de visión, se esté ejecutando. Para comprobar el estado del entorno de ejecución, vaya a [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) y a **Todos los recursos** ->  **\<your IoT hub>**  -> **IoT Edge** ->  **\<your device ID>** . Haga clic en la pestaña **Módulos** para ver el estado del entorno de ejecución de todos los módulos instalados.

:::image type="content" source="./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page-inline.png" alt-text="Pantalla de estado del entorno de ejecución del módulo del dispositivo." lightbox= "./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page.png":::

Si el estado del entorno de ejecución de **azureeyemodule** no aparece como **En ejecución**, haga clic en **Establecer módulos** -> **azureeyemodule**. En la página **Configuración del módulo**, establezca **Estado deseado** como **En ejecución** y haga clic en **Actualizar**.

 :::image type="content" source="./media/vision-solution-troubleshooting/firmware-desired-status-stopped.png" alt-text="Pantalla de configuración de opciones del módulo.":::

## <a name="view-device-rtsp-video-stream"></a>Visualización de la secuencia de vídeo RTSP del dispositivo

Vea la secuencia de vídeo RTSP de su dispositivo en [Azure Percept Studio](./how-to-view-video-stream.md) o [VLC Media Player](https://www.videolan.org/vlc/index.html).

Para abrir la secuencia RTSP en VLC Media Player, vaya a **Media (Medios)**  -> **Open network stream (Abrir secuencia de red)**  -> **rtsp://[dirección IP del dispositivo]/result**.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [guía general de solución de problemas](./troubleshoot-dev-kit.md) para más información sobre la solución de problemas de Azure Percept DK.