---
title: Creación de consultas semánticas
titleSuffix: Azure Cognitive Search
description: Establezca un tipo de consulta semántica para asociar los modelos de aprendizaje profundo al procesamiento de consultas, y así inferir la intención y el contexto dentro de la clasificación y la relevancia de la búsqueda.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/05/2021
ms.openlocfilehash: 7f7a09b9e20b461a8a1e448bf4a7b0747a35fbb1
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487157"
---
# <a name="create-a-semantic-query-in-cognitive-search"></a>Creación de consultas semánticas en Cognitive Search

> [!IMPORTANT]
> El tipo de consulta semántica se encuentra en versión preliminar pública, y está disponible mediante la API REST en versión preliminar y Azure Portal. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Durante el lanzamiento de la versión preliminar inicial, no se aplica ningún cargo por la búsqueda semántica. Para más información, consulte [Disponibilidad y precios](semantic-search-overview.md#availability-and-pricing).

En este artículo, aprenderá a formular una solicitud de búsqueda que usa la clasificación semántica y generará subtítulos y respuestas semánticos.

Las consultas semánticas suelen funcionar mejor en los índices de búsqueda que se crean a partir de contenido de texto intensivo, como PDF o documentos con grandes fragmentos de texto.

## <a name="prerequisites"></a>Requisitos previos

+ Un servicio de búsqueda en un nivel Estándar (S1, S2, S3), ubicado en una de estas regiones: Centro-norte de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Este de EE. UU. 2, Europa del Norte y Oeste de Europa. Si tiene un servicio S1 o superior en una de estas regiones, puede solicitar acceso sin tener que crear un servicio.

+ Acceso a la versión preliminar de la búsqueda semántica: [registro](https://aka.ms/SemanticSearchPreviewSignup)

+ Un índice de búsqueda existente, con contenido en inglés

+ Un cliente de búsqueda para el envío de consultas

  El cliente de búsqueda debe admitir las API REST en versión preliminar en la solicitud de consulta. Puede usar [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) o código que haya modificado para realizar llamadas de REST a las API en versión preliminar. También puede usar el [Explorador de búsqueda](search-explorer.md) de Azure Portal para enviar consultas semánticas.

+ Una solicitud a [Search Documents](/rest/api/searchservice/preview-api/search-documents) con la opción semántica y otros parámetros descritos en este artículo.

## <a name="whats-a-semantic-query"></a>¿Qué es una consulta semántica?

En Cognitive Search, una consulta es una solicitud parametrizada que determina el procesamiento de las consultas y la forma de las respuestas. Una *consulta semántica* agrega parámetros que invocan el modelo de reclasificación semántica. Este algoritmo puede evaluar el contexto y el significado de los resultados coincidentes, promover los mejores a los primeros puestos, y devolver respuestas semánticas y subtítulos.

La solicitud siguiente es representativa de una consulta semántica básica (sin respuestas).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us"  
}
```

Como sucede con todas las consultas de Cognitive Search, la solicitud va dirigida a la colección de documentos de un índice único. Además, las consultas semánticas pasan por la misma secuencia de análisis y examen que una consulta no semántica. La diferencia radica en cómo se calcula la relevancia. Tal como se define en esta versión preliminar, una consulta semántica es aquella cuyos *resultados* se vuelven a procesar mediante algoritmos avanzados, lo que proporciona una manera de mostrar las coincidencias que el clasificador semántico considera de más importancia, en lugar de las puntuaciones que asigna el algoritmo predeterminado de clasificación de similitudes. 

Solo las 50 primeras coincidencias de los resultados iniciales se pueden clasificar semánticamente y todas incluyen subtítulos en la respuesta. Opcionalmente, puede especificar un parámetro **`answer`** en la solicitud para extraer una posible respuesta. Este modelo formula hasta cinco posibles respuestas a la consulta, que puede elegir representar en la parte superior de la página de búsqueda.

## <a name="query-using-rest-apis"></a>Consulta mediante las API REST

La especificación completa de la API REST se puede encontrar en [Search Documents (versión preliminar de REST)](/rest/api/searchservice/preview-api/search-documents).

Las consultas semánticas proporcionan subtítulos y resaltado automáticamente. Si quiere que en la contestación se incluyan las respuestas, puede agregar un parámetro **`answer`** opcional en la solicitud. Este parámetro, más la construcción de la propia cadena de consulta, producirá una respuesta en la respuesta.

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

En esta sección se describen los parámetros de consulta necesarios para la búsqueda semántica.

#### <a name="step-1-set-querytype-and-querylanguage"></a>Paso 1: Establecer queryType y queryLanguage

Agregue los siguientes parámetros al resto. Ambos parámetros son obligatorios.

```json
"queryType": "semantic",
"queryLanguage": "en-us",
```

El parámetro queryLanguage debe ser coherente con los [analizadores del lenguaje](index-add-language-analyzers.md) asignados a las definiciones de campo en el esquema de índice. Si queryLanguage es "en-us", todos los analizadores del lenguaje también deben tener una variante inglesa ("en.microsoft" o "en.lucene"). Los analizadores independientes del lenguaje, como Keyword o Simple, no tienen ningún conflicto con los valores de queryLanguage.

En una solicitud de consulta, si usa también la [corrección ortográfica](speller-how-to-add.md), el valor de queryLanguage que establezca se aplica por igual al corrector ortográfico, a las respuestas y a los subtítulos. No hay invalidaciones para partes individuales. 

Aunque el contenido de un índice de búsqueda puede estar redactado en varios idiomas, es más probable que la entrada de la consulta lo esté en uno. El motor de búsqueda no comprueba la compatibilidad de queryLanguage, el analizador del lenguaje ni el idioma en que está redactado el contenido, por lo que debe asegurarse de establecer el ámbito de las consultas en consecuencia para evitar que se generen resultados incorrectos.

<a name="searchfields"></a>

#### <a name="step-2-set-searchfields"></a>Paso 2: Establecer searchFields

Este parámetro es opcional, ya que no se produce ningún error si lo omite, pero se recomienda encarecidamente proporcionar una lista ordenada de campos para los subtítulos y las respuestas.

El parámetro searchFields se usa para identificar los pasajes que se van a evaluar para determinar su "similitud semántica" con la consulta. En el caso de la versión preliminar, no se recomienda dejar searchFields en blanco, ya que el modelo requiere una sugerencia sobre qué campos son más importantes para procesar.

El orden de searchFields es crítico. Si ya usa searchFields en consultas de Lucene simples o completas existentes, asegúrese de volver a visitar este parámetro al cambiar a un tipo de consulta semántica.

Siga estas instrucciones para garantizar resultados óptimos cuando se especifique dos o más parámetros searchFields:

+ Incluya solo campos de cadena y campos de cadena de nivel superior en las colecciones. Si se incluyen campos que no son de cadena o campos de nivel inferior en una colección, no se produce ningún error, pero esos campos no se usarán en la clasificación semántica.

+ El primer campo siempre debe ser conciso (por ejemplo, un título o un nombre). Lo ideal es que tenga menos de 25 palabras.

+ Si el índice tiene un campo de dirección URL que es textual (legible por el usuario, como `www.domain.com/name-of-the-document-and-other-details`, y no enfocado a la máquina, como `www.domain.com/?id=23463&param=eis`), colóquelo en segundo lugar en la lista (o en primer lugar si no hay ningún campo de título conciso).

+ Siga esos campos por los campos descriptivos en los que se pueda encontrar la respuesta a las consultas semánticas, como el contenido principal de un documento.

Si solo se especifica un campo, use los campos descriptivos en los que se pueda encontrar la respuesta a las consultas semánticas, como el contenido principal de un documento. Elija un campo que proporcione contenido suficiente.

#### <a name="step-3-remove-orderby-clauses"></a>Paso 3: Quitar las cláusulas orderBy

Quite todas las cláusulas orderBy, si las hay en una solicitud existente. La puntuación semántica se usa para ordenar los resultados y, si incluye la lógica de ordenación explícita, se devuelve un error HTTP 400.

#### <a name="step-4-add-answers"></a>Paso 4: Agregar respuestas

De manera opcional, agregue "respuestas" si quiere incluir un procesamiento adicional que proporcione una respuesta. Las respuestas (y los subtítulos) se formulan a partir de los pasajes que se encuentran en los campos enumerados en searchFields. Asegúrese de incluir campos con abundante contenido en searchFields para obtener los mejores subtítulos y respuestas en una respuesta.

Existen condiciones explícitas e implícitas que generan respuestas. 

+ Las condiciones explícitas incluyen la adición de "answers=extractive". Además, para especificar el número de respuestas devueltas en la respuesta global, agregue "count" seguido de un número: `"answers=extractive|count=3"`.  El valor predeterminado es uno. El máximo es cinco.

+ Las condiciones implícitas incluyen una construcción de cadena de consulta que se presta a una respuesta. Es más probable que se responda una consulta compuesta por "qué hotel tiene la habitación verde", que una consulta formada por una instrucción como "hotel con interior decorativo". Como cabría esperar, la consulta no puede dejarse sin especificar ni tener un valor NULL.

Lo más importante es que, si la consulta no parece una pregunta, se omite el procesamiento de respuestas, aunque el parámetro "answers" esté establecido.

#### <a name="step-5-add-other-parameters"></a>Paso 5: Agregar otros parámetros

Especifique cualquier otro parámetro que quiera incluir en la solicitud. Parámetros, como [speller](speller-how-to-add.md), [select](search-query-odata-select.md) y count mejoran la calidad de la solicitud y la legibilidad de la respuesta.

También tiene la opción de personalizar el estilo de resaltado que se aplica a los subtítulos. Los subtítulos aplican el formato de resaltado sobre los pasajes principales del documento que resumen la respuesta. El valor predeterminado es `<em>`. Si quiere especificar el tipo de formato (por ejemplo, fondo amarillo), puede establecer los parámetros highlightPreTag y highlightPostTag.

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
| answers |String | Campo opcional para especificar que el resultado incluya las respuestas semánticas. Actualmente, solo está implementado "extractive". Las respuestas se pueden configurar para que se devuelvan cinco como máximo. El valor predeterminado es uno. En este ejemplo se muestra un recuento de tres respuestas: "extractive\|count3". |

## <a name="query-with-search-explorer"></a>Consulta con el Explorador de búsqueda

La consulta siguiente se dirige al ejemplo integrado hotels-sample-index, con la versión de API 2020-06-30-Preview, y se ejecuta en el explorador de búsqueda. La cláusula `$select` limita los resultados a unos pocos campos, lo que facilita el examen del código JSON detallado en el explorador de búsqueda.

### <a name="with-querytypesemantic"></a>Con queryType=semantic

```json
search=nice hotel on water with a great restaurant&$select=HotelId,HotelName,Description,Tags&queryType=semantic&queryLanguage=english&searchFields=Description,Tags
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
