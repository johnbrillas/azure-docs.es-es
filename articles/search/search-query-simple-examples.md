---
title: Uso de la sintaxis de consulta simple Lucene
titleSuffix: Azure Cognitive Search
description: Ejemplos de consultas que muestran la sintaxis simple para la búsqueda de texto completo, la búsqueda filtrada y la búsqueda geográfica en un índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 2abe19351c92bf9cea85c85dd55f47b5ee6d1625
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101694043"
---
# <a name="use-the-simple-search-syntax-in-azure-cognitive-search"></a>Uso de la sintaxis de búsqueda "simple" en Azure Cognitive Search

En Azure Cognitive Search, la [sintaxis de consulta simple](query-simple-syntax.md) invoca el analizador de consultas predeterminado para realizar búsquedas de texto completo. Este analizador es rápido y gestiona escenarios comunes, incluida la búsqueda de texto completo, la búsqueda por facetas, la búsqueda filtrada y la búsqueda por prefijos. En este artículo se usan ejemplos para ilustrar el uso de la sintaxis simple en una solicitud de [Search Documents (API REST)](/rest/api/searchservice/search-documents).

> [!NOTE]
> Una alternativa es la sintaxis de consulta completa [Lucene](query-lucene-syntax.md), que admite estructuras de consulta más complejas, como búsqueda aproximada y con caracteres comodín. Para obtener más información y ejemplos, vea [Uso de la sintaxis completa Lucene](search-query-lucene-examples.md).

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
    "queryType": "simple",
    "select": "HotelId, HotelName, Category, Tags, Description",
    "count": true
}
```

+ "search" establecido en `*` es una consulta sin especificar que equivale a una búsqueda vacía o NULL. No es especialmente útil, pero es la búsqueda más sencilla que se puede hacer, y muestra todos los campos recuperables en el índice, con todos los valores.

+ "queryType" establecido en "simple" es el valor predeterminado y se puede omitir, pero se incluye para destacar aún más que los ejemplos de consulta de este artículo se expresan con la sintaxis simple.

+ "select" establecido en una lista de campos delimitada por comas se usa para la composición del resultado de la búsqueda incluyendo solo los campos que son útiles en el contexto de los resultados de la búsqueda.

+ "count" devuelve el número de documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, el recuento serán todos los documentos del índice (50 en el caso de hotels-sample-index).

## <a name="example-1-full-text-search"></a>Ejemplo 1: Búsqueda de texto completo

La búsqueda de texto completo sirve para cualquier número de términos independientes o frases entrecomilladas, con o sin operadores booleanos. 

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "pool spa +airport",
    "searchMode": any,
    "queryType": "simple",
    "select": "HotelId, HotelName, Category, Description",
    "count": true
}
```

Una búsqueda de palabras clave que se compone de términos o frases importantes suele funcionar mejor. Los campos de cadena se someten al análisis de texto durante la indexación y la consulta, y se quitan las palabras no esenciales, como "the", "and", "it", etc. Para ver cómo se descompone en tokens una cadena de consulta en el índice, pase la cadena al índice en una llamada a la API de [análisis de texto](/rest/api/searchservice/test-analyzer).

El parámetro "searchMode" controla la precisión y la coincidencia. Si quiere mayor coincidencia, use el valor predeterminado "any", que devuelve un resultado si se produce una coincidencia con cualquier parte de la cadena de consulta. Si su prioridad es la precisión y todas las partes de la cadena deben coincidir, cambie "searchMode" a "all". Pruebe la consulta anterior de las dos formas para ver cómo "searchMode" cambia el resultado.

La respuesta de la consulta "pool spa +airport" (piscina spa +aeropuerto) debe tener un aspecto similar al del ejemplo siguiente (acortado para simplificar):

