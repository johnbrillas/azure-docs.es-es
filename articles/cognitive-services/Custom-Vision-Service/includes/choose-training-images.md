---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: d07a5da3b9013700694f6c20102ef2e8c5066087
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98256214"
---
Como mínimo, se recomienda que use 30 imágenes por etiqueta en el conjunto de entrenamiento inicial. También conviene recopilar algunas imágenes adicionales para probar el modelo una vez que está entrenado.

Para entrenar el modelo de forma eficaz, use imágenes con variedad visual. Seleccione imágenes que varíen en:
* ángulos de cámara
* iluminación
* background
* estilo visual
* sujetos individuales o grupos
* tamaño
* type

Además, asegúrese de que todas las imágenes de entrenamiento cumplen los criterios siguientes:
* formato .jpg, .png, .bmp o .gif
* tienen menos de 6 MB de tamaño (4 MB en el caso de imágenes de predicción)
* tienen más de 256 píxeles en el borde más corto. Custom Vision Service escalará verticalmente y de forma automática todas las imágenes que sean más cortas

> [!NOTE]
> Trove, un proyecto de Microsoft Garage, le permite recopilar y comprar conjuntos de imágenes con fines de aprendizaje. Una vez que haya recopilado sus imágenes, puede descargarlas y, a continuación, importarlas en el proyecto de Custom Vision de la manera habitual. Visite la [página de Trove](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3) para más información.