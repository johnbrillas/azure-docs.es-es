---
title: Captura de imágenes para una solución de visión sin código en Azure Percept Studio
description: Aprenda a capturar imágenes con Azure Percept DK en Azure Percept Studio para una solución de visión sin código.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 44bf498af52f4d8a0d880dc1f1d5874d5b444cae
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102035541"
---
# <a name="capture-images-for-a-vision-project-in-azure-percept-studio"></a>Captura de imágenes para un proyecto de visión en Azure Percept Studio

Siga esta guía para capturar imágenes mediante el módulo de sistema de visión de Azure Percept DK para un proyecto de visión existente en Azure Percept Studio. Si aún no ha creado un proyecto de visión, consulte el [tutorial de visión sin código](./tutorial-nocode-vision.md).

## <a name="prerequisites"></a>Requisitos previos

- Kit de desarrollo Azure Percept DK
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el dispositivo DevKit a una red Wi-Fi, creado una instancia de IoT Hub y conectado el dispositivo DevKit a esa instancia.
- [Proyecto de visión sin código](./tutorial-nocode-vision.md)

## <a name="capture-images"></a>Captura de imágenes

1. Encienda el dispositivo DevKit.

1. Vaya a [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. En el lado izquierdo de la página de información general, haga clic en **Dispositivos**.

    :::image type="content" source="./media/how-to-capture-images/overview-devices-inline.png" alt-text="Pantalla de información general de Azure Percept Studio." lightbox="./media/how-to-capture-images/overview-devices.png":::

1. Seleccione el dispositivo DevKit de la lista.

    :::image type="content" source="./media/how-to-capture-images/select-device.png" alt-text="Lista de dispositivos Percept.":::

1. En la página del dispositivo, haga clic en **Capture images for a project** (Capturar imágenes para un proyecto).

    :::image type="content" source="./media/how-to-capture-images/capture-images.png" alt-text="Página de dispositivos Percept con las acciones disponibles enumeradas.":::

1. En la ventana **Image capture** (Captura de imagen), haga lo siguiente:

    1. En el menú desplegable **Project** (Proyecto), seleccione el proyecto de visión para el que desea recopilar imágenes.

    1. Haga clic en **View Device Stream** (Ver flujo de dispositivo) para asegurarse de que la cámara del módulo de sistema de visión está correctamente colocada.

    1. Haga clic en **Take Photo** (Tomar foto) para capturar una imagen.

    1. Como alternativa, active la casilla junto a **Automatic image capture** (Captura de imagen automática) para configurar un temporizador para la captura de imagen:

        1. Seleccione la velocidad de creación de imágenes preferida en **Capture rate** (Velocidad de captura).
        1. Seleccione el número total de imágenes que quiere recopilar en **Target** (Destino).

    :::image type="content" source="./media/how-to-capture-images/take-photo.png" alt-text="Pantalla Captura de imagen.":::

Todas las imágenes serán accesibles en [Custom Vision](https://www.customvision.ai/).

## <a name="next-steps"></a>Pasos siguientes

[Pruebe y vuelva a entrenar su modelo de visión sin código](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/test-your-model).