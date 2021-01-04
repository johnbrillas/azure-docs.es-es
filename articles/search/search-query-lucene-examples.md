---
title: Uso de la sintaxis de consulta completa de Lucene
titleSuffix: Azure Cognitive Search
description: Sintaxis de consulta de Lucene para la búsqueda aproximada, la búsqueda por proximidad, la priorización de términos, la búsqueda de expresiones regulares y la búsqueda con caracteres comodín en un servicio de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: Lucene query analyzer syntax
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: df26cfc3b220f40a7e73ff1c750d2b2ae37e7625
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401464"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>Uso de la sintaxis de búsqueda "completa" de Lucene (consultas avanzadas en Azure Cognitive Search)

Al construir consultas para Azure Cognitive Search, puede reemplazar el [analizador de consultas sencillo](query-simple-syntax.md) predeterminado por el [analizador de consultas de Lucene más eficaz de Azure Cognitive Search](query-lucene-syntax.md) para formular definiciones de consultas especializadas y avanzadas. 

El analizador de Lucene admite construcciones de consulta más complejas, como las consultas de ámbito de campo, la búsqueda aproximada y con caracteres comodín de infijos y sufijos, la búsqueda por proximidad, la priorización de términos y las expresiones regulares. La potencia adicional trae consigo requisitos de procesamiento adicional, por lo que debe esperar un tiempo de ejecución un poco más largo. En este artículo, puede pasar por los ejemplos de operaciones de consulta según la sintaxis completa.

