---
title: Uso de la sintaxis de consulta simple Lucene
titleSuffix: Azure Cognitive Search
description: Aprenda por ejemplo mediante la ejecución de consultas basadas en la sintaxis simple para la búsqueda de texto completo, la búsqueda filtrada, la búsqueda geográfica o la búsqueda por facetas en un índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/12/2020
ms.openlocfilehash: ff9495e37a499b5502d8f8ced79b69608fa9552a
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401753"
---
# <a name="use-the-simple-search-syntax-in-azure-cognitive-search"></a>Uso de la sintaxis de búsqueda "simple" en Azure Cognitive Search

En Azure Cognitive Search, la [sintaxis de consulta simple](query-simple-syntax.md) invoca el analizador de consultas predeterminado para realizar búsquedas de texto completo. Este analizador es rápido y gestiona escenarios comunes, incluida la búsqueda de texto completo, la búsqueda por facetas, la búsqueda filtrada y la búsqueda por prefijos. En este artículo se usan ejemplos para ilustrar el uso de la sintaxis simple en una solicitud de [Search Documents (API REST)](/rest/api/searchservice/search-documents).

> [!NOTE]
> Una alternativa es la sintaxis de consulta completa [Lucene](query-lucene-syntax.md), que admite estructuras de consulta más complejas, como búsqueda aproximada y con caracteres comodín. Para obtener más información y ejemplos, vea [Uso de la sintaxis completa Lucene](search-query-lucene-examples.md).

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

1. Establezca la dirección URL en **`https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&search=*&$count=true`** . 

   + La colección de documentos en el índice incluye todo el contenido que permite búsquedas. Una clave de API de consulta proporcionada en el encabezado de solicitud solo funciona en las operaciones de lectura destinadas a la colección de documentos.

   + **`$count=true`** devuelve un recuento de los documentos que coinciden con los criterios de búsqueda. En una cadena de búsqueda vacía, el recuento serán todos los documentos del índice (unos 2558 en el caso de los trabajos de Nueva York).

   + **`search=*`** es una consulta sin especificar, equivalente a una búsqueda vacía o NULL. No es especialmente útil, pero es la búsqueda más sencilla que se puede hacer, y muestra todos los campos recuperables en el índice, con todos los valores.

1. Como paso de comprobación, pegue la siguiente solicitud en GET y haga clic en **Enviar**. Los resultados se devuelven como documentos JSON detallados.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-simple-query-parsing"></a>Cómo invocar el análisis de consulta simplificada

