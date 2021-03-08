---
title: Uso de la sintaxis de consulta completa de Lucene
titleSuffix: Azure Cognitive Search
description: Ejemplos de consultas que muestran la sintaxis de consulta de Lucene para la búsqueda aproximada, la búsqueda por proximidad, la priorización de términos, la búsqueda de expresiones regulares y la búsqueda con caracteres comodín en un índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 6213efb6ba14052c6f957a6d999f48f55f65186c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693567"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>Uso de la sintaxis de búsqueda "completa" de Lucene (consultas avanzadas en Azure Cognitive Search)

Al construir consultas para Azure Cognitive Search, puede reemplazar el [analizador de consultas sencillo](query-simple-syntax.md) predeterminado por el [analizador de consultas de Lucene](query-lucene-syntax.md) más eficaz para formular expresiones de consulta especializadas y avanzadas.

El analizador de Lucene admite formatos de consulta más complejos, como las consultas de ámbito de campo, la búsqueda aproximada y con caracteres comodín de infijos y sufijos, la búsqueda por proximidad, la priorización de términos y las expresiones regulares. La potencia adicional trae consigo requisitos de procesamiento adicional, por lo que debe esperar un tiempo de ejecución un poco más largo. En este artículo, puede pasar por los ejemplos de operaciones de consulta según la sintaxis completa.

> [!Note]
> Muchas de las construcciones de consulta especializadas habilitadas mediante la sintaxis de consulta completa de Lucene no son [de análisis de texto](search-lucene-query-architecture.md#stage-2-lexical-analysis), lo que puede ser sorprendente si espera lematización. Solo se realizan análisis léxicos en términos completos (consulta de término o de expresión). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta parciales es el establecimiento de minúsculas. 
>

## <a name="hotels-sample-index"></a>Índice hotels-sample

Las siguientes consultas se basan en el índice de búsqueda hoteles-sample-index, que puede crear siguiendo las instrucciones de este [inicio rápido](search-get-started-portal.md).

Las consultas de ejemplo se articulan mediante la API REST y las solicitudes POST. Puede pegarlas y ejecutarlas en [Postman](search-get-started-rest.md) o en [Visual Studio Code con la extensión Azure Cognitive Search](search-get-started-vs-code.md).

Los encabezados de solicitud deben tener los siguientes valores:

| Clave | Value |
|-----|-------|
| Content-Type | application/json|
| api-key  | `<your-search-service-api-key>`, ya sea una consulta o una clave de administración |

Los parámetros de URI deben incluir el punto de conexión del servicio de búsqueda con el nombre del índice, las colecciones de documentos, el comando de búsqueda y la versión de la API de manera similar al ejemplo siguiente:

```http
https://{{service-name}}.search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30
```

El cuerpo de la solicitud debe tener un formato JSON válido:

```json
{
    "search": "*",
    "queryType": "full",
    "select": "HotelId, HotelName, Category, Tags, Description",
    "count": true
}
```

+ "search" establecido en `*` es una consulta sin especificar que equivale a una búsqueda vacía o NULL. No es especialmente útil, pero es la búsqueda más sencilla que se puede hacer, y muestra todos los campos recuperables en el índice, con todos los valores.

+ "queryType" establecido en "full" invoca el analizador de consultas completas de Lucene y es necesario para esta sintaxis.

+ "select" establecido en una lista de campos delimitada por comas se usa para la composición del resultado de la búsqueda incluyendo solo los campos que son útiles en el contexto de los resultados de la búsqueda.

+ "count" devuelve el número de documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, el recuento serán todos los documentos del índice (50 en el caso de hotels-sample-index).

## <a name="example-1-fielded-search"></a>Ejemplo 1: Búsqueda clasificada por campos

La búsqueda clasificada por campos establece el ámbito de las expresiones de búsqueda insertadas individuales en un campo específico. En este ejemplo se buscan nombres de hotel con el término "hotel" en ellos, pero no "motel". Puede especificar varios campos mediante AND. 

A la hora de usar esta sintaxis de consulta, puede omitir el parámetro "searchFields" si los campos que quiere consultar se encuentren en la propia expresión de búsqueda. Si incluye "searchFields" con la búsqueda clasificada por campos, `fieldName:searchExpression` siempre tiene prioridad sobre "searchFields".

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:(hotel NOT motel) AND Category:'Resort and Spa'",
    "queryType": "full",
    "select": "HotelName, Category",
    "count": true
}
```

La respuesta a esta consulta debe estar filtrada por "Resort and Spa" (Complejo turístico y spa) y devolver nombres de hoteles que incluyen "hotel" o "motel", y debe tener un aspecto similar al del ejemplo siguiente:

```json
"@odata.count": 4,
"value": [
    {
        "@search.score": 4.481559,
        "HotelName": "Nova Hotel & Spa",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.4524608,
        "HotelName": "King's Palace Hotel",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.3970203,
        "HotelName": "Triple Landscape Hotel",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.2953436,
        "HotelName": "Peaceful Market Hotel & Spa",
        "Category": "Resort and Spa"
    }
]
```

La expresión de búsqueda puede ser un solo término, una frase o una expresión más compleja entre paréntesis, opcionalmente con operadores booleanos. A continuación se muestran algunos ejemplos:

+ `HotelName:(hotel NOT motel)`
+ `Address/StateProvince:("WA" OR "CA")`
+ `Tags:("free wifi" NOT "free parking") AND "coffee in lobby"`

Asegúrese de colocar la frase entre comillas si quiere que las dos cadenas se evalúen como una sola entidad, como en este caso para buscar dos ciudades distintas en el campo Address/StateProvince. Dependiendo del cliente, puede que tenga que convertir las comillas en caracteres de escape (`\`).

El campo especificado en `fieldName:searchExpression` debe ser un campo que permita búsquedas. Consulte el artículo [Creación de un índice (API REST)](/rest/api/searchservice/create-index) para obtener más información sobre cómo usar atributos en las definiciones de campo.

## <a name="example-2-fuzzy-search"></a>Ejemplo 2: Búsqueda aproximada

La búsqueda aproximada busca coincidencias con términos similares, incluidas palabras mal escritas. Para realizar una búsqueda aproximada, agregue la virgulilla, `~`, al final de una sola palabra con un parámetro opcional y un valor entre 0 y 2 para especificar la distancia de edición. Por ejemplo, `blue~` o `blue~1` devolverían blue, blues y glue.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "Tags:conserge~",
    "queryType": "full",
    "select": "HotelName, Category, Tags",
    "searchFields": "HotelName, Category, Tags",
    "count": true
}
```