```json
"@odata.count": 6,
"value": [
    {
        "@search.score": 7.3617697,
        "HotelId": "21",
        "HotelName": "Nova Hotel & Spa",
        "Description": "1 Mile from the airport.  Free WiFi, Outdoor Pool, Complimentary Airport Shuttle, 6 miles from the beach & 10 miles from downtown.",
        "Category": "Resort and Spa",
        "Tags": [
            "pool",
            "continental breakfast",
            "free parking"
        ]
    },
    {
        "@search.score": 2.5560288,
        "HotelId": "25",
        "HotelName": "Scottish Inn",
        "Description": "Newly Redesigned Rooms & airport shuttle.  Minutes from the airport, enjoy lakeside amenities, a resort-style pool & stylish new guestrooms with Internet TVs.",
        "Category": "Luxury",
        "Tags": [
            "24-hour front desk service",
            "continental breakfast",
            "free wifi"
        ]
    },
    {
        "@search.score": 2.2988036,
        "HotelId": "35",
        "HotelName": "Suites At Bellevue Square",
        "Description": "Luxury at the mall.  Located across the street from the Light Rail to downtown.  Free shuttle to the mall and airport.",
        "Category": "Resort and Spa",
        "Tags": [
            "continental breakfast",
            "air conditioning",
            "24-hour front desk service"
        ]
    }
```

Fíjese en la puntuación de búsqueda de la respuesta. Esta es la puntuación de relevancia de la coincidencia. De forma predeterminada, un servicio de búsqueda devolverá las 50 coincidencias principales en función de esta puntuación.

Las puntuaciones uniformes de tipo "1.0" se producen cuando no hay ninguna clasificación, ya sea debido a que la búsqueda no era de texto completo o porque no se ha proporcionado ningún criterio. Por ejemplo, en el caso de una búsqueda vacía (search=`*`), las filas se devuelven en un orden arbitrario. Al incluir criterios reales, verá que las puntuaciones de búsqueda evolucionan en valores significativos.

## <a name="example-2-look-up-by-id"></a>Ejemplo 2: Búsqueda por identificador

Cuando se devuelven los resultados de la búsqueda en una consulta, un paso siguiente lógico es proporcionar una página de detalles que incluya más campos del documento. En este ejemplo se muestra cómo devolver un solo documento mediante la API de [búsqueda de documento](/rest/api/searchservice/lookup-document) al pasar el identificador de dicho documento.

```http
GET /indexes/hotels-sample-index/docs/41?api-version=2020-06-30
```

Todos los documentos tienen un identificador único. Si usa Azure Portal, seleccione en índice en la pestaña **Índices** y, después, examine las definiciones de campo para determinar qué campo es la clave. Con REST, la llamada la API de [obtención de índice](/rest/api/searchservice/get-index) devuelve la definición del índice en el cuerpo de la respuesta.

La respuesta de la consulta anterior incluye el documento cuya clave es 41. Cualquier campo que esté marcado como recuperable en la definición del índice se puede devolver en los resultados de la búsqueda y se puede representar en la aplicación.

```json
{
    "HotelId": "41",
    "HotelName": "Ocean Air Motel",
    "Description": "Oceanfront hotel overlooking the beach features rooms with a private balcony and 2 indoor and outdoor pools. Various shops and art entertainment are on the boardwalk, just steps away.",
    "Description_fr": "L'hôtel front de mer surplombant la plage dispose de chambres avec balcon privé et 2 piscines intérieures et extérieures. Divers commerces et animations artistiques sont sur la promenade, à quelques pas.",
    "Category": "Budget",
    "Tags": [
        "pool",
        "air conditioning",
        "bar"
    ],
    "ParkingIncluded": true,
    "LastRenovationDate": "1951-05-10T00:00:00Z",
    "Rating": 3.5,
    "Location": {
        "type": "Point",
        "coordinates": [
            -157.846817,
            21.295841
        ],
        "crs": {
            "type": "name",
            "properties": {
                "name": "EPSG:4326"
            }
        }
    },
    "Address": {
        "StreetAddress": "1450 Ala Moana Blvd 2238 Ala Moana Ctr",
        "City": "Honolulu",
        "StateProvince": "HI",
        "PostalCode": "96814",
        "Country": "USA"
    },
```

## <a name="example-3-filter-on-text"></a>Ejemplo 3: Filtrado por texto

La [sintaxis de filtro](search-query-odata-filter.md) es una expresión de OData que se puede usar por sí sola o con "search". Si los usa conjuntamente, el filtro ("filter") se aplica primero a todo el índice y, después, se realiza la búsqueda en los resultados del filtro. Por lo tanto, los filtros pueden ser una técnica útil para mejorar el rendimiento porque reducen el conjunto de documentos en los que se debe procesar la consulta de búsqueda.

