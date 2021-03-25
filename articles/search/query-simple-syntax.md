---
title: Sintaxis de consulta simplificada
titleSuffix: Azure Cognitive Search
description: Referencia de la sintaxis de consulta simple que se usa para las consultas de búsqueda de texto completo en Azure Cognitive Search.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: f679d6fbab57bcbcccc09b722f6b2f670df49eb2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97516584"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Sintaxis de consulta simple en Azure Cognitive Search

Azure Cognitive Search implementa dos lenguajes de consulta basados en Lucene: [Analizador de consultas simple](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) y [Analizador de consultas de Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). El analizador simple es más flexible e intentará interpretar una solicitud incluso si no está perfectamente compuesta. Debido a esta flexibilidad, es el valor predeterminado para las consultas en Azure Cognitive Search.

La sintaxis simple se usa para las expresiones de consulta que se pasan en el parámetro **`search`** de una solicitud de [búsqueda de documentos (API de REST)](/rest/api/searchservice/search-documents); no se debe confundir con la [sintaxis de OData](query-odata-filter-orderby-syntax.md) que se usa para las expresiones [ **`$filter`**](search-filters.md) y [ **`$orderby`**](search-query-odata-orderby.md) de la misma solicitud. Los parámetros de OData tienen diferentes reglas y sintaxis para crear consultas, cadenas de escape y otros elementos.

Aunque el analizador simple se basa en la clase del [analizador de consultas simples de Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html), la implementación en Cognitive Search excluye la búsqueda aproximada. Si necesita realizar una [búsqueda aproximada](search-query-fuzzy.md), considere la posibilidad de usar la [sintaxis de consulta completa de Lucene](query-lucene-syntax.md) alternativa en su lugar.

## <a name="example-simple-syntax"></a>Ejemplo (sintaxis simple)

Aunque **`queryType`** se establece a continuación, es el valor predeterminado y se puede omitir a menos que se revierta de un tipo alternativo. El ejemplo siguiente es una búsqueda mediante términos independientes, con el requisito de que todos los documentos coincidentes incluyan "pool".

```http
POST https://{{service-name}}.search.windows.net/indexes/hotel-rooms-sample/docs/search?api-version=2020-06-30
{
  "queryType": "simple",
  "search": "budget hotel +pool",
  "searchMode": "all"
}
```

El parámetro **`searchMode`** es significativo en este ejemplo. Cada vez que los operadores booleanos están en la consulta, normalmente debe establecer `searchMode=all` para asegurarse de que se busca la coincidencia con *todos* los criterios. De lo contrario, puede usar el valor predeterminado `searchMode=any` que favorece la coincidencia sobre la precisión.

Para ver ejemplos adicionales, consulte los [ejemplos de sintaxis de consulta simple](search-query-simple-examples.md). Para más información sobre la solicitud de consultas y los parámetros, consulte la [Búsqueda de documentos (API de REST)](/rest/api/searchservice/Search-Documents).

## <a name="keyword-search-on-terms-and-phrases"></a>Búsqueda de palabras clave en términos y frases

Las cadenas que se pasan al parámetro **`search`** pueden incluir términos o frases en cualquier idioma compatible, operadores booleanos, operadores de precedencia, caracteres comodín o prefijos para las consultas de tipo "comienza con", caracteres de escape y caracteres de codificación de direcciones URL. El parámetro **`search`** es opcional. Si no se especifica, la búsqueda (`search=*` o `search=" "`) devuelve los 50 documentos principales de forma arbitraria (sin clasificar).

+ Una *búsqueda de términos* es una consulta para uno o varios términos, donde cualquiera de ellos se considera una coincidencia.

+ Una *búsqueda de frases* es una frase exacta entre comillas `" "`. Por ejemplo, mientras que ```Roach Motel``` (sin comillas) buscaría documentos que contuvieran ```Roach``` y/o ```Motel``` en cualquier lugar y en cualquier orden, ```"Roach Motel"``` (con comillas) solo encontrará coincidencias en documentos que contengan esa frase completa junta y en ese orden (el análisis léxico sigue aplicándose). 

  En función de su cliente de búsqueda, es posible que quiera aplicar una marca de escape a las comillas al realizar una búsqueda de frases. Por ejemplo, en una solicitud POST de Postman, una frase de búsqueda en `"Roach Motel"` en el cuerpo de la solicitud se especificará como `"\"Roach Motel\""`.

De forma predeterminada, todos los términos o frases pasados en el parámetro **`search`** se someten a un análisis léxico. Asegúrese de que comprende el comportamiento de la tokenización del analizador que está utilizando. A menudo, cuando los resultados de la consulta son inesperados, se puede realizar un seguimiento de la manera en que se tokenizan los términos en el momento de la consulta.