Para las consultas interactivas, no tiene que especificar nada: la consulta simplificada es la predeterminada. En el código, si previamente invocó **`queryType=full`** , puede restablecer el valor predeterminado con **`queryType=simple`** .

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "queryType": "simple"
}
```

## <a name="example-1-full-text-search-on-specific-fields"></a>Ejemplo 1: Búsqueda de texto completo en campos específicos

El primer ejemplo no es específico del analizador, pero sirve para introducir el primer concepto fundamental de consulta: la independencia. En este ejemplo se limita tanto la de ejecución de consultas como la respuesta a unos pocos campos específicos. Es importante saber cómo estructurar una respuesta JSON legible cuando la herramienta es Postman o el Explorador de búsqueda. 

Esta consulta solo tiene como destino *business_title* en **`searchFields`** , especificando a través del parámetro **`select`** el mismo campo en la respuesta.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-lucene-examples/postman-sample-results.png" alt-text="Respuesta de ejemplo de Postman" border="false":::

Tal vez haya notado la puntuación de búsqueda en la respuesta. Las puntuaciones uniformes de **1** se producen cuando no hay ninguna clasificación, ya sea debido a que la búsqueda no era de texto completo o porque no se ha proporcionado ningún criterio. En el caso de una búsqueda vacía, las filas vuelven en un orden arbitrario. Al incluir criterios reales, verá que las puntuaciones de búsqueda evolucionan en valores significativos.

## <a name="example-2-look-up-by-id"></a>Ejemplo 2: Búsqueda por identificador

Cuando se devuelven resultados de la búsqueda en una consulta, un paso siguiente lógico es proporcionar una página de detalles que incluya más campos del documento. En este ejemplo se muestra cómo devolver un solo documento mediante una [operación de búsqueda](/rest/api/searchservice/lookup-document) para pasar el identificador de documento.

Todos los documentos tienen un identificador único. Para probar la sintaxis de una consulta de búsqueda, en primer lugar devuelva una lista de identificadores de documento para elegir el que desea utilizar. Para NYC Jobs, los identificadores están almacenados en el campo `id`.

```http
GET /indexes/nycjobs/docs?api-version=2020-06-30&search=*&$select=id&$count=true
```

A continuación, recupere un documento de la colección basado en `id` "9E1E3AF9-0660-4E00-AF51-9B654925A2D5", que aparecía en primer lugar en la respuesta anterior. La siguiente consulta devuelve todos los campos recuperables para todo el documento.

```http
GET /indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2020-06-30
```

## <a name="example-3-filter-queries"></a>Ejemplo 3: Consultas de filtro

La [sintaxis de filtro](./search-query-odata-filter.md) es una expresión de OData que se puede usar con **`search`** . Un filtro independiente, sin parámetros de consulta, resulta útil cuando la expresión de filtro puede definir completamente los documentos de interés. Sin una cadena de consulta, no hay ningún análisis lingüístico ni léxico, ni puntuación (todas son 1), ni clasificación. Tenga en cuenta que la cadena de búsqueda está vacía.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

Si los usa conjuntamente, el filtro se aplica primero a todo el índice y, después, se realiza la búsqueda en los resultados del filtro. Por lo tanto, los filtros pueden ser una técnica útil para mejorar el rendimiento porque reducen el conjunto de documentos en los que se debe procesar la consulta de búsqueda.

  :::image type="content" source="media/search-query-simple-examples/filtered-query.png" alt-text="Respuesta de la consulta de filtro" border="false":::

Otra manera eficaz de combinar el filtrado y la búsqueda es mediante **`search.ismatch*()`** en una expresión de filtro, donde puede usar una consulta de búsqueda dentro del filtro. Esta expresión de filtro utiliza un carácter comodín en *plan* seleccionar business_title, incluido el periodo, el programador, el planeamiento, etc.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "search.ismatch('plan*', 'business_title', 'full', 'any')",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

Para más información acerca de la función, consulte [search.ismatch en los ejemplos de filtro](./search-query-odata-full-text-search-functions.md#examples).

## <a name="example-4-range-filters"></a>Ejemplo 4: Filtros de rango

Se admite el filtrado de rango mediante expresiones **`$filter`** para cualquier tipo de datos. En los siguientes ejemplos se busca en campos numéricos y de cadena. 

Los tipos de datos son importantes en los filtros de rango y funcionan mejor cuando los campos numéricos contienen datos numéricos y los de cadena, datos de cadena. Los datos numéricos en los campos de cadena no son adecuados para los rangos, ya que los números y las cadenas no se pueden comparar en Azure Cognitive Search.

La consulta siguiente es un intervalo numérico:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency"
    }
```
La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-simple-examples/rangefilternumeric.png" alt-text="Filtro de rango para rangos numéricos" border="false":::

En esta consulta, el intervalo está por encima de un campo de cadena (business_title):

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title"
    }
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-simple-examples/rangefiltertext.png" alt-text="Filtro de rango para rangos de texto" border="false":::

> [!NOTE]
> El uso de facetas en intervalos de valores es un requisito habitual de las aplicaciones de búsqueda. Para obtener más información y ejemplos, vea [Creación de filtro de faceta](search-filters-facets.md).

## <a name="example-5-geo-search"></a>Ejemplo 5: Búsqueda georreferenciada

El índice de ejemplo incluye un campo geo_location con las coordenadas de latitud y longitud. En este ejemplo se usa la [función geo.distance](search-query-odata-geo-spatial-functions.md#examples), que filtra en documentos dentro de la circunferencia de un punto de partida, hasta la distancia arbitraria (en kilómetros) que proporcione. Puede ajustar el último valor en la consulta (4) para reducir o ampliar el área expuesta de la consulta.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "business_title, work_location"
    }