> [!Note]
> Muchas de las construcciones de consulta especializadas habilitadas mediante la sintaxis de consulta completa de Lucene no son [de análisis de texto](search-lucene-query-architecture.md#stage-2-lexical-analysis), lo que puede ser sorprendente si espera lematización. Solo se realizan análisis léxicos en términos completos (consulta de término o de expresión). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta parciales es el establecimiento de minúsculas. 
>

## <a name="nyc-jobs-examples"></a>Ejemplos de trabajos de NYC

Los ejemplos siguientes aprovechan un [índice de búsqueda de trabajos de Nueva York](https://azjobsdemo.azurewebsites.net/) que consta de los disponibles según un conjunto de datos proporcionado por la [iniciativa City of New York OpenData](https://nycopendata.socrata.com/). Estos datos no deben considerarse actuales o completos. El índice está en un servicio de espacio aislado que proporciona Microsoft, lo que significa que no necesita una suscripción a Azure o a Azure Cognitive Search para probar estas consultas.

Lo que necesita es Postman o una herramienta equivalente para emitir la solicitud HTTP en GET o POST. Si no está familiarizado con estas herramientas, consulte [Inicio rápido: Exploración de las API REST de Cognitive Search](search-get-started-rest.md).

## <a name="set-up-the-request"></a>Establecimiento de la solicitud

1. Los encabezados de solicitud deben tener los siguientes valores:

   | Clave | Value |
   |-----|-------|
   | Content-Type | `application/json`|
   | api-key  | `252044BE3886FE4A8E3BAA4F595114BB` </br> (se trata de la clave de API de consulta real para el servicio de búsqueda de espacio aislado que hospeda el índice de trabajos de Nueva York) |

1. Establezca el verbo en **`GET`** .

1. Establezca la dirección URL en **`https://azs-playground.search.windows.net/indexes/nycjobs/docs/search=*&api-version=2020-06-30&queryType=full`** .

   + La colección de documentos en el índice incluye todo el contenido que permite búsquedas. Una clave de API de consulta proporcionada en el encabezado de solicitud solo funciona en las operaciones de lectura destinadas a la colección de documentos.

   + **`$count=true`** devuelve un recuento de los documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, el recuento serán todos los documentos del índice (unos 2558 en el caso de los trabajos de Nueva York).

   + **`search=*`** es una consulta sin especificar, equivalente a una búsqueda vacía o NULL. No es especialmente útil, pero es la búsqueda más sencilla que se puede hacer, y muestra todos los campos recuperables en el índice, con todos los valores.

   + **`queryType=full`** invoca el analizador de Lucene completo.

1. Como paso de comprobación, pegue la siguiente solicitud en GET y haga clic en **Enviar**. Los resultados se devuelven como documentos JSON detallados.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-full-lucene-parsing"></a>Invocación del análisis completo de Lucene

Agregue **`queryType=full`** para invocar la sintaxis de consulta completa, reemplazando la sintaxis de consulta simple predeterminada. Todos los ejemplos de este artículo especifican el parámetro de búsqueda **`queryType=full`** , que indica que toda la sintaxis se controla mediante el analizador de consultas de Lucene. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "queryType": "full"
}
```

## <a name="example-1-query-scoped-to-a-list-of-fields"></a>Ejemplo 1: Ámbito de una lista de campos para una consulta

El primer ejemplo no es específico del analizador, pero sirve para introducir el primer concepto fundamental de consulta: la independencia. En este ejemplo se limita tanto la de ejecución de consultas como la respuesta a unos pocos campos específicos. Es importante saber cómo estructurar una respuesta JSON legible cuando la herramienta es Postman o el Explorador de búsqueda. 

Esta consulta solo tiene como destino *business_title* en **`searchFields`** , especificando a través del parámetro **`select`** el mismo campo en la respuesta.

```http
POST https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  ![Respuesta de ejemplo de Postman con puntuaciones](media/search-query-lucene-examples/postman-sample-results.png)

Tal vez haya notado la puntuación de búsqueda en la respuesta. Las puntuaciones uniformes de **1** se producen cuando no hay ninguna clasificación, ya sea debido a que la búsqueda no era de texto completo o porque no se ha proporcionado ningún criterio. En el caso de una búsqueda vacía, las filas vuelven en un orden arbitrario. Al incluir criterios reales, verá que las puntuaciones de búsqueda evolucionan en valores significativos.

## <a name="example-2-fielded-search"></a>Ejemplo 2: Búsqueda clasificada por campos

La sintaxis completa de Lucene admite expresiones de búsqueda individuales para un campo específico. En este ejemplo se buscan puestos de empresa con el término "senior" en ellos, pero no "junior". Puede especificar varios campos mediante AND.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:(senior NOT junior) AND posting_type:external",
    "searchFields": "business_title, posting_type",
    "select": "business_title, posting_type"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla (posting_type no se muestra).

  :::image type="content" source="media/search-query-lucene-examples/intrafieldfilter.png" alt-text="Expresión de búsqueda de respuesta de ejemplo de Postman" border="false":::

La expresión de búsqueda puede ser una sola palabra, una frase o una expresión más compleja entre paréntesis, opcionalmente con operadores booleanos. A continuación se muestran algunos ejemplos:

+ `business_title:(senior NOT junior)`
+ `state:("New York" OR "New Jersey")`
+ `business_title:(senior NOT junior) AND posting_type:external`

Asegúrese de colocar varias cadenas entre comillas si desea que las dos cadenas se evalúen como una sola entidad, como en este caso buscando dos ciudades distintas en el campo `state`. Dependiendo de la herramienta, puede que tenga que escapar (`\`) las comillas. 

El campo especificado en **fieldName:searchExpression** debe ser un campo que permita búsquedas. Consulte [Creación de un índice (API REST de Azure Cognitive Search)](/rest/api/searchservice/create-index) para más información sobre cómo se usan los atributos de índice en las definiciones de campo.

> [!NOTE]
> En el ejemplo anterior, se omite el parámetro **`searchFields`** porque cada parte de la consulta tiene un nombre de campo especificado explícitamente. Sin embargo, puede seguir usando **`searchFields`** si la consulta tiene varias partes (mediante instrucciones AND). Por ejemplo, la consulta `search=business_title:(senior NOT junior) AND external&searchFields=posting_type` coincidiría con `senior NOT junior` únicamente con el campo `business_title`, aunque coincidiría con "external" con el campo `posting_type`. El nombre de campo proporcionado en `fieldName:searchExpression` siempre tiene prioridad sobre **`searchFields`** . Por eso en este ejemplo se puede omitir `business_title` de **`searchFields`** .

## <a name="example-3-fuzzy-search"></a>Ejemplo 3: Búsqueda aproximada

La sintaxis completa de Lucene también admite la búsqueda aproximada, que busca términos que tienen una construcción similar. Para realizar una búsqueda aproximada, agregue la virgulilla, `~`, al final de una sola palabra con un parámetro opcional y un valor entre 0 y 2 para especificar la distancia de edición. Por ejemplo, `blue~` o `blue~1` devolverían blue, blues y glue.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:asosiate~",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Las frases no se admiten directamente, pero puede especificar una coincidencia aproximada en cada término de una frase de varias partes, como `search=business_title:asosiate~ AND comm~`.  En la captura de pantalla siguiente, la respuesta incluye una coincidencia en *community associate*.

  :::image type="content" source="media/search-query-lucene-examples/fuzzysearch.png" alt-text="Respuesta de búsqueda aproximada" border="false":::

> [!Note]
> Las consultas aproximadas no se [analizan](search-lucene-query-architecture.md#stage-2-lexical-analysis). Los tipos de consulta con términos incompletos (consulta de prefijo, de carácter comodín, de expresión regular o aproximada) se agregan directamente en el árbol de la consulta, omitiéndose la fase de análisis. La única transformación que se realiza en los términos de consulta parciales es el establecimiento de minúsculas.
>

## <a name="example-4-proximity-search"></a>Ejemplo 4: Búsqueda por proximidad

Las búsquedas de proximidad se utilizan para buscar términos que están cerca entre sí en un documento. Inserte un símbolo "~" de la tilde de la Ñ al final de una frase seguido del número de palabras que crea el límite de proximidad. Por ejemplo, "hotel airport"~5 buscará los términos "hotel" y "airport" dentro de 5 palabras en un documento.

Esta consulta busca los términos "senior" y "analyst", donde cada término está separado por no más de una palabra, y las comillas se convierten en caracteres de escape (`\"`) para conservar la frase:

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~1",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla. 

  :::image type="content" source="media/search-query-lucene-examples/proximity-before.png" alt-text="Consulta de proximidad" border="false":::

Vuelva a intentarlo, eliminando cualquier distancia (`~0`) entre los términos "senior analyst". Tenga en cuenta que se devuelven ocho documentos para esta consulta en lugar de los diez de la consulta anterior.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~0",
    "searchFields": "business_title",
    "select": "business_title"
}
```

## <a name="example-5-term-boosting"></a>Ejemplo 5: Priorización de términos

"Priorización de términos" hace referencia a la valoración de un documento superior si contiene el término prioritario con respecto a los documentos que no contienen el término. Para impulsar un término, use el símbolo de intercalación, `^`, un símbolo con un factor de impulso (un número) al final del término que quiera buscar.

En esta consulta "anterior", se buscan los trabajos con el término *computer analyst*; observe que no se generan resultados con las palabras *computer* y *analyst*, aunque trabajos *computer* encabezan los resultados.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst",
    "searchFields": "business_title",
    "select": "business_title"
}
```

En la consulta "posterior", se repite la búsqueda y esta vez se da prioridad a los resultados con el término *analyst* sobre el término *computer*, si ninguna de las palabras existen. Una versión en lenguaje natural de la consulta es `search=business_title:computer analyst^2`. En el caso de una consulta factible en Postman, `^2` se codifica como `%5E2`.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst%5e2",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-lucene-examples/termboostingafter.png" alt-text="Priorización de términos posterior" border="false":::

La priorización de términos difiere de los perfiles de puntuación en que estos últimos priorizan ciertos campos, en lugar de términos específicos. En el siguiente ejemplo se muestran las diferencias.

Considere la posibilidad de usar un perfil de puntuación que dé prioridad a las coincidencias en un campo determinado, como **genre** en el ejemplo musicstoreindex. La priorización de términos podría utilizarse para dar mayor prioridad a determinados términos de búsqueda frente a otros. Por ejemplo, "rock^2 electronic" dará prioridad a los documentos que contengan los términos de búsqueda del campo **genre** frente a otros campos que permitan búsquedas en el índice. Además, los documentos que contienen el término de búsqueda "rock" tendrán una clasificación mayor que los del término de búsqueda "electronic" como resultado del valor de la priorización de términos (2).

Al establecer el nivel de factor, cuanto más alto sea el factor de prioridad, más apropiado será el término en relación con los demás términos de búsqueda. De forma predeterminada, el factor de prioridad es 1. Aunque el factor de prioridad debe ser positivo, puede ser inferior a 1 (por ejemplo, 0,2).

## <a name="example-6-regex"></a>Ejemplo 6: Regex

Una búsqueda de expresión regular encuentra una coincidencia en función del contenido entre barras diagonales "/", como se documentó en la [clase RegExp](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html).

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:/(Sen|Jun)ior/",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-lucene-examples/regex.png" alt-text="Consulta de Regex" border="false":::

> [!Note]
> Las consultas Regex no se [analizan](./search-lucene-query-architecture.md#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta parciales es el establecimiento de minúsculas.
>

## <a name="example-7-wildcard-search"></a>Ejemplo 7: Búsqueda con caracteres comodín

Puede usar la sintaxis generalmente reconocida para búsquedas con caracteres comodín únicas (?) o múltiples (\*). Tenga en cuenta que el Analizador de consultas de Lucene admite el uso de estos símbolos con un único término y no una frase.

En esta consulta, busque los trabajos que contengan el prefijo "prog" que incluiría los títulos de empresa con los términos "programación" y "programador". No puede usar `*` ni `?` como primer carácter de una búsqueda.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:prog*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-lucene-examples/wildcard.png" alt-text="Consulta de carácter comodín" border="false":::

> [!Note]
> Las consultas con caracteres comodín no se [analizan](./search-lucene-query-architecture.md#stage-2-lexical-analysis). La única transformación que se realiza en los términos de consulta parciales es el establecimiento de minúsculas.
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