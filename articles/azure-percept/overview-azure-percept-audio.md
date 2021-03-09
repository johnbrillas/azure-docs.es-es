---
title: Introducción al dispositivo Azure Percept Audio
description: Más información acerca de Azure Percept Audio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 8f84fb6bf37a3d3b61f4cad2c89745447aa88a36
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179398"
---
# <a name="introduction-to-azure-percept-audio"></a>Introducción a Azure Percept Audio

Azure Percept Audio es un dispositivo accesorio que agrega funcionalidades de voz con inteligencia artificial a Azure Percept DK. Contiene un procesador de audio preconfigurado y una matriz lineal de cuatro micrófonos, lo que permite aplicar comandos de voz, detección de palabras clave y reconocimiento de voz a gran distancia en dispositivos de escucha locales mediante Azure Cognitive Services. Azure Percept Audio permite a los fabricantes de dispositivos ampliar las funcionalidades de visión de Azure Percept DK en los nuevos dispositivos inteligentes activados por voz. Se integra de forma perfecta con Azure Percept DK, Azure Percept Studio y otros servicios de administración perimetral de Azure. Se puede adquirir en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).

:::image type="content" source="./media/overview-azure-percept-audio/percept-audio.png" alt-text="Dispositivo Azure Percept Audio":::

## <a name="azure-percept-audio-components"></a>Componentes de Azure Percept Audio

Azure Percept Audio contiene los siguientes componentes principales:

- Dispositivo de audio de Azure Percept (módulo de sistema) listo para producción con una matriz lineal de cuatro micrófonos y procesamiento de audio mediante un códec XMOS
- Placa para desarrolladores (intermediador) que incluye 2 botones, 3 indicadores LED, micro USB y conector de audio de 3,5 mm
- Cables necesarios: cable FPC, micro USB tipo B a USB A
- Tarjeta de bienvenida
- Placa de montaje mecánico con montaje de serie 80/20 1010 integrado

## <a name="compute-capabilities"></a>Capacidades de proceso 

Azure Percept Audio pasa la entrada de audio a través de la pila de voz que se ejecuta en la CPU de la placa base del dispositivo Azure Percept DK en forma híbrida de nube-perímetro. Por lo tanto, Azure Percept Audio necesita una placa base con un sistema operativo que admita la pila de voz para funcionar. 

El procesamiento se realiza del siguiente modo: 

- Azure Percept Audio: se ocupa de conformar los haces y cancelar el eco: procesa el audio entrante para optimizar la voz y enviarlo al DK.  

- Azure Percept DK: la pila de voz se ocupa de la detección de palabras clave.  

- Nube: procesa comandos y frases de lenguaje natural, comprueba las palabras clave y repite el entrenamiento. 

- Sin conexión: si el dispositivo está sin conexión, detectará la palabra clave y capturará la telemetría del estado de conexión a Internet. Se puede observar un aumento falso de la tasa de aceptación en la detección de palabras clave, ya que no se puede realizar la comprobación de palabras clave en la nube. 

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

## <a name="build-a-no-code-prototype"></a>Creación de un prototipo sin código

Cree una [solución de voz sin código](./tutorial-no-code-speech.md) en [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) mediante las plantillas del asistente para voz de Azure Percept para escenarios de hostelería, asistencia sanitaria, inventario y automoción.

### <a name="manage-your-no-code-speech-solution"></a>Administración de la solución de voz sin código

- [Configuración del asistente para voz en IoT Hub](./how-to-manage-voice-assistant.md)
- [Configuración del asistente para voz en Azure Percept Studio](./how-to-configure-voice-assistant.md)
- [Solución de problemas de Azure Percept Audio](./troubleshoot-audio-accessory-speech-module.md)

## <a name="additional-technical-information"></a>Información técnica adicional

- [Ficha técnica de Azure Percept Audio](./azure-percept-audio-datasheet.md)
- [Comportamiento del botón y los indicadores LED](./audio-button-led-behavior.md)

## <a name="next-steps"></a>Pasos siguientes

Pida un dispositivo Azure Percept Audio en la [tienda en línea de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=2155270).
