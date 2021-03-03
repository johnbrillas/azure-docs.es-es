---
title: Configuración del algoritmo de clasificación de similitudes
titleSuffix: Azure Cognitive Search
description: Cómo establecer el algoritmo de similitud para probar el nuevo algoritmo de similitud para la clasificación
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 9f806b512ae8e118fca8f32115c8be3b493fd681
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101677778"
---
# <a name="configure-ranking-algorithms-in-azure-cognitive-search"></a>Configuración de algoritmos de clasificación en Azure Cognitive Search

Azure Cognitive Search admite dos algoritmos de clasificación de similitudes:

+ Un algoritmo de *similitud clásica*, que usan todos los servicios de búsqueda hasta el 15 de julio de 2020.
+ Una implementación del algoritmo *BM25 de Okapi*, que usan todos los servicios de búsqueda creados después del 15 de julio.

La clasificación BM25 es la nueva predeterminada porque tiende a generar clasificaciones de búsqueda que se alinean mejor con las expectativas de los usuarios. También permite opciones de configuración para optimizar los resultados en función de factores como el tamaño del documento. En el caso de los servicios creados después del 15 de julio de 2020, BM25 se usa automáticamente y es el único algoritmo similarity. Si intenta establecer la similitud en ClassicSimilarity en un nuevo servicio, se devolverá un error 400 porque el servicio no admite ese algoritmo.

En el caso de los servicios anteriores creados antes del 15 de julio de 2020, la similitud clásica sigue siendo el algoritmo predeterminado. Estos servicios pueden establecer propiedades en un índice de búsqueda para invocar a BM25, como se explica a continuación. Si va a cambiar de la similitud clásica a BM25, puede que observe algunas diferencias con respecto a cómo se ordenan los resultados de búsqueda.

> [!NOTE]
> La búsqueda semántica es un algoritmo de reclasificación semántica adicional que reduce aún más la diferencia entre las expectativas y los resultados. A diferencia de los demás algoritmos, es una característica complementaria que recorre en iteración un conjunto de resultados existente. Para usar el algoritmo de búsqueda semántica en versión preliminar, debe crear un servicio y especificar un [tipo de consulta semántica](semantic-how-to-query-request.md). Para más información, consulte [Introducción a la búsqueda semántica](semantic-search-overview.md).

## <a name="create-a-search-index-for-bm25-scoring"></a>Creación de un índice de búsqueda para la puntuación BM25

Si ejecuta un servicio de búsqueda que se creó antes del 15 de julio de 2020, puede establecer la propiedad de similitud en BM25Similarity o ClassicSimilarity en la definición del índice. Si se omite la propiedad similarity o se establece en NULL, el índice usará el algoritmo clásico.

El algoritmo de similitud solo se puede establecer en el momento de crear el índice. Sin embargo, una vez creado un índice con BM25, puede actualizar el existente para establecer o modificar los parámetros de BM25.

| Biblioteca de cliente | Propiedad de similitud |
|----------------|---------------------|
| .NET  | [SearchIndex.Similarity](/dotnet/api/azure.search.documents.indexes.models.searchindex.similarity) |
| Java | [SearchIndex.setSimilarity](/java/api/com.azure.search.documents.indexes.models.searchindex.setsimilarity) |
| JavaScript | [SearchIndex.Similarity](/javascript/api/@azure/search-documents/searchindex#similarity) |
| Python | [Propiedad de similitud en SearchIndex](/python/api/azure-search-documents/azure.search.documents.indexes.models.searchindex) |

### <a name="rest-example"></a>Ejemplo de REST

También puede usar la [API REST](/rest/api/searchservice/create-index), como se muestra en el ejemplo siguiente:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30
{
    "name": "indexName",
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true
        },
        {
            "name": "name",
            "type": "Edm.String",
            "searchable": true,
            "analyzer": "en.lucene"
        },
        ...
    ],
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity"
    }
}
```

## <a name="bm25-similarity-parameters"></a>Parámetros de similitud de BM25

La similitud de BM25 agrega dos parámetros personalizables por el usuario para controlar la puntuación de relevancia calculada. Puede establecer los parámetros de BM25 durante la creación del índice o como una actualización de este si durante la creación del índice se especificó el algoritmo BM25.

| Propiedad | Tipo | Descripción |
|----------|------|-------------|
| k1 | number | Controla la función de escalado entre la frecuencia de cada uno de los términos coincidentes y la puntuación de relevancia final de un par documento-consulta. Los valores suelen oscilar entre 0,0 y 3,0, siendo 1,2 el predeterminado. </br></br>Un valor de 0,0 representa un "modelo binario", en el que la contribución de un único término coincidente es la misma para todos los documentos coincidentes, con independencia del número de veces que ese término aparezca en el texto, mientras que un valor k1 mayor permite que la puntuación siga aumentando a medida que se encuentren más instancias del mismo término en el documento. </br></br>El uso de un valor de k1 mayor puede ser importante en los casos en los que se espera que varios términos formen parte de una consulta de búsqueda. En estos casos, podemos favorecer documentos que coincidan con muchos de los diferentes términos de consulta buscados frente a los documentos que solo coinciden con uno varias veces. Por ejemplo, al consultar en el índice los documentos que contienen los términos "Apollo Spaceflight", podemos reducir la puntuación de un artículo sobre la mitología griega que contenga el término "Apollo" unas cuantas veces sin menciones a "Spaceflight", en comparación con otro artículo que mencione explícitamente "Apollo" y "Spaceflight" solo unas pocas veces. |
| b | number | Controla cómo afecta la longitud de un documento a la puntuación de relevancia. Los valores oscilan entre 0 y 1, siendo 0,75 el predeterminado. </br></br>Un valor de 0,0 significa que la longitud del documento no influye en la puntuación, mientras que un valor de 1,0 significa que el impacto de la frecuencia de los términos en la puntuación de relevancia se normalizará según la longitud del documento. </br></br>La normalización de la frecuencia de los términos según la longitud del documento es útil en los casos en los que queremos penalizar los documentos más largos. En algunos casos, es más probable que los documentos más largos (como una novela completa) contengan muchos términos irrelevantes, en comparación con documentos mucho más cortos. |

### <a name="setting-k1-and-b-parameters"></a>Establecimiento de los parámetros k1 y b

Para establecer o modificar los valores b o k1, agréguelos al objeto de similitud de BM25. La configuración o el cambio de estos valores en un índice existente dejará el índice sin conexión durante al menos unos segundos, lo que provocará errores en las solicitudes de indexación y consulta activas. Por lo tanto, debe establecer el parámetro "allowIndexDowntime=true" de la solicitud de actualización:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30&allowIndexDowntime=true
{
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
}
```

## <a name="see-also"></a>Consulte también  

+ [Referencia de API de REST](/rest/api/searchservice/)
+ [Adición de perfiles de puntuación al índice](index-add-scoring-profiles.md)
+ [Create Index API](/rest/api/searchservice/create-index)
+ [SDK de .NET de Azure Cognitive Search](/dotnet/api/overview/azure/search)