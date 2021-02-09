---
title: Indexación de blobs que contienen varios documentos
titleSuffix: Azure Cognitive Search
description: Rastree blobs de Azure para buscar contenido de texto mediante el indexador de blobs de Azure Cognitive Search, en el que cada blob podría producir uno o más documentos de índice de búsqueda.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/01/2021
ms.openlocfilehash: ea22b3cff8a0303c4e6698db4090df0f5ed2153a
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430987"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Indexación de blobs para producir varios documentos de búsqueda

De manera predeterminada, un indexador de blobs tratará el contenido de un blob como un único documento de búsqueda. Si quiere una representación más granular del blob en un índice de búsqueda, puede establecer valores de **parsingMode** para crear varios documentos de búsqueda de un blob. Los valores de **parsingMode** que dan como resultado varios documentos de búsqueda incluyen `delimitedText` (para [CSV](search-howto-index-csv-blobs.md)) y `jsonArray` o `jsonLines` (para [JSON](search-howto-index-json-blobs.md)).

Cuando se usa cualquiera de estos modos de análisis, los nuevos documentos de búsqueda que surgen deben tener claves de documento únicas, y se produce un problema al determinar de dónde procede ese valor. El blob primario tiene al menos un valor único en forma de `metadata_storage_path property`, pero si aporta ese valor a más de un documento de búsqueda, la clave ya no es única en el índice.

Para solucionar este problema, el indexador de blobs genera `AzureSearch_DocumentKey`, que identifica de forma única cada documento de búsqueda secundario que se crea a partir del único elemento primario del blob. En este artículo se explica cómo funciona esta característica.

## <a name="one-to-many-document-key"></a>Clave de documento de uno a varios

Cada documento que se muestra en un índice de Azure Cognitive Search se identifica de forma única mediante una clave de documento. 

Cuando no se especifica ningún modo de análisis y, si no hay ninguna [asignación de campos explícita](search-indexer-field-mappings.md) en la definición del indexador para la clave de documento de búsqueda, el indexador de blobs asigna `metadata_storage_path property` automáticamente como la clave de documento. Esta asignación garantiza que cada blob aparezca como un documento de búsqueda distinto y le ahorrará el paso de tener que crear esta asignación de campos (normalmente, solo se asignan automáticamente los campos que tienen nombres y tipos idénticos).

Al usar cualquiera de los modos de análisis mencionados anteriormente, un blob se asigna a "varios" documentos de búsqueda, lo que provoca que una clave de documento que se basa únicamente en los metadatos del blob no sea adecuada. Para superar esta restricción, Azure Cognitive Search es capaz de generar una clave de documento de "uno a varios" para cada entidad individual extraída de un blob. Esta propiedad se denomina AzureSearch_DocumentKey y se agrega a cada entidad individual extraída del blob. Se garantiza que el valor de esta propiedad sea único para cada entidad individual entre blobs y las entidades se mostrarán como documentos de búsqueda independientes.

De manera predeterminada, cuando no se especifica ninguna asignación de campo explícita para el campo de índice de la clave, se le asigna `AzureSearch_DocumentKey` mediante la función `base64Encode` de asignación de campos.

## <a name="example"></a>Ejemplo

Supongamos que hay una definición de índice con los siguientes campos:

+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Y que el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

```json
{ "temperature": 100, "pressure": 100, "timestamp": "2020-02-13T00:00:00Z" }
{ "temperature" : 33, "pressure" : 30, "timestamp": "2020-02-14T00:00:00Z" }
```

_Blob2.json_

```json
{ "temperature": 1, "pressure": 1, "timestamp": "2019-01-12T00:00:00Z" }
{ "temperature" : 120, "pressure" : 3, "timestamp": "2017-05-11T00:00:00Z" }
```

Al crear un indexador y establecer el valor de **parsingMode** en `jsonLines`: sin especificar ninguna asignación de campo explícita para el campo de clave, se aplicará la siguiente asignación de forma implícita.

```http
{
    "sourceFieldName" : "AzureSearch_DocumentKey",
    "targetFieldName": "id",
    "mappingFunction": { "name" : "base64Encode" }
}
```

Esta configuración dará lugar a claves de documento sin ambigüedades, de forma similar a la siguiente ilustración (el identificador codificado en Base64 se ha abreviado para mayor brevedad).

| Id. | temperatura | presión | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2020-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2020-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2019-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2017-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Asignación de campos personalizados para el campo de clave de índice

Adoptando la misma definición de índice que en el ejemplo anterior, supongamos que el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

```json
recordid, temperature, pressure, timestamp
1, 100, 100,"2019-02-13T00:00:00Z" 
2, 33, 30,"2019-02-14T00:00:00Z" 
```

_Blob2.json_

```json
recordid, temperature, pressure, timestamp
1, 1, 1,"2018-01-12T00:00:00Z" 
2, 120, 3,"2013-05-11T00:00:00Z" 
```

Cuando se crea un indexador con **parsingMode** `delimitedText`, puede resultar natural configurar una función de asignación de campos para el campo de clave como se indica a continuación:

```http
{
    "sourceFieldName" : "recordid",
    "targetFieldName": "id"
}
```

Sin embargo, esta asignación _no_ tendrá como resultado cuatro documentos que se muestran en el índice, porque el campo `recordid` no es único _entre blobs_. Por lo tanto, se recomienda hacer uso de la asignación de campos implícita que se aplica desde la propiedad `AzureSearch_DocumentKey` en el campo de índice de la clave para los modos de análisis de "uno a varios".

Si quiere configurar la asignación de campos explícita, asegúrese de que _sourceField_ sea diferente para cada entidad individual **entre todos los blobs**.

> [!NOTE]
> El enfoque usado por `AzureSearch_DocumentKey` para garantizar la unicidad por entidad extraída está sujeto a cambios y, por tanto, no debe confiar en su valor para las necesidades de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Si aún no está familiarizado con la estructura y el flujo de trabajo básicos de la indexación de blobs, debería consultar primero [Indexación de Azure Blob Storage con Azure Cognitive Search](search-howto-index-json-blobs.md). Para más información sobre los modos de análisis de los distintos tipos de contenido de blobs, revise los artículos siguientes.

> [!div class="nextstepaction"]
> [Indexación de blobs CSV](search-howto-index-csv-blobs.md)
> [Indexación de blobs JSON](search-howto-index-json-blobs.md)
