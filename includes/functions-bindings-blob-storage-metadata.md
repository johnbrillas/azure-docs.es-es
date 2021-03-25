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
ms.openlocfilehash: c6b038297945ca900508a822460e1358a2524d23
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102455925"
---
El desencadenador de blobs proporciona varias propiedades de metadatos. Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estos valores tienen la misma semántica que el tipo [CloudBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblob).

|Propiedad  |Tipo  |Description  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Ruta de acceso del blob de desencadenamiento.|
|`Uri`|`System.Uri`|Es el identificador URI del blob correspondiente a la ubicación principal.|
|`Properties` |[BlobProperties](/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Son las propiedades del sistema del blob. |
|`Metadata` |`IDictionary<string,string>`|Son los metadatos definidos por el usuario para el blob.|

Por ejemplo, los ejemplos de JavaScript y el script de C# siguientes registran la ruta de acceso al blob de desencadenamiento, incluido el contenedor:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```