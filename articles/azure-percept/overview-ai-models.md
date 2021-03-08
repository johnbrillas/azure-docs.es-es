---
title: Modelos de inteligencia artificial de Azure Percept
description: Más información sobre los modelos de inteligencia artificial disponibles para la creación de prototipos y la implementación
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/16/2021
ms.custom: template-concept
ms.openlocfilehash: 8a9ed564012e8b530436152d931f5a5f6470ff98
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098373"
---
# <a name="azure-percept-ai-models"></a>Modelos de inteligencia artificial de Azure Percept

Azure Percept le permite desarrollar e implementar modelos de IA directamente en Azure Percept DK con [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819). La implementación de modelos utiliza [Azure IOT Hub](https://azure.microsoft.com/services/iot-hub/) y [Azure IOT Edge](https://azure.microsoft.com/services/iot-edge/#iotedge-overview).

## <a name="sample-ai-models"></a>Modelos de ejemplo de inteligencia artificial

Azure Percept Studio contiene modelos de ejemplo para las siguientes aplicaciones:

- Detección de personas
- Detección de vehículos
- Detección general de objetos
- Detección de productos en estantes

Con modelos previamente entrenados, no se requiere ninguna recopilación de datos de entrenamiento ni codificación. Solo tiene que implementar el modelo que desee en Azure Percept DK desde el portal y abrir la secuencia de vídeo del kit de desarrollo para ver la inferencia de modelos en acción. También se puede acceder a los datos de telemetría de la inferencia de modelos a través de la herramienta [Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases).

## <a name="pre-built-solutions"></a>Soluciones pregeneradas

También está disponible una [solución de análisis espacial pregenerada para la detección de personas](https://github.com/george-moore/Santa-Cruz-AI-App). La solución pregenerada es una aplicación de inteligencia artificial de código abierto que proporciona el recuento de personas en el perímetro con eventos de entrada y salida de la zona definida por el usuario. La salida de vídeo e inteligencia artificial del dispositivo perimetral local se envía a [Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) y la interfaz de usuario se ejecuta como un sitio web de Azure. La inferencia de inteligencia artificial se proporciona mediante un modelo de IA de código abierto para la detección de personas.

:::image type="content" source="./media/overview-ai-models/people-detector.gif" alt-text="GIF de una solución preconfigurada de análisis espacial.":::

## <a name="custom-no-code-solutions"></a>Soluciones sin código personalizadas

Mediante Azure Percept Studio, puede desarrollar soluciones de [visión](./tutorial-nocode-vision.md) y voz personalizadas, sin necesidad de codificación.

En el caso de las soluciones de visión personalizadas, están disponibles los modelos de inteligencia artificial para la detección y la clasificación de objetos. Simplemente cargue y etiquete sus imágenes de entrenamiento, las cuales se pueden tomar directamente del módulo de sistema de visión de Azure Percept DK, si lo desea. El entrenamiento y la evaluación de modelos se realiza fácilmente en [Custom Vision](https://www.customvision.ai/), que forma parte de [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/#overview).

En el caso de las soluciones de voz personalizadas, las plantillas del asistente para voz están disponibles actualmente para las siguientes aplicaciones:

- Hostelería: habitación de hotel equipada con dispositivos inteligentes controlados por voz.
- Atención sanitaria: centro de atención equipado con dispositivos inteligentes controlados por voz.
- Inventario: centro de inventario equipado con dispositivos inteligentes controlados por voz.
- Automoción: centro de automoción equipado con dispositivos inteligentes controlados por voz.

Las palabras clave y los comandos predefinidos del asistente para voz están disponibles directamente a través del portal. Las palabras clave y los comandos personalizados se pueden crear y entrenar en [Speech Studio](https://speech.microsoft.com/), que también forma parte de Azure Cognitive Services.

## <a name="advanced-development"></a>Desarrollo avanzado

Consulte las instrucciones, los tutoriales y los ejemplos de [desarrollo avanzado de Azure Percept en GitHub](https://github.com/microsoft/azure-percept-advanced-development) sobre aspectos como:

* Incorporación de un modelo de AI personalizado al dispositivo
* Actualización de un modelo que ya es compatible con aprendizaje por transferencia
* Y mucho más
