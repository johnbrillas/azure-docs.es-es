---
title: 'Diseños: Form Recognizer'
titleSuffix: Azure Cognitive Services
description: 'Conozca los conceptos relacionados con el análisis de diseños mediante API Form Recognizer: uso y límites.'
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: lajanuar
ms.openlocfilehash: 73bef21f430bde1c6c2c95d7c3f685cccbbd9179
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103467024"
---
# <a name="form-recognizer-layout-service"></a>Servicio Layout de Form Recognizer

Azure Form Recognizer puede extraer texto, tablas, marcas de selección e información de la estructura de los documentos mediante su servicio Layout. La API Layout permite a los clientes tomar documentos de varios formatos y devolver representaciones de datos estructurados de los documentos. Combina nuestras eficaces funcionalidades de [reconocimiento óptico de caracteres (OCR)](../computer-vision/concept-recognizing-text.md) con modelos de aprendizaje profundo para extraer texto, tablas, marcas de selección e información de la estructura de los documentos. 

## <a name="what-does-the-layout-service-do"></a>¿Qué hace el servicio Layout?

La API Layout extrae texto, tablas, marcas de selección e información de la estructura de documentos con una precisión excepcional y los devuelve con una respuesta JSON estructurada organizada. Los documentos pueden estar en varios formatos y tener distintos grados de calidad, lo que incluye imágenes capturadas por un teléfono, documentos digitalizados y archivos PDF digitales. La API Layout extraerá con precisión la salida estructurada de todos estos documentos.

![Ejemplo de diseño](./media/layout-tool-example.JPG)

## <a name="try-it-out"></a>Prueba

Para probar el servicio Layout de Form Recognizer, vaya a la herramienta de interfaz de usuario de ejemplo en línea:

