---
title: Introducción al dispositivo Azure Percept Audio
description: Más información acerca de Azure Percept Audio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: a63f471498667f58fc80f89323ad93b0feb8bc95
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678828"
---
# <a name="introduction-to-azure-percept-audio"></a>Introducción a Azure Percept Audio

Azure Percept Audio es un dispositivo accesorio que agrega funcionalidades de voz con inteligencia artificial a Azure Percept DK. Contiene un acelerador de inteligencia artificial de síntesis de voz preconfigurado y una matriz lineal de cuatro micrófonos, lo que permite aplicar comandos personalizados, detección de palabras clave y reconocimiento de voz a gran distancia en dispositivos de escucha locales. Azure Percept Audio permite a los fabricantes de dispositivos ampliar las funcionalidades de visión de Azure Percept DK a nuevos dispositivos activados por voz. Se integra de forma perfecta con Azure Percept DK, Azure Percept Studio y otros servicios de administración perimetral de Azure. Se puede adquirir en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).

![Dispositivo Azure Percept Audio](./media/overview-azure-percept-audio/percept-audio.png)


## <a name="azure-percept-audio-components"></a>Componentes de Azure Percept Audio

Azure Percept Audio contiene los siguientes componentes principales:

- Dispositivo Azure Percept Audio listo para producción con una matriz lineal de cuatro micrófonos
- Panel para desarrolladores (incluye 2 botones, tres indicadores LED, micro USB y conector de audio de 3,5 mm)
- Cables necesarios: cable Flex, USB Micro Type-B a USB-A
- Tarjeta de bienvenida
- Placa de montaje mecánico con montaje de serie 80/20 1010 integrado


<!---

## How it works

Azure Percept Audio passes the audio input to the Azure Percept DK carrier board in a hybrid edge-cloud manner. Specifically,

- The Azure Percept Audio device: processes the incoming speech input to the clearest format by executing beam forming and echo cancellation befor sending the input to the Azure Percept DK. 
- The Azure Percept DK uses edge processing to perform keyword spotting and then sends the relevant inputs to Azure speech services.
- Cloud: Processing of natural language commands and phrases, in addition to keyword verification and retraining.
- Offline: If the device is offline it will detect the keyword and capture telemetry that there is no internet connection at the time of the command. It will not be able to weed out false accepts since it cannot perform keyword verification.

-->

## <a name="getting-started"></a>Introducción

- [Ensamblaje del kit de desarrollo Azure Percept DK](./quickstart-percept-dk-unboxing.md)
- [Finalización de la experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md)
- [Conexión del dispositivo Azure Percept Audio al kit de desarrollo](./quickstart-percept-audio-setup.md)

### <a name="build-a-no-code-prototype"></a>Creación de un prototipo sin código

Cree una [solución de voz sin código](./tutorial-no-code-speech.md) mediante las plantillas del asistente para voz de Azure Percept para escenarios de hostelería, asistencia sanitaria, inventario y automoción.

## <a name="additional-technical-information"></a>Información técnica adicional

- [Ficha técnica de Azure Percept Audio](./azure-percept-audio-datasheet.md)

## <a name="next-steps"></a>Pasos siguientes

Pida un dispositivo Azure Percept Audio en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).