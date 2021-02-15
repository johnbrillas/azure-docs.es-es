---
title: Búsqueda en blobs de texto sin formato
titleSuffix: Azure Cognitive Search
description: Configure un indexador de búsqueda para extraer texto sin formato de blobs de Azure para la búsqueda de texto completo en Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/01/2021
ms.openlocfilehash: b8881d3fa7ade08da103c5af4b828a12e74cc355
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99509459"
---
# <a name="how-to-index-plain-text-blobs-in-azure-cognitive-search"></a>Indexación de blobs de texto sin formato en Azure Cognitive Search

Si usa un [indexador de blobs](search-howto-indexing-azure-blob-storage.md) para extraer texto de blobs que permite búsquedas para la búsqueda de texto completo, se puede asignar un modo de análisis para obtener mejores resultados de la indexación. De forma predeterminada, el indexador analiza el contenido de los blobs como un único fragmento de texto. Sin embargo, si todos los blobs contienen texto sin formato con la misma codificación, el rendimiento de la indexación se puede mejorar notablemente mediante el uso del modo de análisis`text`.

Las recomendaciones para usar el análisis `text` incluyen:

+ El tipo de archivo es .txt
+ Los archivos son de cualquier tipo, pero el propio contenido es texto (por ejemplo, código fuente del programa, HTML, XML, etc.). En el caso de los archivos de un lenguaje de marcado, los caracteres de la sintaxis aparecerán como texto estático.

Recuerde que todos los indexadores se serializan en JSON. De forma predeterminada, el contenido de todo el archivo de texto completo se indexará en un campo grande como `"content": "<file-contents>"`. Las nuevas líneas y las instrucciones devueltas se insertan en el campo de contenido y se expresan como `\r\n\`.

Si desea un resultado más granular, y si el tipo de archivo es compatible, tenga en cuenta las siguientes soluciones:

+ Modo de análisis [`delimitedText`](search-howto-index-csv-blobs.md), si el origen es CSV
+ [`jsonArray` o `jsonLines`](search-howto-index-json-blobs.md), si el origen es JSON

Una tercera opción para dividir el contenido en varias partes requiere características avanzadas en forma de [enriquecimiento con IA](cognitive-search-concept-intro.md). Agrega análisis que identifica y asigna fragmentos del archivo a distintos campos de búsqueda. Es posible que encuentre una solución completa o parcial mediante [aptitudes integradas](cognitive-search-predefined-skills.md), pero una solución más probable sería un modelo de aprendizaje que conoce el contenido, articulado en un modelo de aprendizaje personalizado, envuelto en una [aptitud personalizada](cognitive-search-custom-skill-interface.md).

## <a name="set-up-plain-text-indexing"></a>Configuración de la indexación del texto sin formato

Para indexar blobs de texto sin formato, cree o actualice una definición de indexador con la propiedad de configuración `parsingMode` a `text`en una solicitud [Crear indexador](/rest/api/searchservice/create-indexer):

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "parsingMode" : "text" } }
}
```

De forma predeterminada, se da por hecha la codificación `UTF-8`. Para especificar otra, use la propiedad de configuración `encoding`: 

```http
{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
}
```

## <a name="request-example"></a>Ejemplo de solicitud

Los modos de análisis se especifican en la definición del indexador.

```http
POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
  "name" : "my-plaintext-indexer",
  "dataSourceName" : "my-blob-datasource",
  "targetIndexName" : "my-target-index",
  "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
}
```

## <a name="next-steps"></a>Pasos siguientes

+ [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
+ [Configuración de un indexador de blobs](search-howto-indexing-azure-blob-storage.md)
+ [Introducción a la indexación de blobs](search-blob-storage-integration.md)