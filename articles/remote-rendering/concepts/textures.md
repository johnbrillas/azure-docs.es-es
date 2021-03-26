---
title: Texturas
description: Flujo de trabajo de recursos de textura
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: e01ddf0690f11d41021e0a5ae5958c7c80646743
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99594424"
---
# <a name="textures"></a>Texturas

Las texturas son un recurso compartido [inmutable](../concepts/lifetime.md). Las texturas se pueden cargar desde [Blob Storage](../how-tos/conversion/blob-storage.md) y se pueden aplicar directamente a los modelos, como se muestra en el [Tutorial: Cambio del entorno y los materiales](../tutorials/unity/materials-lighting-effects/materials-lighting-effects.md). Sin embargo, normalmente, las texturas formarán parte de un [modelo convertido](../how-tos/conversion/model-conversion.md), donde se hace referencia a ellas por sus [materiales](materials.md).

## <a name="texture-types"></a>Tipos de textura

Los diferentes tipos de textura tienen casos de uso diferentes:

* Las **texturas 2D** se usan principalmente en [materiales](materials.md).
* Los **mapas de cubos** se pueden usar para el [cielo](../overview/features/sky.md).

## <a name="supported-texture-formats"></a>Formatos de textura admitidos

Todas las texturas dadas a ARR deben estar en [formato DDS](https://en.wikipedia.org/wiki/DirectDraw_Surface). Preferiblemente con los mapas MIP y compresión de textura.

## <a name="loading-textures"></a>Carga de texturas

Al cargar una textura, tiene que especificar el tipo esperado. Si el tipo no coincide, se produce un error en la carga de la textura.
La carga de una textura con el mismo URI dos veces devolverá el mismo objeto de textura, ya que se trata de un [recurso compartido](../concepts/lifetime.md).

De forma similar a la carga de modelos, hay dos variantes a la hora de direccionar un recurso de textura en el almacenamiento de blobs de origen:

* La textura se puede direccionar directamente mediante parámetros de Blob Storage, en caso de que [Blob Storage esté vinculado a la cuenta](../how-tos/create-an-account.md#link-storage-accounts). En este caso, la función de carga pertinente es `LoadTextureAsync` con el parámetro `LoadTextureOptions`.
* El recurso de textura se puede direccionar por su URI de SAS. La función de carga pertinente es `LoadTextureFromSasAsync` con el parámetro `LoadTextureFromSasOptions`. Use también esta variante al cargar [texturas integradas](../overview/features/sky.md#built-in-environment-maps).

En el código de ejemplo siguiente se muestra cómo cargar una textura:

```cs
async void LoadMyTexture(RenderingSession session, string storageContainer, string blobName, string assetPath)
{
    try
    {
        LoadTextureOptions options = new LoadTextureOptions(storageContainer, blobName, assetPath, TextureType.Texture2D);
        Texture texture = await session.Connection.LoadTextureAsync(options);
    
        // use texture...
    }
    catch (RRException ex)
    {
    }
}
```

```cpp
void LoadMyTexture(ApiHandle<RenderingSession> session, std::string storageContainer, std::string blobName, std::string assetPath)
{
    LoadTextureOptions params;
    params.TextureType = TextureType::Texture2D;
    params.Blob.StorageAccountName = std::move(storageContainer);
    params.Blob.BlobContainerName = std::move(blobName);
    params.Blob.AssetPath = std::move(assetPath);
    session->Connection()->LoadTextureAsync(params, [](Status status, ApiHandle<Texture> texture)
    {
        // use texture...
    });
}
```

Tenga en cuenta que, en el caso de usar su variante de SAS, la única diferencia es la función o el parámetro de carga.

Dependiendo del tipo de textura que se supone que se va a usar, puede haber restricciones en cuanto al tipo y el contenido. Por ejemplo, el mapa de rugosidad de un [material PBR](../overview/features/pbr-materials.md) debe ser una escala de grises.

## <a name="api-documentation"></a>Documentación de la API

* [Clase Texture de C#](/dotnet/api/microsoft.azure.remoterendering.texture)
* [RenderingConnection.LoadTextureAsync() de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadtextureasync)
* [RenderingConnection.LoadTextureFromSasAsync() de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadtexturefromsasasync)
* [Clase Texture de C++](/cpp/api/remote-rendering/texture)
* [RenderingConnection::LoadTextureAsync() de C++](/cpp/api/remote-rendering/renderingconnection#loadtextureasync)
* [RenderingConnection::LoadTextureFromSasAsync() de C++](/cpp/api/remote-rendering/renderingconnection#loadtexturefromsasasync)

## <a name="next-steps"></a>Pasos siguientes

* [Materiales](materials.md)
* [Cielo](../overview/features/sky.md)