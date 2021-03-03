---
title: 'Introducción a la voz neuronal personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: La Voz neuronal personalizada es una característica de conversión de texto a voz que permite crear una voz sintética personalizada única para las aplicaciones mediante los propios datos de audio del usuario que se proporcionan como ejemplo.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/01/2020
ms.author: trbye
ms.openlocfilehash: 36885e4673b83d1db7972f03c4a6309f766206c5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713106"
---
# <a name="what-is-custom-neural-voice"></a>¿Qué es la voz neuronal personalizada?

La voz neuronal personalizada es una característica de [conversión de texto a voz](./text-to-speech.md) (TTS) que le permite crear una voz sintética personalizada única para las aplicaciones mediante los propios datos del usuario que se proporcionan como ejemplo. En la conversión de texto a voz, el texto se convierte en voz sintética mediante un modelo de aprendizaje automático que suene como la voz elegida. Con la [API REST](./rest-text-to-speech.md), puede permitir que las aplicaciones hablen con [voces pregeneradas](./language-support.md#neural-voices) o con sus propios modelos de [voz personalizada](./how-to-custom-voice-prepare-data.md) desarrollados mediante la característica Voz neuronal personalizada. La Voz neuronal personalizada se basa en la tecnología TTS neuronal que crea una voz con sonido natural que con frecuencia no se distingue cuando se compara con la voz humana.
La voz de sonido natural y realista de la Voz neuronal personalizada puede representar marcas, personificar máquinas y permitir que los usuarios interactúen oralmente con las aplicaciones de forma natural.

> [!NOTE]
> Para usar la característica Voz neuronal personalizada es necesario registrarse, y el acceso a ella está limitado en función de los criterios de elegibilidad y uso de Microsoft. Los clientes que quieran usar esta característica deben registrar sus casos de uso mediante el [formulario de entrada](https://aka.ms/customneural).

## <a name="the-basics-of-custom-neural-voice"></a>Aspectos básicos de la Voz neuronal personalizada

La tecnología TTS neuronal subyacente que se usa en la Voz neuronal personalizada consta de tres componentes principales: analizador de texto, modelo acústico neuronal y vocoder neuronal. Para generar voz sintética natural a partir de texto, la primera entrada en el analizador de texto es el texto, cuya salida es una secuencia de fonemas. Un fonema es una unidad básica de sonido que distingue una palabra de otra en un idioma determinado. Una secuencia de fonemas define la pronunciación de las palabras proporcionadas en el texto. 

A continuación, la secuencia de fonemas entra en el modelo acústico neuronal para predecir las características acústicas que definen las señales de voz, como el timbre, el estilo de habla, la velocidad, la entonación y la variedad de acentos. Por último, el vocoder neuronal convierte las características acústicas en ondas audibles para que se genere la voz sintética.

![Imagen de introducción para la voz neuronal personalizada.](./media/custom-voice/cnv-intro.png)

Los modelos de voz de TTS neuronal se entrenan mediante redes neuronal profundas basadas en las muestras de grabación de voces humanas. En este [blog](https://techcommunity.microsoft.com/t5/azure-ai/neural-text-to-speech-extends-support-to-15-more-languages-with/ba-p/1505911), se describe cómo funciona TTS neuronal con modelos de síntesis de voz neuronal de última generación. En el blog también se explica cómo se puede adaptar un modelo base universal con menos de dos horas de datos de voz (o menos de 2000 expresiones grabadas) de un hablante objetivo y aprender a hablar con la voz de este. Para información sobre cómo se entrena un vocoder neuronal, consulte la [entrada de blog ](https://techcommunity.microsoft.com/t5/azure-ai/azure-neural-tts-upgraded-with-hifinet-achieving-higher-audio/ba-p/1847860).

Con la funcionalidad de personalización de la Voz neuronal personalizada, puede adaptar el motor TTS neuronal para ajustarse mejor a los escenarios de los usuarios. Para crear una voz neuronal personalizada, use [Speech Studio](https://speech.microsoft.com/customvoice) para cargar el audio grabado y los scripts correspondientes, entrenar el modelo e implementar la voz en un punto de conexión personalizado. En función del caso de uso, la Voz neuronal personalizada se puede emplear para convertir texto en voz en tiempo real (como en un asistente virtual inteligente) o para generar el contenido de audio sin conexión (como en los audiolibros o las instrucciones de las aplicaciones de e-learning) con la entrada de texto proporcionada por el usuario. Esto se puede hacer a través de la [API REST](./rest-text-to-speech.md), el [SDK de voz](./get-started-text-to-speech.md?pivots=programming-language-csharp&tabs=script%2cwindowsinstall), o un [portal web](https://speech.microsoft.com/audiocontentcreation).

## <a name="terms-and-definitions"></a>Términos y definiciones

| **Término**      | **Definición**                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Modelo de voz   | Un modelo de texto a voz que puede imitar las características vocales únicas de un hablante de destino. Un *modelo de voz* también se conoce como *fuente de voz* o *voz sintética*. Un modelo de voz es un conjunto de parámetros en formato binario que no es legible y no contiene grabaciones de audio. No se puede aplicar ingeniería inversa para derivar o construir el audio de una voz humana. |
| Actor de voz  | Personas o hablantes de destino cuyas voces se graban y se usan para crear modelos de voz diseñados para que suenen como la voz del actor.                                                                                                                                                                                                                                                   |
| TTS estándar  | Método estándar o "tradicional" de TTS que divide el lenguaje hablado en fragmentos de código fonéticos para que se puedan remezclar y relacionar mediante métodos estadísticos o de programación clásicos.                                                                                                                                                                                                    |
| TTS neuronal    | TTS neuronal sintetiza la voz mediante redes neuronales profundas que han "aprendido" la manera en que se combina la fonética en la voz humana natural, en lugar de usar métodos estadísticos o de programación basados en procedimientos. Además de las grabaciones de un actor de voz de destino, TTS neuronal usa un modelo base o de biblioteca de origen que se crea con grabaciones de voz de muchos hablantes diferentes.          |
| Datos de aprendizaje. | Conjunto de datos de entrenamiento de voz neuronal personalizada que incluye las grabaciones de audio del actor de voz y las transcripciones de texto asociadas.                                                                                                                                                                                                                                                               |
| Persona       | Una persona describe quién desea que esta voz sea. Un buen diseño de persona informará de toda la creación de la voz, tanto si se elige un modelo de voz disponible ya creado o se parte de cero y se selecciona y se graba a un nuevo actor de voz.                                                                                                |
| Script        | Un script es un archivo de texto que contiene las expresiones que dirá el actor de voz. (El término "*expresiones*" abarca tanto oraciones completas como frases más cortas).                                                                                                                                                                                                                               |

## <a name="responsible-use-of-ai"></a>Uso responsable de la inteligencia artificial

Para información sobre cómo usar la Voz neuronal personalizada de forma responsable, consulte la [nota de transparencia](/legal/cognitive-services/speech-service/custom-neural-voice/transparency-note-custom-neural-voice?context=/azure/cognitive-services/speech-service/context/context). Las notas de transparencia de Microsoft están pensadas para ayudarle a entender cómo funciona nuestra tecnología de inteligencia artificial, las elecciones que los propietarios del sistema pueden hacer que influyan en el rendimiento y el comportamiento del sistema y la importancia de pensar en todo el sistema, incluida la tecnología, las personas y el entorno.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Voz personalizada](how-to-custom-voice.md)
* [Creación y uso de un punto de conexión de voz personalizada](how-to-custom-voice-create-voice.md)