```

Para obtener resultados más legibles, los resultados de la búsqueda se recortan para incluir el título, el puesto y la ubicación de trabajo. Las coordenadas iniciales se obtuvieron de un documento aleatorio del índice (en este caso, para una ubicación de trabajo en la isla Staten).

  :::image type="content" source="media/search-query-simple-examples/geo-search.png" alt-text="Mapa de la isla Staten" border="false":::

## <a name="example-6-search-precision"></a>Ejemplo 6: Precisión de la búsqueda

Las consultas de términos son términos individuales (puede que muchos de ellos) que se evalúan de forma independiente. Las consultas de frases se incluyen entre comillas y se evalúan como una cadena textual. La precisión de la coincidencia se controla mediante operadores y el parámetro searchMode.

Ejemplo 1: `search=fire` devuelve 140 resultados, donde todas las coincidencias contienen la palabra "fire" en algún lugar del documento.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire"
    }
```

Ejemplo 2: `search=fire department` devuelve 2002 resultados. Se devuelven coincidencias para documentos que contienen las palabras "fire" o "department".

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire department"
    }
```

Ejemplo 3: `search="fire department"` devuelve 77 resultados. Al incluir la cadena entre comillas, se crea una frase de búsqueda textual de ambos términos y las coincidencias encontradas en términos con tokens en el índice están formadas por los términos combinados. Esto explica por qué una búsqueda como `search=+fire +department` no es equivalente. Ambos términos son obligatorios, pero se buscan de forma independiente. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
    "count": true,
    "search": "\"fire department\""
    }
```

> [!Note]
> Dado que una consulta de frase se especifica mediante comillas, en este ejemplo se agrega un carácter de escape (`\`) para conservar la sintaxis.

## <a name="example-7-booleans-with-searchmode"></a>Ejemplo 7: Valores booleanos con searchMode

La sintaxis simplificada es compatible con operadores booleanos en forma de caracteres (`+, -, |`). El parámetro searchMode informa de las compensaciones entre precisión y recuperación, donde **`searchMode=any`** favorece la recuperación (la coincidencia con cualquier criterio permite incluir a un documento en el conjunto de resultados) y **`searchMode=all`** favorece la precisión (deben cumplirse todos los criterios). 

El valor predeterminado es **`searchMode=any`** , lo que puede llevar a confusión si se apila una consulta con varios operadores y se obtienen resultados más amplios, en lugar de más restringidos. Esto sucede especialmente con el operador NOT, donde los resultados incluyen todos los documentos que "no contienen" un término específico.

Con el parámetro searchMode predeterminado (cualquiera), se devuelven 2800 documentos: aquellos que contienen el término de varias partes "fire department", más todos los documentos que no contienen el término "Metrotech Center".

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "any"
    }
```

La respuesta de esta consulta debe tener un aspecto similar a la siguiente captura de pantalla.

  :::image type="content" source="media/search-query-simple-examples/searchmodeany.png" alt-text="modo de búsqueda: any" border="false":::

Al cambiar a **`searchMode=all`** , se aplica un efecto acumulativo en los criterios y se devuelve un conjunto de resultados más pequeño (21 documentos) que consta de los documentos que contienen la frase completa "fire department", menos los trabajos con la dirección de Metrotech Center.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "all"
    }
```

  :::image type="content" source="media/search-query-simple-examples/searchmodeall.png" alt-text="modo de búsqueda: all" border="false":::

## <a name="example-8-structuring-results"></a>Ejemplo 8: Estructuración de los resultados

Varios parámetros controlan qué campos se incluyen en la búsqueda de resultados, el número de documentos devueltos en cada lote y el criterio de ordenación. En este ejemplo reaparecen algunos de los ejemplos anteriores, al limitar los resultados a campos específicos mediante la instrucción **`$select`** y criterios de búsqueda textual, devolviendo 82 coincidencias.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description"
    }
```

Al anexarlo al ejemplo anterior, podrá ordenar por título. Esta ordenación funciona porque civil_service_title *se puede ordenar* en el índice.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title"
    }
```

La paginación de resultados se implementa mediante el parámetro **`$top`** , devolviendo en este caso los cinco documentos principales:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5"
    }
```

Para obtener los cinco siguientes, omita el primer lote:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5",
      "skip": "5"
    }
```

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