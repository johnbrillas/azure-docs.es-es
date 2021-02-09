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
ms.openlocfilehash: 422346430e32ccb8745d5a5d829c5d61089a99c6
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430435"
---
# <a name="how-to-index-plain-text-blobs-in-azure-cognitive-search"></a>Indexación de blobs de texto sin formato en Azure Cognitive Search

Si usa un [indexador de blobs](search-howto-indexing-azure-blob-storage.md) para extraer texto que permite búsquedas para la búsqueda de texto completo, se pueden invocar varios modos de análisis para obtener mejores resultados de la indexación. De forma predeterminada, el indexador analiza el contenido de los blobs como un único fragmento de texto. Sin embargo, si todos los blobs contienen texto sin formato con la misma codificación, el rendimiento de la indexación se puede mejorar notablemente mediante el uso del modo de análisis`text`.

El modo de análisis `text` se debe usar cuando:

+ El tipo de archivo es .txt
+ Los archivos son de cualquier tipo, pero el propio contenido es texto (por ejemplo, código fuente del programa, HTML, XML, etc.). En el caso de los archivos de un lenguaje de marcado, los caracteres de la sintaxis aparecerán como texto estático.

Recuerde que los indexadores se serializan en JSON. El contenido de todo el archivo de texto completo se indexará en un campo grande como `"content": "<file-contents>"`. Las instrucciones new line y return se expresan como `\r\n\`.

Si desea un resultado más granular, tenga en cuenta las siguientes soluciones:

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