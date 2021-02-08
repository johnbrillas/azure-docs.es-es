---
title: Directrices para la implementación responsable de la tecnología de voz sintética
titleSuffix: Azure Cognitive Services
description: Directrices generales de diseño de Microsoft para usar la tecnología de voz sintética. Se desarrollaron en estudios que Microsoft llevó a cabo con actores de voz, consumidores y personas con trastornos del lenguaje para guiar el desarrollo responsable de la voz sintética.
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: 715c09ef65358b21e78cfde204b4819db0c7875d
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99428426"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>Directrices para la implementación responsable de la tecnología de voz sintética

## <a name="general-considerations-to-keep-in-mind-when-implementing-ai-systems"></a>Consideraciones generales que se deben tener en cuenta al implementar sistemas de inteligencia artificial 

En este artículo se habla específicamente sobre la voz sintética y la Voz neuronal personalizada, así como las consideraciones clave para usar esta tecnología de forma responsable. Pero en general hay varios aspectos que se deben tener en cuenta a la hora de decidir cómo usar e implementar productos y características con tecnología de inteligencia artificial: 

* ¿Funcionará correctamente este producto o característica en el escenario? Antes de implementar la inteligencia artificial en el escenario, pruebe su comportamiento con datos reales y asegúrese de que puede proporcionar la precisión que necesita. 
* ¿Está equipado para identificar los errores y responder a ellos? Los productos y las características con tecnología de IA no siempre tendrán una precisión del 100 %, por lo que debe tener en cuenta cómo identificarán los errores que puedan producirse y cómo responderán ante ellos. 

## <a name="general-guidelines-for-using-synthetic-voice-technology"></a>Instrucciones generales para usar la tecnología de voz sintética 
A continuación se presentan las directrices generales de diseño de Microsoft para usar la tecnología de voz sintética. Se desarrollaron en estudios que Microsoft llevó a cabo con actores de voz, consumidores e incluso personas con trastornos del lenguaje para guiar el desarrollo responsable de la voz sintética.

En la implementación de la tecnología de síntesis de voz, se aplican las siguientes directrices en la mayoría de los escenarios.

### <a name="disclose-when-the-voice-is-synthetic"></a>Divulgación de cuando la voz es sintética
Divulgar el hecho de que la voz se generó por ordenador no solo minimiza el riesgo de resultados perjudiciales derivados de una decepción, sino que también aumenta la confianza en la organización que presenta la voz. Más información sobre [cómo divulgar información](concepts-disclosure-guidelines.md).

Microsoft requiere que sus clientes revelen la naturaleza sintética de la voz neuronal personalizada a sus usuarios. 
* Asegúrese de proporcionar una divulgación adecuada para el público, especialmente cuando se use la voz de una persona conocida: los usuarios deciden en función de la persona que habla, ya sea de forma consciente o inconsciente.  Por ejemplo, la divulgación se podría compartir verbalmente al principio de una difusión. Para más información, visite los [modelos de divulgación](concepts-disclosure-patterns.md).   
* Considere la posibilidad de una divulgación adecuada para padres u otras partes con casos de uso diseñados para menores y niños; si el caso de uso está destinado a menores o niños, deberá asegurarse de que los padres o los tutores legales puedan comprender la divulgación sobre el uso de medios sintéticos y tomar la decisión adecuada sobre si utilizar la experiencia o no. 

### <a name="select-appropriate-voice-types-for-your-scenario"></a>Selección de los tipos de voz adecuados para su escenario
Considere cuidadosamente el contexto de uso y los posibles daños asociados al uso de la voz sintética. Por ejemplo, es posible que las voces sintéticas de alta fidelidad no sean adecuadas en situaciones de alto riesgo, como la mensajería personal, las transacciones financieras o situaciones complejas que requieran de la adaptación humana o la empatía. Los usuarios también pueden tener expectativas diferentes para cada tipo de voz. Por ejemplo, al escuchar noticias confidenciales leídas por una voz sintética, algunos usuarios prefieren una lectura más empática y similar a los seres humanos, mientras que otros prefieren una voz más imparcial y monótona. Considere la posibilidad de probar la aplicación para comprender mejor las preferencias del usuario.