Los filtros se pueden definir en cualquier campo marcado como "filterable" (filtrable) en la definición del índice. En hotels-sample-index, los campos filtrables son Category, Tags, ParkingIncluded, Rating y la mayoría de los campos de dirección.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "art tours",
    "queryType": "simple",
    "filter": "Category eq 'Resort and Spa'",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

La respuesta de la consulta anterior solo se limita a los hoteles clasificados como "Resort and Spa", que incluyen los términos "art" (arte) o "tours" (visitas). En este caso, solo hay una coincidencia.

```json
{
    "@search.score": 2.8576312,
    "HotelId": "31",
    "HotelName": "Santa Fe Stay",
    "Description": "Nestled on six beautifully landscaped acres, located 2 blocks from the Plaza. Unwind at the spa and indulge in art tours on site.",
    "Category": "Resort and Spa"
}
```

## <a name="example-4-filter-functions"></a>Ejemplo 4: Funciones de filtro

Las expresiones de filtro pueden incluir [funciones "search.ismatch" y "search.ismatchscoring"](search-query-odata-full-text-search-functions.md), lo que le permite crear una consulta de búsqueda dentro del filtro. Esta expresión de filtro usa un carácter comodín en *free* (gratis) para determinados servicios, wifi gratis, parking gratuito, etc.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
  {
    "search": "",
    "filter": "search.ismatch('free*', 'Tags', 'full', 'any')",
    "select": "HotelId, HotelName, Category, Description",
    "count": true
  }
```

La respuesta de la consulta anterior devuelve coincidencias con 19 hoteles que ofrecen servicios gratis. Fíjese en que hay una puntuación de búsqueda uniforme de "1.0" en todos los resultados. Esto se debe a que la expresión de búsqueda es NULL o está vacía, lo que da lugar a coincidencias de filtro textuales, pero no se realiza ninguna búsqueda de texto completo. Las puntuaciones de relevancia solo se devuelven en la búsqueda de texto completo. Si usa filtros sin "search", asegúrese de que tiene suficientes campos ordenables para poder controlar el rango de búsqueda.

```json
"@odata.count": 19,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "31",
        "HotelName": "Santa Fe Stay",
        "Tags": [
            "view",
            "restaurant",
            "free parking"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "27",
        "HotelName": "Super Deluxe Inn & Suites",
        "Tags": [
            "bar",
            "free wifi"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "39",
        "HotelName": "Whitefish Lodge & Suites",
        "Tags": [
            "continental breakfast",
            "free parking",
            "free wifi"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "11",
        "HotelName": "Regal Orb Resort & Spa",
        "Tags": [
            "free wifi",
            "restaurant",
            "24-hour front desk service"
        ]
    },
```

## <a name="example-5-range-filters"></a>Ejemplo 5: Filtros de intervalo

Se admite el filtrado de intervalo mediante expresiones de filtro para cualquier tipo de datos. En los siguientes ejemplos se muestran intervalos numéricos y de cadena. Los tipos de datos son importantes en los filtros de rango y funcionan mejor cuando los campos numéricos contienen datos numéricos y los de cadena, datos de cadena. Los datos numéricos en los campos de cadena no son adecuados para los intervalos, ya que los números y las cadenas no se pueden comparar entre sí.

La consulta siguiente es un intervalo numérico: En hotels-sample-index, el único campo numérico filtrable es "Rating".

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating ge 2 and Rating lt 4",
    "select": "HotelId, HotelName, Rating",
    "orderby": "Rating desc",
    "count": true
}
```

La respuesta de esta consulta debe tener un aspecto similar al del siguiente ejemplo (acortado para simplificar):

```json
"@odata.count": 27,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "22",
        "HotelName": "Stone Lion Inn",
        "Rating": 3.9
    },
    {
        "@search.score": 1.0,
        "HotelId": "25",
        "HotelName": "Scottish Inn",
        "Rating": 3.8
    },
    {
        "@search.score": 1.0,
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Rating": 3.6
    }
```

La consulta siguiente es un filtro de intervalo en un campo de cadena (Address/StateProvince):

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Address/StateProvince ge 'A*' and Address/StateProvince lt 'D*'",
    "select": "HotelId, HotelName, Address/StateProvince",
    "count": true
}
```

La respuesta de esta consulta debe tener un aspecto similar al del ejemplo que se muestra a continuación (acortado para simplificar): En este ejemplo, no es posible ordenar por "StateProvince" porque el campo no tiene el atributo "sortable" (ordenable) en la definición del índice.

```json
"@odata.count": 9,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "9",
        "HotelName": "Smile Hotel",
        "Address": {
            "StateProvince": "CA "
        }
    },
    {
        "@search.score": 1.0,
        "HotelId": "39",
        "HotelName": "Whitefish Lodge & Suites",
        "Address": {
            "StateProvince": "CO"
        }
    },
    {
        "@search.score": 1.0,
        "HotelId": "7",
        "HotelName": "Countryside Resort",
        "Address": {
            "StateProvince": "CA "
        }
    },