La respuesta de esta consulta (acortada para simplificar) devuelve "concierge" (conserje) en los documentos coincidentes:

```json
"@odata.count": 12,
"value": [
    {
        "@search.score": 1.1832147,
        "HotelName": "Secret Point Motel",
        "Category": "Boutique",
        "Tags": [
            "pool",
            "air conditioning",
            "concierge"
        ]
    },
    {
        "@search.score": 1.1819803,
        "HotelName": "Twin Dome Motel",
        "Category": "Boutique",
        "Tags": [
            "pool",
            "free wifi",
            "concierge"
        ]
    },
    {
        "@search.score": 1.1773309,
        "HotelName": "Smile Hotel",
        "Category": "Suite",
        "Tags": [
            "view",
            "concierge",
            "laundry service"
        ]
    },
```

Las frases no se admiten directamente, pero puede especificar una coincidencia aproximada en cada término de una frase de varias partes, como `search=Tags:landy~ AND sevic~`.  Esta expresión de consulta encuentra 15 coincidencias para "laundry service" (servicio de lavandería).

> [!Note]
> Las consultas aproximadas no se [analizan](search-lucene-query-architecture.md#stage-2-lexical-analysis). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta parciales es la conversión a minúsculas.
>

## <a name="example-3-proximity-search"></a>Ejemplo 3: Búsqueda por proximidad

La búsqueda por proximidad busca términos que están cerca uno del otro en un documento. Inserte un símbolo "~" de la tilde de la Ñ al final de una frase seguido del número de palabras que crea el límite de proximidad.

Esta consulta busca los términos "hotel" y "airport" (aeropuerto) a menos de 5 palabras de distancia unos de otros en un documento. Las comillas se convierten en caracteres de escape (`\"`) para conservar la frase:

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "Description: \"hotel airport\"~5",
    "queryType": "full",
    "select": "HotelName, Description",
    "searchFields": "HotelName, Description",
    "count": true
}
```

La respuesta de esta consulta debe tener un aspecto similar al del siguiente ejemplo:

```json
"@odata.count": 2,
"value": [
    {
        "@search.score": 0.6331726,
        "HotelName": "Trails End Motel",
        "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
    },
    {
        "@search.score": 0.43032226,
        "HotelName": "Catfish Creek Fishing Cabins",
        "Description": "Brand new mattresses and pillows.  Free airport shuttle. Great hotel for your business needs. Comp WIFI, atrium lounge & restaurant, 1 mile from light rail."
    }
]
```

## <a name="example-4-term-boosting"></a>Ejemplo 4: Priorización de términos

"Priorización de términos" hace referencia a la valoración de un documento superior si contiene el término prioritario con respecto a los documentos que no contienen el término. Para impulsar un término, use el símbolo de intercalación, `^`, un símbolo con un factor de impulso (un número) al final del término que quiera buscar. De forma predeterminada, el factor de prioridad es 1 y, aunque debe ser positivo, puede ser menor que 1 (por ejemplo, 0,2). La priorización de términos difiere de los perfiles de puntuación en que estos últimos priorizan ciertos campos, en lugar de términos específicos.

En esta consulta "anterior", busque "beach access" (acceso a la playa). Verá que hay siete documentos con coincidencias de uno o ambos términos.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "beach access",
    "queryType": "full",
    "select": "HotelName, Description, Tags",
    "searchFields": "HotelName, Description, Tags",
    "count": true
}
```

