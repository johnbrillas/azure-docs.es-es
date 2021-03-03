---
title: 'Mejora de síntesis con Voz personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Custom Voice es un conjunto de herramientas en línea que permiten crear una voz única y reconocible para su marca. Todo lo que se necesita para empezar son unos cuantos archivos de audio y las transcripciones asociadas. Siga los vínculos que se incluyen a continuación para empezar a crear una experiencia personalizada de conversión de voz a texto.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: trbye
ms.openlocfilehash: 45f4ca6349c14f21ce48a1fcf34b7fc8998dd278
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733523"
---
# <a name="get-started-with-custom-voice"></a>Introducción a voz personalizada

[Custom Voice](https://aka.ms/customvoice) es un conjunto de herramientas en línea que permiten crear una voz única y reconocible para su marca. Todo lo que se necesita para empezar son unos cuantos archivos de audio y las transcripciones asociadas. Siga los vínculos que se incluyen a continuación para empezar a crear una experiencia personalizada de conversión de texto a voz.

## <a name="whats-in-custom-voice"></a>¿Qué incluye Custom Voice?

Antes de comenzar con Custom Voice, necesitará una cuenta de Azure y una suscripción del servicio de voz. Cuando haya creado una cuenta, podrá preparar los datos, entrenar y probar los modelos, evaluar la calidad de la voz y, finalmente, implementar el modelo de voz personalizado.

En el diagrama siguiente se resaltan los pasos necesarios para crear un modelo de voz personalizado desde el [portal de Custom Voice](https://aka.ms/customvoice). Siga los vínculos para obtener más información.

![Diagrama de arquitectura de Custom Voice](media/custom-voice/custom-voice-diagram.png)

1. [Suscríbase y cree un proyecto](#set-up-your-azure-account): cree una cuenta de Azure y una suscripción del servicio de voz. Esta suscripción unificada proporciona acceso a la conversión de voz a texto, la conversión de texto a voz, la traducción de voz y el portal de Custom Voice. A continuación, mediante la suscripción al servicio de voz, cree su primer proyecto de Voz personalizada.

2. [Cargar datos](how-to-custom-voice-create-voice.md#upload-your-datasets): carga de datos (audio y texto) mediante el portal de Custom Voice o una API de voz personalizada. Desde el portal, puede investigar y evaluar las puntuaciones de pronunciación y las relaciones de señal a ruido. Para más información, consulte el artículo sobre la [preparación de datos para Custom Voice](how-to-custom-voice-prepare-data.md).

3. [Entrenar el modelo](how-to-custom-voice-create-voice.md#build-your-custom-voice-model): utilice los datos para crear un modelo de voz de conversión de texto a voz personalizada. Puede entrenar un modelo en diferentes idiomas. Después del entrenamiento, pruebe el modelo y, si le satisface el resultado, ya puede implementar el modelo.

4. [Implementar el modelo](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint): cree un punto de conexión personalizado para el modelo de voz de conversión de texto a voz y utilícelo para la síntesis de voz en sus productos, herramientas y aplicaciones.

## <a name="custom-neural-voices"></a>Voces neuronales personalizadas

La voz personalizada admite actualmente los niveles estándar y neuronal. Voz neuronal personalizada permite a los usuarios crear modelos de voz de mayor calidad, requiere menos datos y proporciona medidas para ayudarle a implementar la IA de manera responsable. Se recomienda usar Voz neuronal personalizada para desarrollar voces más realistas dirigidas a interfaces de conversación más naturales y permitir que clientes y usuarios finales se beneficien de la tecnología de texto a voz más moderna, de forma responsable. [Más información sobre Voz neuronal personalizada](/legal/cognitive-services/speech-service/custom-neural-voice/transparency-note-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext). 

> [!NOTE]
> Como parte del compromiso de Microsoft de diseñar tecnología de IA responsable, hemos limitado el uso de Voz neuronal personalizada. Tan solo podrá obtener acceso a la tecnología una vez que se hayan revisado sus aplicaciones y se comprometa a utilizarla de acuerdo con nuestros principios éticos. Obtenga más información sobre nuestra [directiva sobre la limitación de acceso](/legal/cognitive-services/speech-service/custom-neural-voice/limited-access-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) y [haga una solicitud aquí](https://aka.ms/customneural). Los [idiomas](language-support.md#customization) y las [regiones](regions.md#custom-voices) compatibles con las versiones estándar y neuronal de la voz personalizada son diferentes. Compruebe los detalles antes de empezar.  

## <a name="set-up-your-azure-account"></a>Configuración de la cuenta de Azure

Para poder usar el portal de Habla personalizada y crear un modelo personalizado, se necesita una suscripción al servicio de voz. Siga estas instrucciones para crear una suscripción al servicio de voz en Azure. Si no tiene una cuenta de Azure, puede registrarse para obtener una nueva.  

Después de crear una cuenta de Azure y la suscripción al servicio de voz, deberá iniciar sesión en el portal de Voz personalizada y conectarse a su suscripción.

1. Obtenga la clave de la suscripción del servicio de voz en Azure Portal.
2. Inicie sesión en el [portal de Custom Voice](https://aka.ms/custom-voice).
3. Seleccione la suscripción y cree un proyecto de voz.
4. Si desea cambiar a otra suscripción de voz, utilice el icono de engranaje situado en el panel de navegación superior.

> [!NOTE]
> Debe tener una clave de servicio de voz F0 o S0 creada en Azure para poder usar el servicio. Voz neuronal personalizada solo admite el nivel S0. 

## <a name="how-to-create-a-project"></a>Creación de un proyecto

El contenido como datos, modelos, pruebas y puntos finales se organizan en **proyectos** en el portal de Custom Voice. Cada proyecto es específico de un idioma o país y del género de la voz que desea crear. Por ejemplo, puede crear un proyecto de voz femenina para los bots de chat del centro de llamadas que utilizan el inglés de Estados Unidos (en-US).

Para crear su primer proyecto, seleccione la pestaña **Text-to-Speech/Custom Voice** (Conversión de texto a voz/Conversión de voz personalizada) y, después, haga clic en **New Project** (Nuevo proyecto). Siga las instrucciones del asistente para crear el proyecto. Después de crear el proyecto, verá cuatro pestañas: **Data** (Datos), **Training** (Entrenamiento), **Testing** (Pruebas) y **Deployment** (Implementación). Use los vínculos incluidos en [Pasos siguientes](#next-steps) para aprender a usar cada pestaña.

> [!IMPORTANT]
> El [portal de Voz personalizada](https://aka.ms/custom-voice) se ha actualizado recientemente. Si creó datos, modelos y pruebas anteriores y publicó puntos de conexión en el portal CRIS.ai o con API, debe crear un nuevo proyecto en el nuevo portal para conectarse a estas entidades antiguas.

## <a name="how-to-migrate-to-custom-neural-voice"></a>Migración a Voz neuronal personalizada

Si usa la voz personalizada no neuronal (o estándar), considere la posibilidad de migrar a Voz neuronal personalizada inmediatamente siguiendo los pasos que se indican a continuación. Se recomienda pasar a Voz neuronal personalizada para desarrollar voces más realistas dirigidas a interfaces de conversación más naturales y permitir que clientes y usuarios finales se beneficien de la tecnología de texto a voz más moderna, de forma responsable. 

1. Obtenga más información sobre nuestra [directiva sobre la limitación de acceso](/legal/cognitive-services/speech-service/custom-neural-voice/limited-access-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) y [haga una solicitud aquí](https://aka.ms/customneural). Tenga en cuenta que el acceso al servicio Voz neuronal personalizada está sujeto a la exclusiva discreción de Microsoft de acuerdo con sus criterios de idoneidad. Los clientes pueden obtener acceso a la tecnología solo después de que se haya revisado su solicitud y se les haya confirmado su uso de acuerdo con los [principios de IA responsable](https://microsoft.com/ai/responsible-ai) y el [código de conducta](/legal/cognitive-services/speech-service/tts-code-of-conduct?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext). 
2. Una vez aprobada la solicitud, se le proporcionará acceso a la característica de entrenamiento "neuronal". Asegúrese de iniciar sesión en el [portal de Voz personalizado](https://speech.microsoft.com/customvoice) con la misma suscripción de Azure que facilita en la aplicación. 
    > [!IMPORTANT]
    > Para proteger al actor de voz y evitar el entrenamiento de modelos de voz con grabaciones no autorizadas o sin el consentimiento de dicha persona, es necesario que el cliente cargue una declaración grabada del actor de voz en la que dé su consentimiento. Cuando prepare el script de grabación, asegúrese de incluir esta oración. "I [state your first and last name] am aware that recordings of my voice will be used by [state the name of the company] to create and use a synthetic version of my voice." ("Yo [indique su nombre y apellido] acepto que [indique el nombre de empresa] use grabaciones de mi voz para crear y usar una versión sintética de la voz").
    > Esta oración debe cargarse en la pestaña **Voice Talent** (Actor de voz) como archivo de consentimiento verbal. Se usará para comprobar si las grabaciones de los conjuntos de datos de entrenamiento se corresponden con la persona que otorga el consentimiento.
3. Una vez creado el modelo de Voz neuronal personalizada, implemente el modelo de voz en un nuevo punto de conexión. Para crear un punto de conexión de voz personalizado con el modelo de voz neuronal, vaya a **Texto a voz > Voz personalizada > Implementación**. Seleccione **Implementar modelo** y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. A continuación, seleccione el modelo de voz neuronal personalizada que le gustaría asociar a este punto de conexión y confirme la implementación.  
4. Si ha creado un punto de conexión con un nuevo modelo, actualice el código de las aplicaciones. 

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de datos de voz personalizado](how-to-custom-voice-prepare-data.md)
- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Guía: Grabación de muestras de voz](record-custom-voice-samples.md)