```

## <a name="example-6-geo-search"></a>Ejemplo 6: Búsqueda geográfica

El índice hotels-sample incluye un campo "geo_location" con las coordenadas de latitud y longitud. En este ejemplo se usa la [función geo.distance](search-query-odata-geo-spatial-functions.md#examples), que filtra en documentos dentro de la circunferencia de un punto de partida, hasta la distancia arbitraria (en kilómetros) que proporcione. Puede ajustar el último valor en la consulta (10) para reducir o ampliar el área expuesta de la consulta.

```http
POST /indexes/v/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "geo.distance(Location, geography'POINT(-122.335114 47.612839)') le 10",
    "select": "HotelId, HotelName, Address/City, Address/StateProvince",
    "count": true
}
```

La respuesta para esta consulta devuelve todos los hoteles en una distancia de 10 kilómetros de las coordenadas proporcionadas:

```json
{
    "@odata.count": 3,
    "value": [
        {
            "@search.score": 1.0,
            "HotelId": "45",
            "HotelName": "Arcadia Resort & Restaurant",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        },
        {
            "@search.score": 1.0,
            "HotelId": "24",
            "HotelName": "Gacc Capital",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        },
        {
            "@search.score": 1.0,
            "HotelId": "16",
            "HotelName": "Double Sanctuary Resort",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        }
    ]
}
```

## <a name="example-7-booleans-with-searchmode"></a>Ejemplo 7: Valores booleanos con searchMode

La sintaxis simple admite operadores booleanos en forma de caracteres (`+, -, |`) para admitir los operadores lógicos AND, OR y NOT. La búsqueda booleana se comporta del modo esperado, con algunas excepciones destacadas. 

En los ejemplos anteriores, el parámetro "searchMode" se presenta como un mecanismo para influir en la precisión y la coincidencia, con "searchMode=any" que favorece la coincidencia (un documento que cumple alguno de los criterios se considera una coincidencia) y "searchMode=all" que favorece la precisión (todos los criterios deben coincidir en un documento). 

En el contexto de una búsqueda booleana, el valor predeterminado es "searchMode=any", lo que puede llevar a confusión si se apila una consulta con varios operadores y se obtienen resultados más amplios, en lugar de más restringidos. Esto sucede especialmente con el operador NOT, donde los resultados incluyen todos los documentos que no contienen un término o una frase específicos.

Esto se muestra en el ejemplo siguiente. Al ejecutar la siguiente consulta con searchMode (any), se devuelven 42 documentos: los que contienen el término "restaurant" (restaurante) y todos los que no tienen la frase "air conditioning" (aire acondicionado). 

Observe que no hay espacio entre el operador booleano (`-`) y "air conditioning".

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "restaurant -\"air conditioning\"",
    "searchMode": "any",
    "searchFields": "Tags",
    "select": "HotelId, HotelName, Tags",
    "count": true
}
```

Si se cambia a "searchMode=all", se aplica un efecto acumulativo en los criterios y se devuelve un conjunto de resultados más pequeño (7 coincidencias) que consta de los documentos que contienen el término "restaurant", menos aquellos que contienen la frase "air conditioning".

