---
title: Estructura de las respuestas semánticas
titleSuffix: Azure Cognitive Search
description: Em este artículo de describe el algoritmo de clasificación semántica de Cognitive Search y cómo estructurar "respuestas semánticas" y "subtítulos semánticos" de un conjunto de resultados.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: febbfd0f3def8e681107ef78d9478463b1c2a872
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678930"
---
# <a name="semantic-ranking-and-responses-in-azure-cognitive-search"></a>Clasificación y respuestas semánticas en Azure Cognitive Search

> [!IMPORTANT]
> El algoritmo de clasificación semántica y los subtítulos y respuestas semánticos se encuentran en versión preliminar pública, y solo están disponibles mediante la API REST en versión preliminar. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La clasificación semántica mejora la precisión de los resultados de búsqueda al volver a clasificar las coincidencias principales mediante un modelo de clasificación semántica entrenado para consultas expresadas en lenguaje natural por oposición a las palabras clave.

En este artículo se describe el algoritmo de clasificación semántica y cómo se modela una respuesta semántica. Las respuestas incluyen subtítulos, en texto sin formato y con resaltados, y respuestas (opcional).

+ Los subtítulos semánticos son pasajes de texto pertinentes para la consulta extraída de los resultados de búsqueda. Pueden ayudar a resumir un resultado cuando los campos de contenido individuales son demasiado grandes para la página de resultados. Los subtítulos destacan la semántica, lo que permite a los usuarios hojear rápidamente los resultados de la consulta para encontrar los documentos más pertinentes, lo cual mejora la experiencia de usuario global.

+ Las respuestas semánticas usan modelos de aprendizaje automático de Bing para formular respuestas a consultas que se parecen a las preguntas. Las respuestas se seleccionan de una lista de los pasajes más pertinentes para la consulta, tal como se extraen de los principales documentos del conjunto de resultados de la consulta. Las respuestas se devuelven como un objeto de nivel superior e independiente en la carga de respuesta de la consulta, el cual puede elegir para su representación en las páginas de búsqueda, junto con los resultados de búsqueda.

## <a name="prerequisites"></a>Prerrequisitos

+ Consultas formuladas mediante el tipo de consulta semántica. Para más información, consulte [Creación de consultas semánticas](semantic-how-to-query-request.md).

## <a name="understanding-a-semantic-response"></a>Descripción de una respuesta semántica

Una respuesta semántica incluye nuevas propiedades para puntuaciones, subtítulos y respuestas. Una respuesta semántica se genera a partir de la respuesta estándar, donde el [motor de búsqueda de texto completo](search-lucene-query-architecture.md) devuelve los primeros 50 resultados, que, luego, se vuelven a clasificar mediante el clasificador semántico. Si se especifican más de 50, se devuelven los resultados adicionales, pero no se vuelven a clasificar semánticamente.

Al igual que con todas las consultas, una respuesta consta de todos los campos marcados como recuperables, o solo de los campos enumerados en la instrucción SELECT. También incluye un campo de respuesta y subtítulos.

+ Cada resultado semántico tiene, de forma predeterminada, una respuesta que se devuelve como un campo distinto que puede elegir representar en una página de búsqueda. Puede especificar hasta cinco. La formulación de la respuesta es automatizada: se leen todos los documentos de los resultados iniciales, se ejecuta un resumen extractivo seguido de una comprensión lectora automática y, finalmente, se promueve una respuesta directa a la pregunta del usuario en el campo de respuesta.

+ Un subtítulo es un resumen basado en la extracción del contenido del documento, devuelto en texto sin formato o con resaltados. Los subtítulos se incluyen automáticamente y no se pueden suprimir. Los resaltados se aplican mediante la comprensión lectora automática para identificar qué cadenas deben enfatizarse. Lo que hacen es atraer la atención a los pasajes más importantes, de modo que puede examinar rápidamente una página de resultados hasta encontrar el documento correcto.

Un conjunto de resultados vuelto a clasificar semánticamente ordena los resultados por el valor @search.rerankerScore. Si agrega una cláusula OrderBy, se devolverá un error HTTP 400 porque esa cláusula no se admite en las consultas semánticas.

## <a name="example"></a>Ejemplo

El valor @search.rerankerScore existe junto con el estándar @search.score y se usa para volver a clasificar los resultados.

Dada la siguiente consulta:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "answers": "none",
    "searchFields": "HotelName,Category,Description",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

Puede esperar ver el resultado siguiente, representativo de una respuesta semántica. Estos resultados muestran solo las tres respuestas principales, clasificadas por "@search.rerankerScore". Observe cómo Oceanside Resort ahora aparece en primer lugar. En la clasificación predeterminada de "@search.score", este resultado se devolvería en segundo lugar, después de Trails End.

```http
{
    "@odata.count": 31,
    "@search.answers": [],
    "value": [
        {
            "@search.score": 1.8920634,
            "@search.rerankerScore": 1.1091284966096282,
            "@search.captions": [
                {
                    "text": "Oceanside Resort. Budget. New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
                    "highlights": "<em>Oceanside Resort.</em> Budget. New Luxury Hotel.  Be the first to stay.<em> Bay views</em> from every room, location near the piper, rooftop pool, waterfront dining & more."
                }
            ],
            "HotelId": "18",
            "HotelName": "Oceanside Resort",
            "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
            "Category": "Budget"
        },
        {
            "@search.score": 2.5204072,
            "@search.rerankerScore": 1.0731962407007813,
            "@search.captions": [
                {
                    "text": "Trails End Motel. Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
                    "highlights": "<em>Trails End Motel.</em> Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
                }
            ],
            "HotelId": "40",
            "HotelName": "Trails End Motel",
            "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
            "Category": "Luxury"
        },
        {
            "@search.score": 1.4104348,
            "@search.rerankerScore": 1.06992666143924,
            "@search.captions": [
                {
                    "text": "Winter Panorama Resort. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
                    "highlights": "<em>Winter Panorama Resort</em>. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs."
                }
            ],
            "HotelId": "12",
            "HotelName": "Winter Panorama Resort",
            "Description": "Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
            "Category": "Resort and Spa"
        }
```

## <a name="next-steps"></a>Pasos siguientes

+ [Introducción a la búsqueda semántica](semantic-search-overview.md)
+ [Algoritmo de similitud](index-ranking-similarity.md)
+ [Creación de consultas semánticas](semantic-how-to-query-request.md)
+ [Creación de una consulta básica](search-query-create.md)