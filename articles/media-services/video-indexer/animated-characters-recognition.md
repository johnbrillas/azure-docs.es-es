---
title: Detección de personajes animados con Video Indexer
titleSuffix: Azure Media Services
description: En este tema se muestra cómo usar la detección de personajes animados con Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/19/2019
ms.author: juliako
ms.openlocfilehash: f1b19f178a60671108ec32ef6fbed1afe73a0cee
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96854757"
---
# <a name="animated-character-detection-preview"></a>Detección de personajes animados (versión preliminar)

Video Indexer de Azure Media Services admite la detección, la agrupación y el reconocimiento de personajes en contenido animado a través de la [integración con la versión personalizada de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Esta funcionalidad está disponible tanto a través del portal como de la API.

Después de cargar un vídeo animado con un modelo de animación específico, Video Indexer extrae los fotogramas clave, detecta los personajes animados en estos fotogramas, agrupa los personajes similares y elige el mejor ejemplo. A continuación, envía los personajes agrupados a Custom Vision, que identifica los personajes en función de los modelos en los que se entrenó. 

Antes de empezar a entrenar el modelo, los personajes se detectan sin nombre. Al agregar nombres y entrenar el modelo, Video Indexer reconocerá los personajes y les asignará el nombre correspondiente.

## <a name="flow-diagram"></a>Diagrama de flujo

En el siguiente diagrama se muestra el flujo del proceso de detección de personajes animados.

![Diagrama de flujo](./media/animated-characters-recognition/flow.png)

## <a name="accounts"></a>Cuentas

En función de cada tipo de cuenta de Video Indexer, habrá disponibles diferentes conjuntos de características. Para obtener información sobre cómo conectar su cuenta a Azure, consulte [Creación de una cuenta de Video Indexer conectada a Azure](connect-to-azure.md).

* Cuenta de prueba: Video Indexer usa una cuenta de Custom Vision interna para crear el modelo y conectarlo a su cuenta de Video Indexer. 
* Cuenta de pago: la cuenta de Custom Vision se conecta a su cuenta de Video Indexer (si aún no tiene una, primero debe crear una cuenta).

### <a name="trial-vs-paid"></a>De prueba frente a de pago

|Funcionalidad|Versión de prueba|De pago|
|---|---|---|
|Cuenta de Custom Vision|Se administra en segundo plano por Video Indexer. |Su cuenta de Custom Vision está conectada a Video Indexer.|
|Número de modelos de animación|Uno|Hasta 100 modelos por cuenta (limitación de Custom Vision).|
|Entrenamiento del modelo|Video Indexer entrena el modelo para buscar nuevos personajes en otros ejemplos de personajes existentes.|El propietario de la cuenta entrena el modelo cuando está listo para realizar cambios.|
|Opciones avanzadas en Custom Vision|Sin acceso al portal de Custom Vision.|Puede ajustar los modelos usted mismo en el portal de Custom Vision.|

## <a name="use-the-animated-character-detection-with-portal--and-api"></a>Uso de la detección de personajes animados con el portal y la API

Para obtener más información, vea [Uso de la detección de personajes animados con el portal y la API](animated-characters-recognition-how-to.md).

## <a name="limitations"></a>Limitaciones

* Actualmente, la funcionalidad de "identificación de animación" no se admite en la región de Asia Oriental.
* Es posible que los personajes que aparezcan pequeños o lejanos en el vídeo no se identifiquen correctamente si la calidad del vídeo es deficiente.
* Se recomienda usar un modelo por conjunto de personajes animados (por ejemplo, por serie animada).

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Video Indexer](video-indexer-overview.md)