Ahora la respuesta de esta consulta tendría un aspecto similar al del ejemplo siguiente (acortado para simplificar):

```json
"@odata.count": 7,
"value": [
    {
        "@search.score": 2.5460577,
        "HotelId": "11",
        "HotelName": "Regal Orb Resort & Spa",
        "Tags": [
            "free wifi",
            "restaurant",
            "24-hour front desk service"
        ]
    },
    {
        "@search.score": 2.166792,
        "HotelId": "10",
        "HotelName": "Countryside Hotel",
        "Tags": [
            "24-hour front desk service",
            "coffee in lobby",
            "restaurant"
        ]
    },
```

## <a name="example-8-paging-results"></a>Ejemplo 8: Paginación de resultados

En los ejemplos anteriores, ha aprendido sobre los parámetros que afectan a la composición de los resultados de la búsqueda, incluido "select", que determina qué campos se encuentran en un resultado, los criterios de ordenación y cómo incluir un recuento de todas las coincidencias. Este ejemplo es una continuación de la composición del resultado de la búsqueda en forma de parámetros de paginación que permiten procesar por lotes el número de resultados que aparecen en una página determinada. 

De forma predeterminada, un servicio de búsqueda devuelve las 50 coincidencias principales. Para controlar el número de coincidencias en cada página, use "top" para definir el tamaño del lote y, después, use "skip" para seleccionar lotes posteriores.

En el ejemplo siguiente se usa un filtro y un criterio de ordenación en el campo de clasificación (Rating, que es filtrable y ordenable) porque es más fácil ver los efectos de la paginación en resultados ordenados. En una consulta de búsqueda completa normal, las coincidencias principales se clasifican y se paginan mediante "@search.score".

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating gt 4",
    "select": "HotelName, Rating",
    "orderby": "Rating desc",
    "top": "5",
    "count": true
}
```

La consulta encuentra 21 documentos coincidentes, pero como se ha especificado "top", la respuesta devuelve solo las cinco coincidencias principales, con clasificaciones a partir de 4,9 y hasta 4,7 con "Lady of the Lake B & B". 

Para obtener los cinco siguientes, omita el primer lote:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating gt 4",
    "select": "HotelName, Rating",
    "orderby": "Rating desc",
    "top": "5",
    "skip": "5",
    "count": true
}
```

La respuesta del segundo lote omite las cinco primeras coincidencias y devuelve las cinco siguientes, empezando por "Pull'r Inn Motel". Para continuar con otros lotes, debe mantener "top" en 5 y, después, incrementar "skip" en 5 en cada solicitud nueva (skip=5, skip=10, skip=15, etc.).

```json
"value": [
    {
        "@search.score": 1.0,
        "HotelName": "Pull'r Inn Motel",
        "Rating": 4.7
    },
    {
        "@search.score": 1.0,
        "HotelName": "Sublime Cliff Hotel",
        "Rating": 4.6
    },
    {
        "@search.score": 1.0,
        "HotelName": "Antiquity Hotel",
        "Rating": 4.5
    },
    {
        "@search.score": 1.0,
        "HotelName": "Nordick's Motel",
        "Rating": 4.5
    },
    {
        "@search.score": 1.0,
        "HotelName": "Winter Panorama Resort",
        "Rating": 4.5
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene más práctica con la sintaxis de consulta básica, pruebe a especificar consultas en el código. En los vínculos siguientes se explica cómo configurar las consultas de búsqueda para los SDK de Azure.

+ [Consultas del índice con el SDK para .NET](search-get-started-dotnet.md)
+ [Consultas del índice con el SDK para Python](search-get-started-python.md)
+ [Consultas del índice con el SDK para JavaScript](search-get-started-javascript.md)

En los vínculos siguientes, se puede encontrar una referencia de la sintaxis, la arquitectura de las consultas y ejemplos:

+ [Ejemplos de consulta con sintaxis de Lucene para la creación de consultas avanzadas](search-query-lucene-examples.md)
+ [Funcionamiento de la búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Sintaxis de consulta simplificada](query-simple-syntax.md)
+ [Sintaxis de consulta completa de Lucene ](query-lucene-syntax.md)
+ [Sintaxis de filtro](search-query-odata-filter.md)