De hecho, solo hay un documento en el que aparece "access" (acceso) y, dado que es la única coincidencia, la posición es alta (segunda posición) aunque el documento no contenga el término "beach" (playa).

```json
"@odata.count": 7,
"value": [
    {
        "@search.score": 2.2723424,
        "HotelName": "Nova Hotel & Spa",
        "Description": "1 Mile from the airport.  Free WiFi, Outdoor Pool, Complimentary Airport Shuttle, 6 miles from the beach & 10 miles from downtown."
    },
    {
        "@search.score": 1.5507699,
        "HotelName": "Old Carrabelle Hotel",
        "Description": "Spacious rooms, glamorous suites and residences, rooftop pool, walking access to shopping, dining, entertainment and the city center."
    },
    {
        "@search.score": 1.5358944,
        "HotelName": "Whitefish Lodge & Suites",
        "Description": "Located on in the heart of the forest. Enjoy Warm Weather, Beach Club Services, Natural Hot Springs, Airport Shuttle."
    },
    {
        "@search.score": 1.3433652,
        "HotelName": "Ocean Air Motel",
        "Description": "Oceanfront hotel overlooking the beach features rooms with a private balcony and 2 indoor and outdoor pools. Various shops and art entertainment are on the boardwalk, just steps away."
    },
```

En la consulta "posterior", se repite la búsqueda, y esta vez se da prioridad a los resultados con el término "beach" sobre el término "access". Una versión en lenguaje natural de la consulta es `search=Description:beach^2 access`. Dependiendo de su cliente, es posible que tenga que expresar `^2` como `%5E2`.

Después de dar prioridad al término "beach", la coincidencia de "Old Carrabelle Hotel" se desplaza a la sexta posición.

<!-- Consider a scoring profile that boosts matches in a certain field, such as "genre" in a music app. Term boosting could be used to further boost certain search terms higher than others. For example, "rock^2 electronic" will boost documents that contain the search terms in the "genre" field higher than other searchable fields in the index. Furthermore, documents that contain the search term "rock" will be ranked higher than the other search term "electronic" as a result of the term boost value (2). -->

## <a name="example-5-regex"></a>Ejemplo 5: Regex

Una búsqueda de expresión regular encuentra una coincidencia en función del contenido entre barras diagonales "/", como se documentó en la [clase RegExp](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html).

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:/(Mo|Ho)tel/",
    "queryType": "full",
    "select": "HotelName",
    "count": true
}
```

La respuesta de esta consulta debe tener un aspecto similar al del siguiente ejemplo:

```json
    "@odata.count": 22,
    "value": [
        {
            "@search.score": 1.0,
            "HotelName": "Days Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Triple Landscape Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Smile Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Pelham Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Sublime Cliff Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Twin Dome Motel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Nova Hotel & Spa"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Scarlet Harbor Hotel"
        },
```

> [!Note]
> Las consultas Regex no se [analizan](./search-lucene-query-architecture.md#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta parciales es la conversión a minúsculas.
>

## <a name="example-6-wildcard-search"></a>Ejemplo 6: Búsqueda con caracteres comodín

Puede usar la sintaxis generalmente reconocida para búsquedas con caracteres comodín únicas (`*`) o múltiples (`?`). Tenga en cuenta que el Analizador de consultas de Lucene admite el uso de estos símbolos con un único término y no una frase.

En esta consulta, busque los nombres de hotel que empiezan por "sc". No puede usar `*` ni `?` como primer carácter de una búsqueda.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:sc*",
    "queryType": "full",
    "select": "HotelName",
    "count": true
}
```

La respuesta de esta consulta debe tener un aspecto similar al del siguiente ejemplo:

```json
    "@odata.count": 2,
    "value": [
        {
            "@search.score": 1.0,
            "HotelName": "Scarlet Harbor Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Scottish Inn"
        }
    ]
```

> [!Note]
> Las consultas con caracteres comodín no se [analizan](./search-lucene-query-architecture.md#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta parciales es la conversión a minúsculas.
>

## <a name="next-steps"></a>Pasos siguientes

Pruebe a especificar las consultas en el código. En los vínculos siguientes se explica cómo configurar las consultas de búsqueda para los SDK de Azure.

+ [Consultas del índice con el SDK para .NET](search-get-started-dotnet.md)
+ [Consultas del índice con el SDK para Python](search-get-started-python.md)
+ [Consultas del índice con el SDK para JavaScript](search-get-started-javascript.md)

En los vínculos siguientes, se puede encontrar una referencia de la sintaxis, la arquitectura de las consultas y ejemplos:

+ [Ejemplos de consulta con sintaxis de Lucene para la creación de consultas avanzadas](search-query-lucene-examples.md)
+ [Funcionamiento de la búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Sintaxis de consulta simplificada](query-simple-syntax.md)
+ [Sintaxis de consulta completa de Lucene ](query-lucene-syntax.md)
+ [Sintaxis de filtro](search-query-odata-filter.md)