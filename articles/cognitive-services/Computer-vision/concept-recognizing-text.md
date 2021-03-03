---
title: Reconocimiento óptico de caracteres (OCR) en Computer Vision
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento óptico de caracteres (OCR) de imágenes y documentos que contienen texto impreso y manuscrito mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 2833fd44b75f4bebf41b5100eb2350ca69436520
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100362801"
---
# <a name="optical-character-recognition-ocr"></a>Reconocimiento óptico de caracteres (OCR)

Computer Vision API de Azure incluye funcionalidades para el reconocimiento óptico de caracteres (OCR) que permiten extraer texto impreso o manuscrito de imágenes. Puede extraer texto de imágenes, como fotos de matrículas o de contenedores con números de serie, así como de documentos; por ejemplo, facturas, recibos, informes financieros y artículos, entre otros.

## <a name="read-api"></a>Read API 

Computer Vision [Read API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) es la tecnología de OCR más reciente de Azure ([información sobre las novedades](./whats-new.md)) que permite extraer texto impreso (en varios idiomas), texto manuscrito (solo en inglés), dígitos y símbolos de divisa de imágenes, y documentos PDF con varias páginas. Esta tecnología está optimizada para extraer texto de imágenes con mucho texto y de documentos PDF con varias páginas y una mezcla de idiomas. Es compatible con la detección de texto impreso y manuscrito en un mismo documento o una misma imagen.

