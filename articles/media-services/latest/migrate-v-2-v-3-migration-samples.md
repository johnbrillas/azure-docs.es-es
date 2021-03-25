---
title: Comparación de ejemplos de migración de Media Services v2 a v3
description: Un conjunto de ejemplos que le ayudarán a comparar las diferencias de código entre Azure Media Services v2 y v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2021
ms.author: inhenkel
ms.openlocfilehash: 640b9b40295ae9b9aea865f7b6159da6ff4a3251
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98898314"
---
# <a name="media-services-migration-code-sample-comparison"></a>Comparación de ejemplos de código de la migración de Media Services

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

Puede usar algunos de nuestros ejemplos de código para comparar el funcionamiento de los SDK.

## <a name="samples-for-comparison"></a>Ejemplos para comparación

En la tabla siguiente se muestra una lista de ejemplos para comparar entre v2 y v3 en escenarios comunes.

|Escenario|API v2|API v3|
|---|---|---|
|Crear un recurso y cargar un archivo |[Ejemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[Ejemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Enviar un trabajo|[Ejemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[Ejemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Se explica cómo crear una transformación y después enviar un trabajo.|
|Publicar un recurso con cifrado AES |1. Cree `ContentKeyAuthorizationPolicyOption`<br/>2. Cree `ContentKeyAuthorizationPolicy`<br/>3. Cree `AssetDeliveryPolicy`<br/>4. Cree un `Asset` y cargue contenido o envíe `Job` y use `OutputAsset`<br/>5. Asocie `AssetDeliveryPolicy` con `Asset`<br/>6. Cree `ContentKey`<br/>7. Adjunte `ContentKey` a `Asset`<br/>8. Cree `AccessPolicy`<br/>9. Cree `Locator`<br/><br/>[Ejemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. Cree `ContentKeyPolicy`<br/>2. Cree `Asset`<br/>3. Cargue contenido o use `Asset` como `JobOutput`<br/>4. Cree `StreamingLocator`<br/><br/>[Ejemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|Obtener detalles de un trabajo y administrar trabajos |[Administrar trabajos con v2](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[Administrar trabajos con v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
