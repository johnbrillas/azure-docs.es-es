---
title: Visualización de la secuencia de vídeo RTSP de Azure Percept DK
description: Aprenda a ver la secuencia de vídeo RTSP desde el módulo de sistema de visión de Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 20fb8495e17d4294351a50c3bc97436de9f03450
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660564"
---
# <a name="view-your-azure-percept-dks-rtsp-video-stream"></a>Visualización de la secuencia de vídeo RTSP de Azure Percept DK

Siga esta guía para ver la secuencia de vídeo RTSP desde el módulo de sistema de visión de Azure Percept DK en Azure Percept Studio. La inferencia de los modelos de inteligencia artificial de visión implementados en el dispositivo se podrá ver en el flujo de web.

## <a name="prerequisites"></a>Prerrequisitos

- Kit de desarrollo Azure Percept DK
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el kit de desarrollo a una red Wi-Fi, creado una instancia de IoT Hub y conectado el kit a esta instancia.

## <a name="view-the-rtsp-video-stream"></a>Visualización de la secuencia de vídeo RTSP

1. Conecte el kit de desarrollo.

1. Vaya a [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. En el lado izquierdo de la página de información general, haga clic en **Dispositivos**.

    :::image type="content" source="./media/how-to-view-video-stream/overview-devices-inline.png" alt-text="Pantalla de información general de Azure Percept Studio." lightbox="./media/how-to-view-video-stream/overview-devices.png":::

1. Seleccione el kit de desarrollo de la lista.

    :::image type="content" source="./media/how-to-view-video-stream/select-device.png" alt-text="Pantalla de información general de Azure Percept Studio.":::

1. Haga clic en **Ver el flujo del dispositivo**.

    :::image type="content" source="./media/how-to-view-video-stream/view-device-stream.png" alt-text="Pantalla de información general de Azure Percept Studio.":::

    Se abrirá una pestaña independiente que muestra el flujo de web en directo desde el módulo de sistema de visión de Azure Percept DK.

    :::image type="content" source="./media/how-to-view-video-stream/webstream.png" alt-text="Pantalla de información general de Azure Percept Studio.":::

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo ver los [datos de telemetría de Azure Percept DK](./how-to-view-telemetry.md).