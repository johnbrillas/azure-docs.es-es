---
title: Adición de la corrección ortográfica
titleSuffix: Azure Cognitive Search
description: Asocie la corrección ortográfica a la canalización de consulta para corregir los errores tipográficos en los términos de consulta antes de ejecutar la consulta.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: be72cae8d109bfeccd60e8c821625f75357568e3
ms.sourcegitcommit: ec39209c5cbef28ade0badfffe59665631611199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2021
ms.locfileid: "103232365"
---
# <a name="add-spell-check-to-queries-in-cognitive-search"></a>Adición de la corrección ortográfica a las consultas en Cognitive Search

> [!IMPORTANT]
> La corrección ortográfica se encuentra en versión preliminar pública, y solo está disponible mediante la API REST en versión preliminar. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Durante el lanzamiento de la versión preliminar inicial, no se aplica ningún cargo por el corrector ortográfico. Para más información, consulte [Disponibilidad y precios](semantic-search-overview.md#availability-and-pricing).

Se pueden mejorar las coincidencias mediante la corrección ortográfica de los términos de consulta de búsqueda individuales antes de que lleguen al motor de búsqueda. El parámetro **speller** es compatible con todos los tipos de consulta: [simple](query-simple-syntax.md), [completa](query-lucene-syntax.md) y [semántica](semantic-how-to-query-request.md), la nueva opción, actualmente en versión preliminar pública.

## <a name="prerequisites"></a>Requisitos previos

+ Un índice de búsqueda existente, con contenido en inglés

+ Un cliente de búsqueda para el envío de consultas

  El cliente de búsqueda debe admitir las API REST en versión preliminar en la solicitud de consulta. Puede usar [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) o código que haya modificado para realizar llamadas de REST a las API en versión preliminar.

+ [Una solicitud de consulta](/rest/api/searchservice/preview-api/search-documents) que usa la corrección ortográfica tiene "api-version=2020-06-30-Preview", "speller=lexicon" y "queryLanguage=en-us".

  El parámetro queryLanguage es necesario para el corrector ortográfico y actualmente el único valor válido es "en-us".

> [!Note]
> El parámetro speller está disponible en todos los niveles, en las mismas regiones que proporcionan búsqueda semántica. No es necesario suscribirse para acceder a esta característica en versión preliminar. Para más información, consulte [Disponibilidad y precios](semantic-search-overview.md#availability-and-pricing).

## <a name="spell-correction-with-simple-search"></a>Corrección ortográfica con búsqueda simple

En el ejemplo siguiente se usa el ejemplo hotels-sample-index integrado para mostrar la corrección ortográfica en una consulta de texto de formato libre. Sin la corrección ortográfica, la consulta devuelve cero resultados. Con la corrección, la consulta devuelve un resultado para el resort orientado a familias de Johnson.

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "famly acitvites",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "queryType": "simple",
    "select": "HotelId,HotelName,Description,Category,Tags",
    "count": true
}
```

## <a name="spell-correction-with-full-lucene"></a>Corrección ortográfica con sintaxis de Lucene completa

La corrección ortográfica se produce en términos de consulta individuales que se someten a análisis, por lo que puede usar el parámetro speller con algunas consultas de Lucene, pero no con otras.

+ Los formularios de consulta incompatibles que omiten el análisis de texto incluyen: caracteres comodín, expresiones regulares, coincidencias parciales.
+ Los formularios de consulta compatibles incluyen: búsqueda con campo, proximidad, priorización por término.

En este ejemplo se usa la búsqueda con campo en el campo Category, con la sintaxis de Lucene completa y un término de consulta mal escrito. Al incluir el corrector ortográfico, el error ortográfico en "Suiite" se corrige y la consulta se realiza correctamente.

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "Category:(Resort and Spa) OR Category:Suiite",
    "queryType": "full",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "select": "Category",
    "count": true
}
```

## <a name="spell-correction-with-semantic-search"></a>Corrección ortográfica con búsqueda semántica

Esta consulta, con errores tipográficos en todos los términos excepto uno, se somete a correcciones ortográficas para devolver los resultados importantes. Para más información, consulte [Creación de consultas semánticas](semantic-how-to-query-request.md).

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview     
{
    "search": "hisotoric hotell wiht great restrant nad wiifi",
    "queryType": "semantic",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Tags,Description",
    "select": "HotelId,HotelName,Description,Category,Tags",
    "count": true
}
```

## <a name="language-considerations"></a>Consideraciones de idioma

El parámetro queryLanguage necesario para la corrección ortográfica debe ser coherente con los [analizadores del lenguaje](index-add-language-analyzers.md) asignados a las definiciones de campo en el esquema de índice. 

+ Este parámetro determina qué lexicones se usan para la corrección ortográfica. También se emplea como entrada para el [algoritmo de clasificación semántica](semantic-answers.md), si se está utilizando "queryType=semantic".

+ Los analizadores del lenguaje se usan durante la indexación y la ejecución de consultas para recuperar documentos coincidentes en el índice de búsqueda. Un ejemplo de una definición de campo que utiliza un analizador de lenguaje es `"name": "Description", "type": "Edm.String", "analyzer": "en.microsoft"`.

Para conseguir los mejores resultados, si queryLanguage es "en-us", todos los analizadores del lenguaje también deben tener una variante inglesa ("en.microsoft" o "en.lucene").

> [!NOTE]
> Los analizadores independientes del lenguaje (como Keyword, Simple, Standard, Stop, Whitespace o `standardasciifolding.lucene`) no entran en conflicto con la configuración de queryLanguage.

En una solicitud de consulta, el valor de queryLanguage que establezca se aplica por igual al corrector ortográfico, las respuestas y los subtítulos. No hay invalidaciones para partes individuales.

Aunque el contenido de un índice de búsqueda puede estar redactado en varios idiomas, es más probable que la entrada de la consulta lo esté en uno. El motor de búsqueda no comprueba la compatibilidad de queryLanguage, el analizador del lenguaje ni el idioma en que está redactado el contenido, por lo que debe asegurarse de establecer el ámbito de las consultas en consecuencia para evitar que se generen resultados incorrectos.

## <a name="next-steps"></a>Pasos siguientes

+ [Creación de consultas semánticas](semantic-how-to-query-request.md)
+ [Creación de una consulta básica](search-query-create.md)
+ [Uso de la sintaxis de consulta completa de Lucene](query-Lucene-syntax.md)
+ [Uso de sintaxis de consulta simple](query-simple-syntax.md)
+ [Introducción a la búsqueda semántica](semantic-search-overview.md)