---
title: Introducción a Azure Percept DK
description: Más información sobre Azure Percept DK.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 4c2ace609d67cc48d1b73bdb044e7048ebda21e7
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098339"
---
# <a name="azure-percept-dk-overview"></a>Introducción a Azure Percept DK

Azure Percept DK es un kit de desarrollo de inteligencia artificial e IoT perimetral diseñado para desarrollar la prueba de conceptos de la inteligencia artificial para visión y audio. Cuando se combina con [Azure Percept Studio](./overview-azure-percept-studio.md) y [Azure Percept Audio](./overview-azure-percept-audio.md), se convierte en una plataforma eficaz y fácil de usar para crear soluciones de inteligencia artificial perimetral para una amplia gama de aplicaciones de inteligencia artificial para visión y audio. Se puede adquirir en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).

:::image type="content" source="./media/overview-azure-percept-dk/dk-image.png" alt-text="Dispositivo Azure Percept DK.":::

## <a name="key-features"></a>Principales características

- **La capacidad de ejecutar inteligencia artificial en el perímetro**. Con la aceleración de hardware integrada, puede ejecutar modelos de inteligencia artificial de visión sin conexión a la nube.
- **Seguridad de la raíz de confianza del hardware integrada**. Consulte esta introducción sobre la [seguridad de Azure Percept](./overview-percept-security.md) para más información.
- **Integración perfecta con [Azure Percept Studio](./overview-azure-percept-studio.md)** y otros servicios de Azure, como, por ejemplo, Azure IoT Hub, Azure Cognitive Services y [Live Video Analytics](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/overview)
- **Integración perfecta con un dispositivo [Azure Percept Audio](./overview-azure-percept-audio.md) opcional.**
- **Compatibilidad con las plataformas de inteligencia artificial más importantes**, como ONNX y TensorFlow.
- **Integración con el sistema de raíl 80/20**. Facilita la creación de prototipos en entornos de producción. Más información acerca de la [integración 80/20](./overview-8020-integration.md).

## <a name="hardware-components"></a>Componentes de hardware

- La placa base de Azure Percept DK
    - Procesador NXP iMX8m
    - Módulo de plataforma segura (TPM), versión 2.0
    - Conectividad por Wi-Fi y Bluetooth
    - Consulte la [ficha técnica](./azure-percept-dk-datasheet.md) completa.
- Módulo de sistema Azure Percept Vision
    - Unidad de procesamiento de visión de Intel Movidius Myriad X (MA2085)
    - Sensor de cámara RGB con la posibilidad de agregar un segundo sensor
    - Consulte la [ficha técnica](./azure-percept-vision-datasheet.md) completa.

## <a name="get-started-with-the-azure-percept-dk"></a>Primeros pasos con Azure Percept DK

- Complete estas guías de inicio rápido
    - [Desempaquetado y ensamblaje de Azure Percept DK](./quickstart-percept-dk-unboxing.md)
    - [Instalación de Azure Percept DK y ejecución del primer modelo de IA de visión](./quickstart-percept-dk-set-up.md)
- Comience a crear pruebas de conceptos con estos tutoriales
    - [Creación de una solución de visión sin código en Azure Percept Studio](./tutorial-nocode-vision.md)
    - [Creación de un asistente para voz en Azure Percept Studio](./tutorial-no-code-speech.md)

## <a name="next-steps"></a>Pasos siguientes

Pida un dispositivo Azure Percept DK en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).