> [!div class="nextstepaction"]
> [Herramienta de prueba de OCR de formulario (FOTT)](https://fott-preview.azurewebsites.net)

Necesitará una suscripción de Azure ([cree una gratis](https://azure.microsoft.com/free/cognitive-services)) y un punto de conexión de [recursos de Form Recognizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer), así como la clave para probar la API Layout de Form Recognizer. 

![Captura de pantalla de la interfaz de usuario de ejemplo; se analizan el texto, las tablas y las marcas de selección de un documento](./media/analyze-layout.png)

### <a name="input-requirements"></a>Requisitos de entrada 

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-layout-operation"></a>La operación Analyze Layout

En primer lugar, llame la operación [Análisis de diseño](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/AnalyzeLayoutAsync). El Análisis de diseño toma un documento (archivo de imagen, TIFF o PDF) como entrada y extrae el texto, las tablas, las marcas de selección y la estructura del mismo. La llamada devuelve un campo de encabezado de respuesta denominado `Operation-Location`. El valor `Operation-Location` es una dirección URL que contiene el id. de resultado que se va a usar en el paso siguiente.

|Encabezado de respuesta| Dirección URL del resultado |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.3/prebuilt/layout/analyzeResults/44a436324-fc4b-4387-aa06-090cfbf0064f` |

### <a name="natural-reading-order-output-latin-only"></a>Salida de orden de lectura natural (solo idiomas procedentes del latín)

Puede especificar el orden en que se generan las líneas de texto con el parámetro de consulta `readingOrder`. Use `natural` si quiere obtener una salida de orden de lectura más natural, como se muestra en el ejemplo siguiente. Esta característica solo es compatible con los idiomas procedentes del latín.

:::image type="content" source="../Computer-vision/Images/ocr-reading-order-example.png" alt-text="Ejemplo de orden del diseño de lectura" lightbox="../Computer-vision/Images/ocr-reading-order-example.png":::

### <a name="select-page-numbers-or-ranges-for-text-extraction"></a>Seleccione los intervalos o los números de páginas para la extracción de texto

En el caso de documentos de varias páginas de gran tamaño, use el parámetro de consulta `pages`para indicar números de página o intervalos de páginas específicos para la extracción de texto. En el ejemplo siguiente se muestra un documento con 10 páginas, con texto extraído para ambos casos: todas las páginas (1-10) y las páginas seleccionadas (3-6).

:::image type="content" source="../Computer-vision/Images/ocr-select-pages.png" alt-text="Salida de páginas seleccionadas de diseño":::

## <a name="the-get-analyze-layout-result-operation"></a>La operación Get Analyze Layout Result

El segundo paso consiste en llamar a la operación [Get Analyze Layout Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/GetAnalyzeLayoutResult). Esta operación toma como entrada el identificador de resultado que la operación Analyze Layout ha creado. Devuelve una respuesta JSON que contiene un campo de **estado** con los siguientes valores posibles. 

|Campo| Tipo | Valores posibles |
|:-----|:----:|:----|
|status | string | `notStarted`: la operación de análisis no se ha iniciado.<br /><br />`running`: la operación de análisis está en curso.<br /><br />`failed`: error en la operación de análisis.<br /><br />`succeeded`: la operación de análisis se realizó correctamente.|

Llame esta operación de forma iterativa hasta que se devuelva con el valor `succeeded`. Use un intervalo de 3 a 5 segundos para evitar superar la tasa de solicitudes por segundo (RPS).

Cuando el campo de **estado** tiene el valor `succeeded`, la respuesta de JSON incluirá el diseño, el texto, las tablas y las marcas de selección extraídos. Los datos extraídos incluyen las líneas de texto y palabras extraídas, los rectángulos delimitadores, la aparición de texto con una indicación de texto manuscrito, las tablas y las marcas de selección indicando si se ha seleccionado o no. 

### <a name="handwritten-classification-for-text-lines-latin-only"></a>Clasificación manuscrita de líneas de texto (solo para idiomas derivados del latín)

La respuesta incluye la clasificación de si cada línea de texto es de estilo manuscrito o no, junto con una puntuación de confianza. Esta característica solo es compatible con los idiomas procedentes del latín. En el ejemplo siguiente se muestra la clasificación manuscrita del texto de la imagen.

:::image type="content" source="../Computer-vision/Images/ocr-handwriting-classification.png" alt-text="Ejemplo de clasificación de escritura a mano":::

### <a name="sample-json-output"></a>Salida de JSON de ejemplo

La respuesta a la operación *Get Analyze Layout Result* es una representación estructurada del documento con toda la información extraída. Aquí encontrará un [archivo con un documento de ejemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout.pdf) y su salida estructurada, una [salida de un diseño de ejemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout-output.json).

La salida JSON tiene dos partes:

* El nodo `readResults` contiene todo el texto reconocido y las marcas de selección. El texto se organiza por página, después, por líneas y, finalmente, por palabras individuales. 
* El nodo `pageResults` contiene las tablas y celdas extraídas con sus rectángulos delimitadores, su confianza y una referencia a las líneas y palabras de "readResults".

## <a name="example-output"></a>Ejemplo de salida

### <a name="text"></a>Texto

La API Layout extrae texto de documentos (PDF, TIFF) e imágenes (JPG, PNG, BMP) con varios ángulos y colores de fuente. Es compatible con fotografías de documentos, faxes, texto impreso o manuscrito (solo en inglés) y modos mixtos. El texto se extrae con información sobre las líneas, palabras, rectángulos delimitadores, puntuaciones de confianza y estilo (manuscrito u otro). Toda la información del texto se incluye en la sección `readResults` de la salida JSON. 

### <a name="tables"></a>Tablas

La API Layout extrae tablas de documentos (PDF y TIFF) e imágenes (JPG, PNG y BMP). Los documentos se pueden digitalizar o fotografiar. Las tablas pueden ser complejas con celdas o columnas combinadas, con o sin bordes, y con ángulos impares. La información de las tablas extraídas incluyen el número de columnas y filas, el intervalo de filas y el intervalo de columnas. Cada celda se extrae con su cuadro de límite y referencia al texto extraído en la sección `readResults`. La información de la tabla se encuentra en la sección `pageResults` de la salida JSON. 

![Tablas de ejemplo](./media/tables-example.jpg)

### <a name="selection-marks"></a>Marcas de selección

La API Layout también extrae marcas de selección de los documentos. Entre las marcas de selección extraídas se incluyen el rectángulo delimitador, la confianza y el estado (seleccionado o no seleccionado). La información de la marca de selección se extrae en la sección `readResults` de la salida JSON. 

## <a name="next-steps"></a>Pasos siguientes

* Pruebe su propia extracción de diseño mediante la [herramienta de interfaz de usuario de ejemplo de Form Recognizer](https://fott-preview.azurewebsites.net/)
* Realice un [inicio rápido de Form Recognizer](quickstarts/client-library.md) para empezar a extraer diseños en el lenguaje de desarrollo que prefiera.

## <a name="see-also"></a>Consulte también

* [¿Qué es Form Recognizer?](./overview.md)
* [Documentos de referencia de la API de REST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/AnalyzeLayoutAsync)