![Cómo OCR convierte imágenes y documentos en una salida estructurada con texto extraído](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Requisitos de entrada
La llamada a **Read** usa las imágenes y los documentos como entrada. Tienen los siguientes requisitos:

* Formatos de archivos admitidos: JPEG, PNG, BMP, PDF y TIFF.
* En el caso de los archivos PDF y TIFF, se procesan hasta 2000 páginas (solo las primeras dos páginas en el nivel Gratis).
* El tamaño de archivo debe ser inferior a 50 MB (4 MB para el nivel Gratis); y sus dimensiones, de al menos 50x50 píxeles y, como máximo, de 10 000x10 000 píxeles. 
* Los archivos PDF deben tener unas dimensiones de 17x17 pulgadas como máximo, lo que se corresponde con los tamaños de papel Legal o A3 y más pequeños.

> [!NOTE]
> **Entrada de idioma** 
>
> La [llamada a Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) tiene un parámetro de solicitud opcional para el idioma. La lectura admite la identificación automática del idioma y documentos multilingües, por lo que solo debe proporcionar un código de idioma si desea forzar el procesamiento del documento como ese idioma específico.

## <a name="ocr-demo-examples"></a>Demostración de OCR (ejemplos)

![Demostraciones de OCR](./Images/ocr-demo.gif)

## <a name="step-1-the-read-operation"></a>Paso 1: Operación de lectura

La [llamada a Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) de Read API toma como entrada una imagen o un documento PDF y extrae texto de forma asincrónica. La llamada se devuelve un campo de encabezado de respuesta denominado `Operation-Location`. El valor `Operation-Location` es una dirección URL que contiene el identificador de la operación que se va a usar en el paso siguiente.

|Encabezado de respuesta| Dirección URL del resultado |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Facturación** 
>
> La página de [precios de Computer Vision](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) incluye el plan de tarifa de Read. Cada imagen o página analizada cuenta como una transacción. Si llama a la operación con un documento PDF o TIFF con 100 páginas, la operación de Read la contabilizará como 100 transacciones para su facturación. Si realizó 50 llamadas a la operación, y cada llamada envió un documento con 100 páginas, se facturarán 50x100 = 5000 transacciones.

## <a name="step-2-the-get-read-results-operation"></a>Paso 2: Operación de obtención de resultados de lectura

El segundo paso es llamar a la operación [Get Read Results](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750). Esta operación toma como entrada el identificador de operación que la operación de lectura ha creado. Devuelve una respuesta JSON que contiene un campo de **estado** con los siguientes valores posibles. Llamará a esta operación de forma iterativa hasta que se devuelva con el valor **correcto**. Use un intervalo de 1 a 2 segundos para evitar superar la tasa de solicitudes por segundo (RPS).

|Campo| Tipo | Valores posibles |
|:-----|:----:|:----|
|status | string | notStarted: la operación no se ha iniciado. |
| |  | running: la operación se está procesando. |
| |  | failed: Se produjeron errores en la operación. |
| |  | succeeded: La operación se ha realizado correctamente. |

> [!NOTE]
> El nivel gratuito limita la tasa de solicitudes a 20 llamadas por minuto. El nivel de pago permite 10 solicitudes por segundo (RPS) que pueden aumentar a petición. Use el canal de soporte técnico de Azure o contacte con el equipo de cuentas para solicitar una tasa de solicitudes por segundo (RPS) más alta.

Cuando el campo **estado** tiene el valor **succeeded** (correcto), la respuesta JSON contiene el contenido de texto extraído de la imagen o el documento. La respuesta en JSON mantiene las agrupaciones de líneas originales de palabras reconocidas. Incluye las líneas de texto extraídas y las coordenadas del cuadro de límite correspondientes. Cada línea de texto incluye todas las palabras extraídas, con las coordenadas y las puntuaciones de confianza respectivas.

> [!NOTE]
> Los datos enviados a la operación `Read` se cifran temporalmente y se almacenan en reposo durante un corto tiempo y después se eliminan. Esto permite que las aplicaciones recuperen el texto extraído como parte de la respuesta del servicio.

## <a name="sample-json-output"></a>Salida de JSON de ejemplo

Consulte el siguiente ejemplo de una respuesta JSON correcta:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

## <a name="natural-reading-order-output-latin-only"></a>Salida de orden de lectura natural (solo idiomas procedentes del latín)
Con [Read API 3.2 versión preliminar](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005), especifique el orden en que se generan las líneas de texto con el parámetro de consulta `readingOrder`. Use `natural` si quiere obtener una salida de orden de lectura más natural, como se muestra en el ejemplo siguiente. Esta característica solo es compatible con los idiomas procedentes del latín.

:::image border type="content" source="./Images/ocr-reading-order-example.png" alt-text="Ejemplo de orden de lectura de OCR":::

## <a name="handwritten-classification-for-text-lines-latin-only"></a>Clasificación manuscrita de líneas de texto (solo para idiomas derivados del latín)
La respuesta de [Read API 3.2 versión preliminar](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005) incluye la clasificación de si cada línea de texto es de estilo manuscrito o no, junto con una puntuación de confianza. Esta característica solo es compatible con los idiomas procedentes del latín. En el ejemplo siguiente se muestra la clasificación manuscrita del texto de la imagen.

:::image border type="content" source="./Images/ocr-handwriting-classification.png" alt-text="Ejemplo de clasificación de escritura a mano OCR":::

## <a name="select-pages-or-page-ranges-for-text-extraction"></a>Selección de las páginas o de los intervalos de páginas para la extracción de texto
Con [Read API 3.2 versión preliminar](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005), en el caso de documentos grandes de varias páginas, use el parámetro de consulta `pages` para especificar números de página o intervalos de páginas para extraer texto solo de esas páginas. En el ejemplo siguiente se muestra un documento con 10 páginas, con texto extraído para ambos casos: todas las páginas (1-10) y las páginas seleccionadas (3-6).

:::image border type="content" source="./Images/ocr-select-pages.png" alt-text="Salida de páginas seleccionadas":::

## <a name="supported-languages"></a>Idiomas compatibles
Las instancias de Read API admiten un total de 73 idiomas en texto de estilo impreso. Consulte la lista completa de [idiomas admitidos por OCR](./language-support.md#optical-character-recognition-ocr). El OCR de estilo manuscrito se admite exclusivamente en inglés.

## <a name="use-the-cloud-api-or-deploy-on-premise"></a>Uso de Cloud API o implementación local
Las instancias de Read Cloud API 3.x son la opción preferida para la mayoría de los clientes debido a su facilidad de integración y su inmediata productividad. Azure y el servicio Computer Vision controlan las necesidades de escalado, rendimiento, seguridad de los datos y cumplimiento, lo que le permite centrarse en satisfacer las necesidades de los clientes.

En las implementaciones locales, el [contenedor de Docker de Read (versión preliminar)](./computer-vision-how-to-install-containers.md) le permite implementar las nuevas funcionalidades de OCR en su entorno local. Los contenedores son excelentes para requisitos específicos de control de datos y seguridad.

## <a name="ocr-api"></a>API de OCR

La [API de OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d) usa un modelo de reconocimiento anterior, solo admite imágenes y se ejecuta de forma sincrónica, así que devuelve el texto detectado inmediatamente. Consulte los [idiomas admitidos por OCR](./language-support.md#optical-character-recognition-ocr) y Read API.

> [!NOTE]
> Las operaciones de reconocimiento de texto de Computer Vision 2.0 pronto estarán en desuso en favor de la nueva Read API de la que se habla en este artículo. Los clientes existentes deben [realizar la transición a operaciones de lectura](upgrade-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

- Introducción a las [guías de inicio rápido de la API REST o de la biblioteca cliente de Computer Vision](./quickstarts-sdk/client-library.md).
- Obtenga información sobre la [API REST de Read 3.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005).
- Conozca [Read API REST 3.2 versión preliminar pública](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-2/operations/5d986960601faab4bf452005) y su compatibilidad con un total de 73 idiomas.
