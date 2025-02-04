---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 16ab569428510b3b423d6727fd31ee450a8d197e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100381618"
---
### <a name="default"></a>Valor predeterminado

Los siguientes tipos de parámetros se pueden usar en el blob de desencadenamiento:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> requiere un enlace "inout" `direction` en *function.json* o `FileAccess.ReadWrite` en una biblioteca de clases de C#.

Si intenta enlazar a uno de los tipos de SDK de Storage y recibe un mensaje de error, asegúrese de que hace referencia a [la versión de SDK de Storage correcta](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x).

Solo se recomienda establecer un enlace con `string` o `Byte[]` si el blob es pequeño, ya que todo su contenido se carga en la memoria. Por lo general, es preferible usar un tipo `Stream` o `CloudBlockBlob`. Para obtener más información, consulte [Uso de simultaneidad y memoria](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) más adelante en este artículo.

### <a name="additional-types"></a>Tipos adicionales

Las aplicaciones que usan la [versión 5.0.0 o posterior de la extensión de Azure Storage](../articles/azure-functions/functions-bindings-storage-blob.md#storage-extension-5x-and-higher) también pueden usar tipos de [Azure SDK para .NET](/dotnet/api/overview/azure/storage.blobs-readme). En esta versión se elimina la compatibilidad con los tipos `ICloudBlob`, `CloudBlockBlob`, `CloudPageBlob` y `CloudAppendBlob` heredados en favor de los siguientes tipos:

- [BlobClient](/dotnet/api/azure.storage.blobs.blobclient)<sup>1</sup>
- [BlockBlobClient](/dotnet/api/azure.storage.blobs.specialized.blockblobclient)<sup>1</sup>
- [PageBlobClient](/dotnet/api/azure.storage.blobs.specialized.pageblobclient)<sup>1</sup>
- [AppendBlobClient](/dotnet/api/azure.storage.blobs.specialized.appendblobclient)<sup>1</sup>
- [BlobBaseClient](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient)<sup>1</sup>

<sup>1</sup> requiere un enlace "inout" `direction` en *function.json* o `FileAccess.ReadWrite` en una biblioteca de clases de C#.

Para obtener ejemplos de uso de estos tipos, consulte el [repositorio de GitHub de la extensión](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Microsoft.Azure.WebJobs.Extensions.Storage.Blobs#examples).
