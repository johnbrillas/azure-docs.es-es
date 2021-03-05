---
title: Creación de consultas semánticas
titleSuffix: Azure Cognitive Search
description: Establezca un tipo de consulta semántica para asociar los modelos de aprendizaje profundo al procesamiento de consultas, y así inferir la intención y el contexto dentro de la clasificación y la relevancia de la búsqueda.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 7551ef88c2251b64cf6f6db1de4fed22db2c69e2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693652"
---
# <a name="create-a-semantic-query-in-cognitive-search"></a>Creación de consultas semánticas en Cognitive Search

> [!IMPORTANT]
> El tipo de consulta semántica se encuentra en versión preliminar pública, y está disponible mediante la API REST en versión preliminar y Azure Portal. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Durante el lanzamiento de la versión preliminar inicial, no se aplica ningún cargo por la búsqueda semántica. Para más información, consulte [Disponibilidad y precios](semantic-search-overview.md#availability-and-pricing).

En este artículo, aprenderá a formular una solicitud de búsqueda que usa la clasificación semántica y generará subtítulos y respuestas semánticos.

## <a name="prerequisites"></a>Requisitos previos

+ Un servicio de búsqueda en un nivel Estándar (S1, S2, S3), ubicado en una de estas regiones: Centro-norte de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Este de EE. UU. 2, Europa del Norte y Oeste de Europa. Si tiene un servicio S1 o superior en una de estas regiones, puede solicitar acceso sin tener que crear un servicio.

+ Acceso a la versión preliminar de la búsqueda semántica: [registro](https://aka.ms/SemanticSearchPreviewSignup)

+ Un índice de búsqueda existente, con contenido en inglés

+ Un cliente de búsqueda para el envío de consultas

  El cliente de búsqueda debe admitir las API REST en versión preliminar en la solicitud de consulta. Puede usar [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) o código que haya modificado para realizar llamadas de REST a las API en versión preliminar. También puede usar el [Explorador de búsqueda](search-explorer.md) de Azure Portal para enviar consultas semánticas.

+ Una solicitud a [Search Documents](/rest/api/searchservice/preview-api/search-documents) con la opción semántica y otros parámetros descritos en este artículo.

## <a name="whats-a-semantic-query"></a>¿Qué es una consulta semántica?

En Cognitive Search, una consulta es una solicitud parametrizada que determina el procesamiento de las consultas y la forma de las respuestas. Una *consulta semántica* agrega parámetros que invocan el algoritmo de reclasificación semántica. Este algoritmo puede evaluar el contexto y el significado de los resultados coincidentes y promover los mejores a los primeros puestos.

La solicitud siguiente es representativa de una consulta semántica básica (sin respuestas).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us",  
}
```

Como sucede con todas las consultas de Cognitive Search, la solicitud va dirigida a la colección de documentos de un índice único. Además, las consultas semánticas pasan por la misma secuencia de análisis y examen que una consulta no semántica. La diferencia radica en cómo se calcula la relevancia. Tal como se define en esta versión preliminar, una consulta semántica es aquella cuyos *resultados* se vuelven a procesar mediante algoritmos avanzados, lo que proporciona una manera de mostrar las coincidencias que el clasificador semántico considera de más importancia, en lugar de las puntuaciones que asigna el algoritmo predeterminado de clasificación de similitudes. 

Solo las 50 primeras coincidencias de los resultados iniciales se pueden clasificar semánticamente y todas incluyen subtítulos en la respuesta. Opcionalmente, puede especificar un parámetro **`answer`** en la solicitud para extraer una posible respuesta. Este modelo formula hasta cinco posibles respuestas a la consulta, que puede elegir representar en la parte superior de la página de búsqueda.

## <a name="query-using-rest-apis"></a>Consulta mediante las API REST

La especificación completa de la API REST se puede encontrar en [Search Documents (versión preliminar de REST)](/rest/api/searchservice/preview-api/search-documents).

Las consultas semánticas están pensadas para preguntas abiertas del tipo: "¿cuál es la mejor planta para los polinizadores?" o "¿cómo freír un huevo?". Si quiere que en la contestación se incluyan las respuestas, puede agregar un parámetro **`answer`** opcional en la solicitud.

En el ejemplo siguiente se usa hotels-sample-index para crear una solicitud de consulta semántica con respuestas y subtítulos semánticos:

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview      
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Category,Description",
    "speller": "lexicon",
    "answers": "extractive|count-3",
    "highlightPreTag": "<strong>",
    "highlightPostTag": "</strong>",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

### <a name="formulate-the-request"></a>Formulación de la solicitud

1. Establezca **`"queryType"`** en "semantic" y **`"queryLanguage"`** en "en-us". Ambos parámetros son obligatorios.

   El parámetro queryLanguage debe ser coherente con los [analizadores del lenguaje](index-add-language-analyzers.md) asignados a las definiciones de campo en el esquema de índice. Si queryLanguage es "en-us", todos los analizadores del lenguaje también deben tener una variante inglesa ("en.microsoft" o "en.lucene"). Los analizadores independientes del lenguaje, como Keyword o Simple, no tienen ningún conflicto con los valores de queryLanguage.

   En una solicitud de consulta, si usa también la [corrección ortográfica](speller-how-to-add.md), el valor de queryLanguage que establezca se aplica por igual al corrector ortográfico, a las respuestas y a los subtítulos. No hay invalidaciones para partes individuales. 

   Aunque el contenido de un índice de búsqueda puede estar redactado en varios idiomas, es más probable que la entrada de la consulta lo esté en uno. El motor de búsqueda no comprueba la compatibilidad de queryLanguage, el analizador del lenguaje ni el idioma en que está redactado el contenido, por lo que debe asegurarse de establecer el ámbito de las consultas en consecuencia para evitar que se generen resultados incorrectos.

<a name="searchfields"></a>

1. Establezca **`"searchFields"`** (opcional, pero recomendable).

   En una consulta semántica, el orden de los campos en "searchFields" refleja la prioridad o la importancia relativa del campo en las clasificaciones semánticas. Solo se usan campos de cadena de nivel superior (solos o de una colección). Dado que searchFields se comporta de forma diferente en las consultas de Lucene simples y completas (donde no hay un orden de prioridad implícito), otros campos y subcampos que no sean de cadena no producirán un error, pero tampoco se usarán en la clasificación semántica.

   Al especificar searchFields, siga estas instrucciones:

   + Los campos concisos, como HotelName o un título, deben preceder a los campos detallados, como Description.

   + Si el índice tiene un campo de dirección URL que es textual (legible por el usuario, como `www.domain.com/name-of-the-document-and-other-details` y no está enfocado a la máquina como `www.domain.com/?id=23463&param=eis`), colóquelo en segundo lugar en la lista (o en primer lugar si no hay ningún campo de título conciso).

   + Si solo se especifica un campo, se considerará un campo descriptivo para la clasificación semántica de los documentos.  

   + Si no se especifica ningún campo, todos los campos de búsqueda se tienen en cuenta para la clasificación semántica de los documentos. Sin embargo, esta opción no se recomienda, ya que puede que el índice de búsqueda no produzca los resultados más óptimos.

1. Quite las cláusulas **`"orderBy"`** , si las hay en una solicitud existente. La puntuación semántica se usa para ordenar los resultados y, si incluye la lógica de ordenación explícita, se devuelve un error HTTP 400.

1. Opcionalmente, agregue el parámetro **`"answers"`** establecido en "extractive" y especifique el número de respuestas si quiere más de 1.

1. También tiene la opción de personalizar el estilo de resaltado que se aplica a los subtítulos. Los subtítulos aplican el formato de resaltado sobre los pasajes principales del documento que resumen la respuesta. El valor predeterminado es `<em>`. Si quiere especificar el tipo de formato (por ejemplo, fondo amarillo), puede establecer los parámetros highlightPreTag y highlightPostTag.

1. Especifique cualquier otro parámetro que quiera en la solicitud. Parámetros, como [speller](speller-how-to-add.md), [select](search-query-odata-select.md) y count mejoran la calidad de la solicitud y la legibilidad de la respuesta.

### <a name="review-the-response"></a>Revisar la respuesta

La respuesta de la consulta anterior devuelve la siguiente coincidencia como elección principal. Los subtítulos se devuelven automáticamente, con texto sin formato y versiones resaltadas. Para más información sobre las respuestas semánticas, consulte [Clasificación y respuestas semánticas](semantic-how-to-query-response.md).

```json
"@odata.count": 29,
"value": [
    {
        "@search.score": 1.8920634,
        "@search.rerankerScore": 1.1091284966096282,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Budget. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Budget. New Luxury Hotel. Be the first to stay.<strong> Bay views</strong> from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelId": "18",
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Budget"
    },
```

### <a name="parameters-used-in-a-semantic-query"></a>Parámetros usados en las consultas semánticas

En la tabla siguiente se resumen los parámetros de consulta que se usan en una consulta semántica para que pueda verlos holísticamente. Puede encontrar una lista de todos los parámetros en [Search Documents (versión preliminar de REST)](/rest/api/searchservice/preview-api/search-documents).

| Parámetro | Tipo | Descripción |
|-----------|-------|-------------|
| queryType | String | Los valores válidos son simple, full y semantic. Para las consultas semánticas, se requiere un valor de "semantic". |
| queryLanguage | String | Necesario para las consultas semánticas. Actualmente, solo está implementado "en-us". |
| searchFields | String | Una lista delimitada por comas de campos que permiten búsqueda. Opcional, pero recomendable. Especifica los campos en los que se produce la clasificación semántica. </br></br>A diferencia de los tipos de consulta simple y completa, el orden en que se muestran los campos determina la precedencia.|
| answers |String | Campo opcional para especificar que el resultado incluya las respuestas semánticas. Actualmente, solo está implementado "extractive". Las respuestas se pueden configurar para que se devuelvan cinco como máximo. En este ejemplo, "extractive|count3" muestra un recuento de tres respuestas. El valor predeterminado es 1.|

## <a name="query-with-search-explorer"></a>Consulta con el Explorador de búsqueda

La consulta siguiente se dirige al ejemplo integrado hotels-sample-index, con la versión de API 2020-06-30-Preview, y se ejecuta en el explorador de búsqueda. La cláusula `$select` limita los resultados a unos pocos campos, lo que facilita el examen del código JSON detallado en el explorador de búsqueda.

### <a name="with-querytypesemantic"></a>Con queryType=semantic

```json
search=I want a nice hotel on the water with a great restaurant&$select=HotelId,HotelName,Description,Tags&queryType=semantic&queryLanguage=english&searchFields=Description,Tags
```

Los primeros resultados son los siguientes.

```json
{
    "@search.score": 0.38330218,
    "@search.rerankerScore": 0.9754053303040564,
    "HotelId": "18",
    "HotelName": "Oceanside Resort",
    "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
    "Tags": [
        "view",
        "laundry service",
        "air conditioning"
    ]
},
{
    "@search.score": 1.8920634,
    "@search.rerankerScore": 0.8829904259182513,
    "HotelId": "36",
    "HotelName": "Pelham Hotel",
    "Description": "Stunning Downtown Hotel with indoor Pool. Ideally located close to theatres, museums and the convention center. Indoor Pool and Sauna and fitness centre. Popular Bar & Restaurant",
    "Tags": [
        "view",
        "pool",
        "24-hour front desk service"
    ]
},
{
    "@search.score": 0.95706713,
    "@search.rerankerScore": 0.8538530203513801,
    "HotelId": "22",
    "HotelName": "Stone Lion Inn",
    "Description": "Full breakfast buffet for 2 for only $1.  Excited to show off our room upgrades, faster high speed WiFi, updated corridors & meeting space. Come relax and enjoy your stay.",
    "Tags": [
        "laundry service",
        "air conditioning",
        "restaurant"
    ]
},
```

### <a name="with-querytype-default"></a>Con queryType (predeterminado)

Para compararlos, ejecute la misma consulta que antes, pero quite `&queryType=semantic&queryLanguage=english&searchFields=Description,Tags`. Observe que no hay ningún valor `"@search.rerankerScore"` en estos resultados y que aparecen distintos hoteles en los tres primeros puestos.

```json
{
    "@search.score": 8.633856,
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Tags": [
        "air conditioning",
        "bar",
        "continental breakfast"
    ]
},
{
    "@search.score": 6.407289,
    "HotelId": "40",
    "HotelName": "Trails End Motel",
    "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
    "Tags": [
        "continental breakfast",
        "view",
        "view"
    ]
},
{
    "@search.score": 5.843788,
    "HotelId": "14",
    "HotelName": "Twin Vertex Hotel",
    "Description": "New experience in the Making.  Be the first to experience the luxury of the Twin Vertex. Reserve one of our newly-renovated guest rooms today.",
    "Tags": [
        "bar",
        "restaurant",
        "air conditioning"
    ]
},
```

## <a name="next-steps"></a>Pasos siguientes

Recuerde que la clasificación y las respuestas semánticas se crean sobre un conjunto de resultados inicial. Cualquier lógica que mejore la calidad de los resultados iniciales hará avanzar la búsqueda semántica. El paso siguiente sería revisar las características que contribuyen a los resultados iniciales, como los analizadores que afectan al modo en que se acortan las cadenas, los perfiles de puntuación que pueden optimizar los resultados y el algoritmo de relevancia predeterminado.

+ [Analizadores para el procesamiento de texto](search-analyzers.md)
+ [Similitud y puntuación en Cognitive Search](index-similarity-and-scoring.md)
+ [Adición de perfiles de puntuación](index-add-scoring-profiles.md)
+ [Introducción a la búsqueda semántica](semantic-search-overview.md)
+ [Adición de la corrección ortográfica a los términos de consulta](speller-how-to-add.md)
