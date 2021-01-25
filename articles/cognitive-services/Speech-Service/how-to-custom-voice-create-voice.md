---
title: 'Creación de un proyecto de Voz personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Cuando esté listo para cargar los datos, vaya al portal de voz personalizada. Cree o seleccione un proyecto de voz personalizada. El proyecto debe compartir el idioma o configuración regional y las propiedades de género correctos con los datos que pretende usar para el entrenamiento de voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 262a96c0c316987d0245ed29836f6a013c4339d1
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98573163"
---
# <a name="create-a-custom-voice"></a>Creación de una voz personalizada

En [Preparar los datos para crear una voz personalizada](how-to-custom-voice-prepare-data.md), se describen los tipos de datos diferentes que puede usar para entrenar una voz personalizada y los distintos requisitos de formato. Cuando haya preparado los datos, puede empezar a cargarlos en el [portal de voz personalizada](https://aka.ms/custom-voice-portal), o a través de la API de entrenamiento de voz personalizada. Aquí se describen los pasos del entrenamiento de una voz personalizada mediante el portal.

> [!NOTE]
> En esta página se supone que ha leído [Empezar a trabajar con la voz personalizada](how-to-custom-voice.md) y [Preparar los datos para crear una voz personalizado](how-to-custom-voice-prepare-data.md) y que ha creado un proyecto de voz personalizada.

Compruebe los idiomas admitidos para la voz personalizada: [idioma para la personalización](language-support.md#customization).

## <a name="upload-your-datasets"></a>Cargar los conjuntos de datos

Cuando esté listo para cargar los datos, vaya al [portal de voz personalizada](https://aka.ms/custom-voice-portal). Cree o seleccione un proyecto de voz personalizada. El proyecto debe compartir el idioma o configuración regional y las propiedades de género correctos con los datos que pretende usar para el entrenamiento de voz. Por ejemplo, seleccione `en-GB` si las grabaciones de audio se han realizado en inglés con acento británico.

Vaya a la pestaña **Data** (Datos) y haga clic en **Upload data** (Cargar datos). En el asistente, seleccione el tipo de datos correcto que coincida con lo que ha preparado.

Cada conjunto de datos que cargue debe cumplir los requisitos del tipo de datos elegido. Es importante dar formato correctamente a los datos antes de cargarlos. Esto garantiza que el servicio de voz personalizada procesa los datos con precisión. Vaya a [Preparar los datos para crear una voz personalizada](how-to-custom-voice-prepare-data.md) y asegúrese de que los datos tienen el formato correcto.

> [!NOTE]
> Los usuarios de suscripción gratuita (F0) pueden cargar dos conjuntos de datos al mismo tiempo. En cambio, los usuarios con una suscripción estándar (S0) pueden cargar cinco conjuntos de datos a la vez. Si alcanza el límite, espere hasta que al menos uno de los conjuntos de datos finalice la importación. A continuación, inténtelo de nuevo.

> [!NOTE]
> El número máximo de conjuntos de datos que se pueden importar por suscripción es de 10 archivos ZIP para usuarios de la suscripción gratuita (F0) y 500 para usuarios para la suscripción estándar (S0).

Los conjuntos de datos se validan automáticamente una vez que pulsa el botón de carga. La validación de datos incluye una serie de comprobaciones en los archivos de audio para comprobar su formato de archivo, el tamaño y la frecuencia de muestreo. Corrija los errores si los hay y vuelva a realizarla. Cuando se inicia correctamente la solicitud de importación de datos, debería ver una entrada en la tabla de datos que se corresponde con el conjunto de datos que acaba de cargar.

En la siguiente tabla se muestran los estados de procesamiento de los conjuntos de datos importados:

| State | Significado |
| ----- | ------- |
| Processing | El conjunto de datos se ha recibido y se está procesando. |
| Correcto | El conjunto de datos se ha validado y se puede usar para compilar un modelo de voz. |
| Con error | El conjunto de datos ha dado error durante el procesamiento debido a muchas razones, por ejemplo, errores de archivo, problemas de datos o problemas de red. |

Una vez completada la validación, puede ver el número total de expresiones coincidentes para cada uno de los conjuntos de datos en la columna **Utterances** (Expresiones). Si el tipo de datos que ha seleccionado requiere una segmentación de audio de larga duración, esta columna solo refleja las expresiones que se han segmentado automáticamente en función de las transcripciones o mediante el servicio de transcripción de voz. Puede seguir descargando el conjunto de datos validado para ver los resultados detallados de las expresiones importadas correctamente y sus transcripciones de asignación. Sugerencia: la segmentación de audio de larga duración puede tardar más de una hora en completar el procesamiento de datos.

En la vista detallada de datos, puede comprobar la puntuación de las pronunciaciones y el nivel de ruido de cada uno de los conjuntos de datos. La puntuación de las pronunciaciones abarca un rango del 0 al 100. Una puntuación por debajo de 70 normalmente indica un error en el discurso o que el guion no coincide. Un acento marcado puede reducir la puntuación de las pronunciaciones, y afectar a la voz digital que se ha creado.

Una relación de la señal y el ruido (SNR) más alta indica un ruido más bajo en el audio. Por lo general, el audio puede alcanzar una SNR de más de 50 puntos si se graba en estudios profesionales. Un audio que tenga un valor de SNR por debajo de 20 puntos puede provocar un ruido obvio en la voz generada.

Puede volver a grabar cualquier expresión que tenga una puntuación baja debido a la pronunciación o a la pobre relación entre el ruido y la señal. Si no es posible volver a realizar la grabación, puede excluir esas expresiones de su conjunto de datos.

> [!NOTE]
> Si usa Voz neuronal personalizada, debe registrar su actor de voz en la pestaña **Voice Talent** (Actor de voz). Al preparar el script de grabación, asegúrese de incluir la frase siguiente para obtener la confirmación del actor de voz del uso de sus datos de voz para crear un modelo de voz TTS y generar voz sintética. "I [state your first and last name] am aware that recordings of my voice will be used by [state the name of the company] to create and use a synthetic version of my voice." ("Yo [indique su nombre y apellido] acepto que [indique el nombre de empresa] use grabaciones de mi voz para crear y usar una versión sintética de la voz").
Esta frase se usará para comprobar si las grabaciones de los conjuntos de datos de entrenamiento se corresponden con la persona que otorga el consentimiento. [Obtenga más información sobre cómo se procesarán los datos y cómo se realiza la comprobación del actor de voz](https://aka.ms/CNV-data-privacy). 

## <a name="build-your-custom-voice-model"></a>Compilación del modelo de voz personalizada

Una vez que el conjunto de datos se haya validado, podrá usarlo para compilar su modelo de voz personalizado.

1.  Vaya a **Text-to-Speech > Custom Voice > [name of project] > Model** (Texto a voz > Voz personalizada > [nombre del proyecto] > Modelo).

2.  Haga clic en **Train model** (Entrenar modelo).

3.  A continuación, escriba un **nombre** y una **descripción** que le ayuden a identificar este modelo.

    Elija un nombre con cuidado. El nombre que escriba aquí será el nombre que use para especificar la voz en su solicitud de síntesis de voz como parte de la entrada de SSML. Solo se permiten letras, números y algunos signos de puntuación, como -, \_ y (","). Use nombres diferentes paras modelos de voz diferentes.

    Un uso habitual del campo **Descripción** es registrar los nombres de los conjuntos de datos que se usaron para crear el modelo.

4.  En la página **Select training data** (Seleccionar datos de entrenamiento), elija uno o varios conjuntos de datos que quiera usar para el entrenamiento. Compruebe el número de expresiones antes de enviarlas. Puede comenzar con cualquier número de expresiones para los modelos de voz de en-US y zh-CN con el método de entrenamiento adaptable. Para otras configuraciones regionales, debe seleccionar más de 2000 expresiones para poder entrenar una voz con un nivel estándar, incluidos los métodos de entrenamiento concatenativo y paramétrico estadístico, y más de 300 expresiones para entrenar una voz neuronal personalizada. 

    > [!NOTE]
    > Los nombres de audio duplicados se quitarán del entrenamiento. Asegúrese de que los conjuntos de datos que seleccione no contengan los mismos nombres de audio en varios archivos ZIP.

    > [!TIP]
    > Para obtener unos resultados de calidad, es necesario usar los conjuntos de datos del mismo altavoz. Los distintos métodos de entrenamiento requieren un tamaño de datos de entrenamiento diferente. Para entrenar un modelo con el método paramétrico estadístico, se necesitan al menos 2000 expresiones distintas. Para el método concatenativo se necesitan 6000 expresiones, mientras que el neuronal exige un tamaño de datos mínimo de 300 expresiones.

5. Seleccione el **método de entrenamiento** en el siguiente paso. 

    > [!NOTE]
    > Si desea entrenar una voz neuronal, debe especificar un perfil de talento de voz con el archivo de consentimiento de audio del talento de voz aceptando que se usen sus datos de voz para entrenar un modelo de voz personalizado. Voz neuronal personalizada está disponible con acceso limitado. Asegúrese de comprender los [requisitos de IA responsable](https://aka.ms/gating-overview) y [aplicar el acceso aquí](https://aka.ms/customneural). 
    
    En esta página también puede cargar su script para realizar pruebas. El script de prueba debe ser un archivo txt de menos de 1 MB. Entre los formatos de codificación compatibles se incluyen ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE o UTF-16-BE. Cada párrafo de la expresión dará como resultado un audio independiente. Si desea combinar todas las frases en un solo audio, reúnalas todas en un párrafo. 

6. Haga clic en **Train** (Entrenar) para empezar a crear el modelo de voz.

En la tabla de entrenamiento se muestra una nueva entrada que corresponde a este modelo recién creado. En la tabla también se muestra el estado: Procesando, Correcto, Error.

El estado que se muestra refleja el proceso de convertir el conjunto de datos en un modelo de voz, como se muestra aquí.

| State | Significado |
| ----- | ------- |
| Processing | Se está creando el modelo de voz. |
| Correcto | El modelo de voz se ha creado y se puede implementar. |
| Con error | El modelo de voz ha dado error en el entrenamiento por muchas razones, por ejemplo, problemas desapercibidos con los datos o problemas de red. |

El tiempo de aprendizaje varía según el volumen de datos de audio procesados y el método de entrenamiento seleccionado. Puede variar entre 30 minutos y 40 horas. Cuando finalice correctamente el entrenamiento del modelo, puede empezar a probarlo. 

> [!NOTE]
> Los usuarios con una suscripción gratuita (F0) pueden entrenar una fuente de voz al mismo tiempo. En cambio, los usuarios que tengan una suscripción estándar (S0) pueden entrenar tres voces simultáneamente. Si alcanza el límite, espere hasta que al menos una de las fuentes de voz finalice el aprendizaje e inténtelo de nuevo.

> [!NOTE]
> El entrenamiento de voces neuronales personalizadas no es gratuito. Consulte aquí los [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/). 

> [!NOTE]
> El número máximo de modelos de voz que se pueden entrenar por suscripción es de 10 modelos para los usuarios con una suscripción gratuita (F0) y de 100 para los usuarios con una suscripción estándar (S0).

Si usa la funcionalidad de entrenamiento de voz neuronal, puede optar por entrenar un modelo optimizado para escenarios de streaming en tiempo real o un modelo neuronal de alta definición optimizado para la [síntesis de audio de larga duración](long-audio-api.md) de manera asincrónica.  

## <a name="test-your-voice-model"></a>Prueba del modelo de voz

Cada entrenamiento generará 100 audios de ejemplo automáticamente para ayudarle a probar el modelo. Una vez que el modelo de voz se haya creado correctamente, podrá probarlo antes de implementarlo para su uso.

1.  Vaya a **Text-to-Speech > Custom Voice > [name of project] > Model** (Texto a voz > Voz personalizada > [nombre del proyecto] > Modelo).

2.  Haga clic en el nombre del modelo que desea probar.

3.  En la página de detalles del modelo, puede encontrar los audios de ejemplo en la pestaña **Pruebas**. 

La calidad de la voz depende de una serie de factores, como el tamaño de los datos de entrenamiento, la calidad de la grabación, la precisión del archivo de transcripción, el grado en que la voz grabada en los datos de entrenamiento coincide con la personalidad de la voz diseñada para el caso de uso previsto, etc. [Consulte aquí más información sobre las funcionalidades y los límites de nuestra tecnología y el procedimiento recomendado para mejorar la calidad del modelo](https://aka.ms/CNV-limits). 

## <a name="create-and-use-a-custom-voice-endpoint"></a>Creación y uso de un punto de conexión de voz personalizado

Una vez que haya creado y probado con éxito su modelo de voz, impleméntelo en un punto de conexión personalizado de Text to Speech. A continuación, use ese punto de conexión en lugar del punto de conexión habitual al realizar solicitudes de Text to Speech a través de la API de REST. Recuerde que solo se puede llamar al punto de conexión personalizado mediante la suscripción que utilizó para implementar la fuente.

Para crear un nuevo punto de conexión personalizado, vaya a **Text-to-Speech > Custom Voice > Endpoint** (Texto a voz > Voz personalizada > Punto de conexión). Seleccione **Add endpoint** (Agregar punto de conexión) y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. A continuación, seleccione el modelo de voz personalizada que le gustaría asociar a este punto de conexión.

Después de haber hecho clic en el botón **Add** (Agregar), en la tabla de puntos de conexión, verá una entrada para el nuevo punto de conexión. El proceso para crear instancias del nuevo punto de conexión puede llevar unos minutos. Cuando el estado de la implementación muestra el valor **Completado**, quiere decir que el punto de conexión está listo para su uso.

Puede **suspender** y **reanudar** el punto de conexión si no lo usa continuamente. Cuando se reactiva un punto de conexión tras la suspensión, la dirección URL del punto de conexión se mantiene igual, por lo que no es necesario cambiar el código de las aplicaciones. 

También puede actualizar el punto de conexión a un nuevo modelo. Para cambiar el modelo, asegúrese de que el nuevo modelo tiene el mismo nombre que el que desea actualizar. 

> [!NOTE]
> Los usuarios con una suscripción gratuita (F0) solo pueden tener un modelo implementado. En cambio, los usuarios que tengan una suscripción estándar (S0) pueden crear hasta 50 puntos de conexión, cada uno de ellos con su propia voz personalizada.

> [!NOTE]
> Para usar su voz personalizada, debe especificar el nombre del modelo de voz, utilizar el URI personalizado directamente en una solicitud HTTP y emplear la misma suscripción para pasar por la autenticación del servicio TTS.

Una vez implementado el punto de conexión, su nombre aparece como un vínculo. Haga clic en el vínculo para mostrar información específica del punto de conexión, como la clave o la dirección URL, y código de ejemplo.

Las pruebas en línea del punto de conexión también están disponibles a través del portal de voz personalizada. Para probar el punto de conexión, elija **Check endpoint** (Comprobar el punto de conexión) en la página **Endpoint detail** (Detalle de punto de conexión). Aparecerá la página para probar los puntos de conexión. Escriba el texto que se hablará (en texto sin formato o [formato SSML](speech-synthesis-markup.md) en el cuadro de texto. Seleccione **Reproducir** para escuchar el texto que se habla en su fuente de voz personalizada. Esta característica de pruebas se les cobrará con el uso de síntesis de voz personalizada.

El punto de conexión personalizado es técnicamente idéntico al punto de conexión estándar que se usa en las solicitudes de texto a voz. Consulte la [API REST](rest-text-to-speech.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

* [Guía: Grabación de muestras de voz](record-custom-voice-samples.md)
* [Referencia de Text-to-Speech API](rest-text-to-speech.md)
* [Long Audio API](long-audio-api.md)
