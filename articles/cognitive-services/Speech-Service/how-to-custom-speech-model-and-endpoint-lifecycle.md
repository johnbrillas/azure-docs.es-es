---
title: Ciclo de vida de los modelos y los puntos de conexión del servicio de voz de Custom Speech
titleSuffix: Azure Cognitive Services
description: Custom Speech proporciona modelos base adaptables y permite crear modelos personalizados a partir de los datos. En este artículo se describen las escalas de tiempo de los modelos y los puntos de conexión que utilizan estos modelos.
services: cognitive-services
author: heikora
manager: dongli
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/10/2021
ms.author: heikora
ms.openlocfilehash: b8e02071eca139cde02a8bad1b0e0e443db6ab86
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103547962"
---
# <a name="model-and-endpoint-lifecycle"></a>Ciclo de vida del modelo y el punto de conexión

Custom Speech usa tanto *modelos base* como *modelos personalizados*. Cada idioma tiene uno o varios modelos base. Por lo general, cuando se publica un nuevo modelo de voz en el servicio de voz normal, también se importa en el servicio Custom Speech como nuevo modelo base. Se actualizan entre cada 6 y 12 meses. Los modelos anteriores normalmente dejan pierden utilidad con el tiempo, ya que el modelo más reciente suele tener mayor precisión.

En cambio, los modelos personalizados se crean mediante la adaptación de un modelo base elegido con los datos de un escenario de cliente determinado. Una vez que tenga un modelo personalizado que cubra sus necesidades, puede seguir usándolo durante mucho tiempo. Pero se recomienda actualizarlo periódicamente al modelo base más reciente y volver a entrenarlo con datos adicionales. 

Otros términos clave relacionados con el ciclo de vida del modelo incluyen:

* **Adaptación**: tomar un modelo base y personalizarlo para el dominio o escenario propios mediante datos de texto o datos de audio.
* **Descodificación**: usar un modelo y realizar el reconocimiento de voz (descodificar audio en texto).
* **Punto de conexión**: una implementación específica del usuario, bien de un modelo base, bien de un modelo personalizado al que *solo* puede acceder un usuario dado.

### <a name="expiration-timeline"></a>Escala de tiempo de expiración

A medida que los nuevos modelos y la nueva funcionalidad pasan a estar disponibles, se retiran los modelos más antiguos y menos precisos. Consulte las siguientes escalas de tiempo de expiración del modelo y el punto de conexión:

**Modelos base** 

* Adaptación: disponibles durante un año. Una vez que se importa el modelo, está disponible durante un año para crear modelos personalizados. Después de un año, deben crearse nuevos modelos personalizados a partir de una versión más reciente del modelo base.  
* Descodificación: disponibles durante dos años después de la importación. Por consiguiente, puede crear un punto de conexión y usar la transcripción por lotes durante dos años con este modelo. 
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

**Modelos personalizados**

* Descodificación: disponible durante dos años después de la creación del modelo. Por consiguiente, el modelo personalizado se puede usar durante dos años (lote, tiempo real y pruebas) una vez creado. Después de dos años, *debe volver a entrenar el modelo*, ya que la mayoría de las veces el modelo base habrá quedado en desuso para la adaptación.  
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

Cuando expira un modelo base o un modelo personalizado, siempre se revierte a la *versión más reciente del modelo base*. Por tanto, la implementación nunca se interrumpirá, pero puede ser menos precisa para *sus datos específicos* si los modelos personalizados llegan a expirar. Puede ver la expiración de un modelo en los siguientes lugares del portal de Habla personalizada de Speech Studio:

* Resumen del entrenamiento del modelo
* Detalles del entrenamiento del modelo
* Resumen de implementación
* Detalles de la implementación

Este es un formulario de ejemplo del resumen del entrenamiento del modelo:

![Resumen de entrenamiento del modelo ](media/custom-speech/custom-speech-model-training-with-expiry.png) Y también de la página de detalles de entrenamiento del modelo:

![Detalles del entrenamiento del modelo](media/custom-speech/custom-speech-model-details-with-expiry.png)

También puede comprobar las fechas de expiración mediante las API [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) y [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) de habla personalizada, en la propiedad `deprecationDates` de la respuesta JSON.

Este es un ejemplo de los datos de expiración de la llamada de API GetModel. "DEPRECATIONDATES" muestra: 
```json
{
    "SELF": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}",
    "BASEMODEL": {
    "SELF": HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/BASE/{id}
    },
    "DATASETS": [
    {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/datasets/{id}
    }
    ],
    "LINKS": {
    "MANIFEST": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}/MANIFEST",
    "COPYTO": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}/copyto
    },
    "PROJECT": {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/projects/{id}
    },
    "PROPERTIES": {
    "DEPRECATIONDATES": {
        "ADAPTATIONDATETIME": "2022-01-15T00:00:00Z",     // last date this model can be used for adaptation
        "TRANSCRIPTIONDATETIME": "2023-03-01T21:27:29Z"   // last date this model can be used for decoding
    }
    },
    "LASTACTIONDATETIME": "2021-03-01T21:27:40Z",
    "STATUS": "SUCCEEDED",
    "CREATEDDATETIME": "2021-03-01T21:27:29Z",
    "LOCALE": "EN-US",
    "DISPLAYNAME": "EXAMPLE MODEL",
    "DESCRIPTION": "",
    "CUSTOMPROPERTIES": {
    "PORTALAPIVERSION": "3"
    }
}
```
Tenga en cuenta que puede actualizar el modelo en un punto de conexión de habla personalizada sin tiempo de inactividad si cambia el modelo usado por el punto de conexión en la sección de implementación de Speech Studio o por medio de la API de habla personalizada.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento e implementación de un modelo](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparación y prueba de los datos](./how-to-custom-speech-test-and-train.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)