### <a name="be-transparent-about-capabilities-and-limitations"></a>Transparencia en cuanto a funcionalidades y limitaciones
Es más probable que los usuarios tengan mayores expectativas al interactuar con agentes de voz sintéticos de alta fidelidad. Por consiguiente, cuando las funcionalidades del sistema no cumplen esas expectativas, la confianza puede verse afectada y puede dar lugar a experiencias desagradables o incluso dañinas.

### <a name="provide-optional-human-support"></a>Ofrecimiento de soporte técnico humano opcional
En escenarios transaccionales ambiguos (por ejemplo, un centro telefónico de soporte técnico), los usuarios no siempre confían en que un agente artificial responderá correctamente a sus solicitudes. El soporte técnico humano puede ser necesario en estas situaciones, independientemente de la calidad realista de la voz o la funcionalidad del sistema.

## <a name="considerations-for-voice-talent"></a>Consideraciones sobre los actores de voz
Al trabajar con actores de voz, como actores de doblaje, para crear voces sintéticas, se aplica la directriz siguiente.

### <a name="obtain-meaningful-consent-from-voice-talent"></a>Obtención del consentimiento significativo del actor de voz
Los actores de voz deben tener control sobre su modelo de voz (cómo y dónde se utilizará), así como ser compensados siempre que se use. Microsoft requiere que los clientes de Voz personalizada obtengan el permiso expreso por escrito de sus actores de voz para crear una voz sintética y su contrato con el actor de voz contempla la duración, el uso y las limitaciones de contenido.  Si va a crear una voz sintética de una persona conocida, debe proporcionar una forma para que la persona responsable de la voz edite o apruebe el contenido.

Algunos actores de voz no saben cuáles son los posibles usos malintencionados de la tecnología y los propietarios del sistema deben instruirlos sobre las funcionalidades de la tecnología. Microsoft requiere que los clientes compartan la [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent) de Microsoft directamente con el actor de voz o su representante autorizado, en la que se describe cómo se desarrollan las voces sintéticas y su funcionamiento junto con los servicios de texto a voz.

## <a name="considerations-for-those-with-speech-disorders"></a>Consideraciones para las personas con trastornos del habla
Al trabajar con personas con trastornos del habla para crear o implementar la tecnología de voz sintética, se aplican las siguientes directrices.

### <a name="provide-guidelines-to-establish-contracts"></a>Ofrecimiento de directrices para celebrar contratos
Proporcione directrices para celebrar contratos con personas que usan la voz sintética como ayuda para hablar. El contrato debería especificar las partes que poseen la voz, la duración de uso, los criterios de transferencia de propiedad, los procedimientos para eliminar la fuente de voz y cómo evitar el acceso no autorizado. Además, habilite la transferencia contractual de la propiedad de la fuente de voz después de la muerte a los familiares si esa persona ha dado su permiso.

### <a name="account-for-inconsistencies-in-speech-patterns"></a>Consideración de las incoherencias en los patrones de habla
En el caso de las personas con trastornos del habla que registran sus propias fuentes de voz, las incoherencias en su patrón de habla (mala pronunciación o incapacidad de pronunciar ciertas palabras) pueden complicar el proceso de grabación. En estos casos, la tecnología de voz sintética y las sesiones de grabación deberían adaptarse a ellos (es decir, proporcionar descansos y un número adicional de sesiones de grabación).

### <a name="allow-modification-over-time"></a>Permitir la modificación a lo largo del tiempo
Las personas con trastornos del habla quieren hacer actualizaciones en su voz sintética para reflejar su edad (por ejemplo, un niño que alcanza la pubertad). Las personas también pueden tener preferencias estilísticas que cambian con el tiempo y puede que quieran realizar cambios en las características de tono, acento u otras.


## <a name="reference-docs"></a>Documentos de referencia

* [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Información general sobre control](concepts-gating-overview.md)
* [Procedimiento de divulgación](concepts-disclosure-guidelines.md)
* [Modelos de diseño de divulgación](concepts-disclosure-patterns.md)

## <a name="next-steps"></a>Pasos siguientes

* [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Procedimiento de divulgación](concepts-disclosure-guidelines.md)
* [Modelos de diseño de divulgación](concepts-disclosure-patterns.md)