Cualquier texto con uno o varios términos se considera un punto inicial válido para la ejecución de consultas. Azure Cognitive Search encontrará coincidencias en los documentos que contengan cualquiera de los términos o todos ellos, incluidas las variaciones encontradas durante el análisis del texto.

Tan sencillo como suena: hay un aspecto de la ejecución de consultas en Azure Cognitive Search que *podría* producir resultados inesperados, aumentando (en lugar de disminuir) los resultados de búsqueda a medida que se agregan más términos y operadores a la cadena de entrada. Que se produzca o no esta expansión realmente depende de la inclusión de un operador NOT, combinado con una configuración del parámetro **`searchMode`** , que determina cómo se interpreta NOT en términos de comportamientos de AND u OR. Para obtener más información, consulte el operador NOT en [Operadores booleanos](#boolean-operators).

## <a name="boolean-operators"></a>Operadores booleanos

Puede incrustar operadores booleanos en una cadena de consulta para mejorar la precisión de una coincidencia. En la sintaxis simple, los operadores booleanos están basados en caracteres. Recuerde que no se admiten operadores de texto, como la palabra AND.

| Carácter | Ejemplo | Uso |
|----------- |--------|-------|
| `+` | `pool + ocean` | Una operación AND. Por ejemplo, `pool + ocean` estipula que un documento debe contener ambos términos.|
| `|` | `pool | ocean` | Una operación OR busca una coincidencia cuando se encuentra cualquiera de los términos. En este ejemplo, el motor de consultas devolverá una coincidencia en los documentos que contengan `pool`, `ocean` o ambos. Como OR es el operador de conjunción predeterminado, podría también dejarlo fuera, de forma que `pool ocean` es el equivalente de `pool | ocean`.|
| `-` | `pool – ocean` | Una operación NOT devuelve coincidencias en documentos que excluyen el término. <br/><br/>Para obtener el comportamiento esperado en una expresión NOT, considere la posibilidad de establecer **`searchMode=all`** en la solicitud. De lo contrario, en el valor predeterminado **`searchMode=any`** , obtendrá coincidencias en `pool`, además de las coincidencias en todos los documentos que no contengan `ocean`, lo que podría ser una gran cantidad de documentos. El parámetro **`searchMode`** de una solicitud de consulta controla si un término con el operador NOT también cuenta con AND u OR en otros términos de la consulta (suponiendo que no haya ningún operador `+` o `|` en los demás términos). Al usar **`searchMode=all`** , aumenta la precisión de las consultas al incluirse menos resultados y, de forma predeterminada, se interpretará como "AND NOT". <br/><br/>Al decidir sobre una configuración de **`searchMode`** , tenga en cuenta los patrones de interacción del usuario para las consultas que realice en varias aplicaciones. Es más probable que los usuarios que buscan información incluyan un operador en una consulta, en lugar de sitios de comercio electrónico que tengan más estructuras de navegación integradas. |

<a name="prefix-search"></a>

## <a name="prefix-queries"></a>Consultas de prefijo

En el caso de las consultas de tipo "empieza con", agregue un operador de sufijo (`*`) como marcador de posición para la última parte de un término. Una consulta de prefijo debe comenzar con al menos un carácter alfanumérico para poder agregar el operador de sufijo.

| Carácter | Ejemplo | Uso |
|----------- |--------|-------|
| `*` | `lingui*` coincidirá con "linguistic" o "linguini". | El asterisco (`*`) representa uno o varios caracteres de longitud arbitraria, sin distinción de mayúsculas y minúsculas.  |

De forma similar a los filtros, una consulta de prefijo busca una coincidencia exacta. Como tal, no hay ninguna puntuación de relevancia (todos los resultados reciben una puntuación de búsqueda de 1,0). Tenga en cuenta que las consultas de prefijo pueden ser lentas, en especial si el índice es grande y el prefijo está formado por un número pequeño de caracteres. Podría funcionar más rápido una metodología alternativa, como la tokenización de n-gramas perimetrales.

La sintaxis simple solo admite la coincidencia de prefijos. En cuanto a la coincidencia de sufijos o interfijos en la parte inicial o intermedia de un término, use la [sintaxis de Lucene completa para la búsqueda con caracteres comodín](query-lucene-syntax.md#bkmk_wildcard).

## <a name="escaping-search-operators"></a>Escape de los operadores de búsqueda  

En la sintaxis simple, los operadores de búsqueda incluyen estos caracteres: `+ | " ( ) ' \`  

Si alguno de estos caracteres forma parte de un token en el índice, aplique el escape anteponiendo un carácter de barra diagonal inversa (`\`) en la consulta. Por ejemplo, supongamos que ha usado un analizador personalizado para la tokenización de todo el término y el índice contiene la cadena "Luxury+Hotel". Para obtener una coincidencia exacta en este token, inserte un carácter de escape: `search=luxury\+hotel`.

Con el fin de simplificar las cosas para los casos más típicos, existen dos excepciones a esta regla donde la secuencia de escape no es necesaria:  

+ El operador NOT `-` necesita escaparse solamente si es el primer carácter después de un espacio en blanco. Si `-` aparece en el centro (por ejemplo, en `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`), puede omitir el escape.

+ El operador de sufijo `*` solo debe ser un carácter de escape si es el último carácter antes de un espacio en blanco. Si `*` aparece en el centro (por ejemplo, en `4*4=16`), no se necesita ningún escape.

> [!NOTE]  
> De forma predeterminada, el analizador estándar eliminará y dividirá palabras en guiones, espacios en blanco, y comercial y otros caracteres durante el [análisis léxico](search-lucene-query-architecture.md#stage-2-lexical-analysis). Si necesita que caracteres especiales permanezcan en la cadena de consulta, es posible que necesite un analizador que los conserve en el índice. Algunas opciones incluyen [analizadores de lenguaje](index-add-language-analyzers.md) natural de Microsoft, que conserva palabras con guiones o un analizador personalizado para patrones más complejos. Para más información, vea [Términos parciales, patrones y caracteres especiales](search-query-partial-matching.md).

## <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codificación de caracteres reservados y no seguros en las direcciones URL

Asegúrese de que todos los caracteres reservados y no seguros estén codificados en una dirección URL. Por ejemplo, "#" es un carácter no seguro porque es un identificador de delimitador o fragmento en una dirección URL. El carácter debe codificarse con `%23` si se usa en una dirección URL. "&" y "=" son ejemplos de caracteres reservados, ya que delimitan parámetros y especifican valores en Azure Cognitive Search. Para obtener más información, consulte [RFC1738: Localizador uniforme de recursos (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Los caracteres no seguros son ``" ` < > # % { } | \ ^ ~ [ ]``. Los caracteres reservados son `; / ? : @ = + &`.

## <a name="special-characters"></a>Caracteres especiales

En algunas circunstancias, es posible que quiera buscar un carácter especial, como el emoji "❤" o el signo "€". En ese caso, asegúrese de que el analizador que usa no filtre esos caracteres. Recuerde que el analizador estándar omite muchos caracteres especiales y los excluye del índice.

Aquellos analizadores que dividen en tokens los caracteres especiales incluyen el analizador "whitespace", que tiene en cuenta todas las secuencias de caracteres separadas por espacios en blanco como tokens, por lo que la cadena "❤" se consideraría un token. Asimismo, un analizador de idioma como el analizador de inglés de Microsoft ("en.microsoft") debe incluir la cadena "€" como un token. Puede [probar un analizador](/rest/api/searchservice/test-analyzer) para ver qué tokens genera para una consulta determinada.

Al usar caracteres Unicode, asegúrese de que los símbolos se escapen correctamente en la dirección URL de la consulta (por ejemplo, para "❤" usaría la secuencia de escape `%E2%9D%A4+`). Postman realiza esta traducción automáticamente.  

## <a name="precedence-grouping"></a>Precedencia (agrupación)

Puede usar paréntesis para crear subconsultas, incluidos los operadores dentro de la instrucción entre paréntesis. Por ejemplo, `motel+(wifi|luxury)` buscará los documentos que contengan el término "motel" y "wifi" o "luxury" (o ambos).

## <a name="query-size-limits"></a> Límites de tamaño de consulta

Si la aplicación genera consultas de búsqueda mediante programación, se recomienda que la diseña de manera que no genere consultas de tamaño ilimitado. 

+ En cuanto al operador GET, la longitud de la dirección URL no puede superar los 8 KB.

+ En el caso de POST (y cualquier otra solicitud), donde el cuerpo de la solicitud incluye `search` y otros parámetros como `filter` y `orderby`, el tamaño máximo es de 16 MB, donde el número máximo de cláusulas de `search` (expresiones separadas por AND, OR, etc.) es 1024. También hay un límite de aproximadamente 32 KB en el tamaño de cualquier término individual en una consulta. Para obtener más información, consulte los [límites de solicitudes de API](search-limits-quotas-capacity.md#api-request-limits).

## <a name="next-steps"></a>Pasos siguientes

Si va a crear consultas mediante programación, revise el artículo [Búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md) para comprender las fases del procesamiento de consultas y las implicaciones del análisis de texto.

Asimismo, también puede revisar los siguientes artículos para obtener más información sobre la creación de consultas:

+ [Ejemplos de consulta para una búsqueda simple](search-query-simple-examples.md)
+ [Ejemplos de consulta para una búsqueda completa de Lucene](search-query-lucene-examples.md)
+ [API de REST de documentos de búsqueda](/rest/api/searchservice/Search-Documents)
+ [Sintaxis de consulta de Lucene](query-lucene-syntax.md)
+ [Filtrado y selección (OData) de sintaxis de expresiones](query-odata-filter-orderby-syntax.md)