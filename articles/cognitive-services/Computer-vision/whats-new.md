---
title: Novedades de Computer Vision
titleSuffix: Azure Cognitive Services
description: Este artículo describe las novedades de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 01/13/2021
ms.author: pafarley
ms.openlocfilehash: 31ca65099d645b7fdfee70275515f0a864cd905f
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "100007404"
---
# <a name="whats-new-in-computer-vision"></a>Novedades de Computer Vision

Conozca las novedades del servicio. Estos elementos pueden ser notas de la versión, vídeos, entradas de blogs y otros tipos de información. Agregue esta página a sus marcadores para mantenerse actualizado con el servicio.

## <a name="february-2021"></a>Febrero de 2021

### <a name="read-api-v32-public-preview-with-ocr-support-for-73-languages"></a>Versión preliminar pública de Read API v3.2 con compatibilidad con OCR para 73 idiomas
La versión preliminar pública de Read API v3.2 de Computer Vision, disponible como servicio en la nube y como contenedor de Docker, incluye estas actualizaciones:
* [OCR para 73 idiomas](./language-support.md#optical-character-recognition-ocr) entre los que se incluyen chino simplificado y tradicional, japonés, coreano y los idiomas procedentes del latín.
* Orden de lectura natural para la salida de la línea de texto.
* Clasificación de estilos de escritura a mano para líneas de texto junto con una puntuación de confianza (solo para idiomas procedentes del latín).
* Extracción de texto solo de las páginas seleccionadas de un documento.
* Disponible como [contenedor sin distribución](./computer-vision-how-to-install-containers.md?tabs=version-3-2) para la implementación local.

[Más información](concept-recognizing-text.md) acerca de Read API.

> [!div class="nextstepaction"]
> [Use la versión preliminar pública de Read API v3.2](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005)


## <a name="january-2021"></a>Enero de 2021

### <a name="spatial-analysis-container-update"></a>Actualización del contenedor de análisis espacial

Se ha lanzado una nueva versión del [contenedor de análisis espacial](spatial-analysis-container.md) con un nuevo conjunto de características. Este contenedor de Docker permite analizar vídeo de streaming en tiempo real para comprender las relaciones espaciales entre las personas y su movimiento en entornos físicos. 

* Las [operaciones de análisis espaciales](spatial-analysis-operations.md) se pueden configurar para detectar si una persona tiene algún tipo de protección facial, como una mascarilla. 
    * Se puede habilitar un clasificador de mascarillas para las operaciones `personcount`, `personcrossingline` y `personcrossingpolygon` mediante la configuración del parámetro `ENABLE_FACE_MASK_CLASSIFIER`.
    * Los atributos `face_mask` y `face_noMask` se devolverán como metadatos con una puntuación de confianza para cada persona detectada en el flujo de vídeo
* La operación *personcrossingpolygon* se ha ampliado para permitir el cálculo del tiempo que una persona permanece en una zona. Puede establecer el parámetro `type` en la configuración de zona de la operación en `zonedwelltime` y un nuevo evento del tipo *personZoneDwellTimeEvent* incluirá el campo `durationMs` rellenado con el número de milisegundos que la persona estuvo en la zona.
* **Cambio importante**: se ha cambiado el nombre del evento *personZoneEvent* a *personZoneEnterExitEvent*. Este evento lo genera la operación *personZoneEnterExitEvent* cuando una persona entra o sale de la zona y proporciona información direccional con el lado numerado de la zona que se ha atravesado.
* La dirección URL de vídeo se puede proporcionar como "parámetro privado/ofuscado" en todas las operaciones. La ofuscación es opcional ahora y solo funcionará si se proporcionan `KEY` y `IV` como variables de entorno.
* La calibración está habilitada de forma predeterminada para todas las operaciones. Establezca `do_calibration: false` para deshabilitarla.
* Se ha agregado compatibilidad con la recalibración automática (que está deshabilitada de forma predeterminada) mediante el parámetro `enable_recalibration`. Para más información, consulte [Operaciones de análisis espacial](https://docs.microsoft.com/azure/cognitive-services/computer-vision/spatial-analysis-operations).
* Parámetros de calibración de la cámara en `DETECTOR_NODE_CONFIG`. Para más información, consulte [Operaciones de análisis espacial](https://docs.microsoft.com/azure/cognitive-services/computer-vision/spatial-analysis-operations).


## <a name="october-2020"></a>Octubre de 2020

### <a name="computer-vision-api-v31-ga"></a>Disponibilidad general de Computer Vision API v3.1

La versión de Computer Vision API en disponibilidad general se ha actualizado a v3.1.

## <a name="september-2020"></a>Septiembre de 2020

### <a name="spatial-analysis-container-preview"></a>Versión preliminar del contenedor de análisis espacial

El [contenedor de análisis espacial](spatial-analysis-container.md) ahora está en versión preliminar. La característica de análisis espacial de Computer Vision permite analizar vídeo de streaming en tiempo real para comprender las relaciones espaciales entre las personas y su movimiento en entornos físicos. El análisis espacial es un contenedor de Docker que puede usar de forma local. 

### <a name="read-api-v31-public-preview-adds-ocr-for-japanese"></a>La versión preliminar pública de Read API v3.1 agrega OCR para el idioma japonés
La versión preliminar pública de Read API v3.1 de Computer Vision agrega estas funcionalidades:
* OCR para el idioma japonés
* En cada línea de texto, indique si la apariencia es escritura a mano o estilo de impresión, junto con una puntuación de confianza (solo en idiomas procedentes del latín).
* En un documento de varias páginas, extraiga texto solo de las seleccionadas o de un intervalo.

* Esta versión preliminar de Read API admite los idiomas inglés, holandés, francés, alemán, italiano, portugués, chino simplificado y español.

Consulte [Introducción a Read API](concept-recognizing-text.md) para obtener más información.

> [!div class="nextstepaction"]
> [Más información sobre la versión preliminar pública 2 de Read API v3.1](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005)

## <a name="july-2020"></a>Julio de 2020

### <a name="read-api-v31-public-preview-with-ocr-for-simplified-chinese"></a>Versión preliminar pública de Read API v3.1 con OCR para chino simplificado
La versión preliminar de Read API v3.1 de Computer Vision agrega compatibilidad con chino simplificado.

* Esta versión preliminar de Read API admite los idiomas inglés, holandés, francés, alemán, italiano, portugués, chino simplificado y español.

Consulte [Introducción a Read API](concept-recognizing-text.md) para obtener más información.

> [!div class="nextstepaction"]
> [Más información sobre la versión preliminar pública 1 de Read API v3.1](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)

## <a name="may-2020"></a>Mayo de 2020
Computer Vision API v3.0 entró en disponibilidad general, con actualizaciones para [Read API](concept-recognizing-text.md):

* Compatibilidad con inglés, neerlandés, francés, alemán, italiano, portugués y español
* Precisión mejorada
* Puntuación de confianza para cada palabra extraída
* Nuevo formato de salida

## <a name="march-2020"></a>Marzo de 2020

* TLS 1.2 ya se exige en todas las solicitudes HTTP para este servicio. Para más información, consulte [Seguridad de Azure Cognitive Services](../cognitive-services-security.md).

## <a name="january-2020"></a>Enero de 2020

### <a name="read-api-30-public-preview"></a>Read API 3.0 versión preliminar pública

Ahora tiene la opción de usar la versión 3.0 de Read API para extraer texto impreso o manuscrito de imágenes. En comparación con las versiones anteriores, la versión 3.0 proporciona lo siguiente:
* Precisión mejorada
* Nuevo formato de salida
* Puntuación de confianza para cada palabra extraída
* Compatibilidad con español e inglés con el parámetro de idioma adicional.

Siga las indicaciones de una de las guías de [inicio rápido de extracción de texto](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/REST/CSharp-hand-text.md?tabs=version-3) para empezar a usar la API 3.0.

## <a name="cognitive-service-updates"></a>Actualizaciones de Cognitive Service

[Anuncios de actualización de Azure para Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
