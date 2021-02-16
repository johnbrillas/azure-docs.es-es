---
title: Modelos
description: Describe qué es un modelo en Azure Remote Rendering.
author: jakrams
ms.author: jakras
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 0d1e66d09db3e3934871ed15493feb685d1cbe6a
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593881"
---
# <a name="models"></a>Modelos

Un *modelo*, en Azure Remote Rendering, se refiere a una representación de objeto completa, formada por [entidades](entities.md) y [componentes](components.md). Los modelos son la manera principal de obtener datos personalizados en el servicio Remote Rendering.

## <a name="model-structure"></a>Estructura del modelo

Un modelo tiene exactamente una [entidad](entities.md) como nodo raíz. Por debajo, puede tener una jerarquía arbitraria de entidades secundarias. Al cargar un modelo, se devuelve una referencia a esta entidad raíz.

Cada entidad puede tener [componentes](components.md) adjuntos. En el caso más común, las entidades tienen *MeshComponents*, que hacen referencia a los [recursos de malla](meshes.md).

## <a name="creating-models"></a>Creación de modelos

La creación de modelos para el entorno en tiempo de ejecución se logra mediante la [conversión de modelos de entrada](../how-tos/conversion/model-conversion.md) a partir de formatos de archivo como FBX y GLTF. El proceso de conversión extrae todos los recursos, como texturas, materiales y mallas, y los convierte en formatos para entornos en tiempo de ejecución optimizados. También extrae la información estructural y la convierte en la estructura del grafo de entidad/componente de ARR.

> [!IMPORTANT]
> [La conversión de modelos](../how-tos/conversion/model-conversion.md) es la única manera de crear [mallas](meshes.md). Aunque las mallas se pueden compartir entre entidades en tiempo de ejecución, no hay otra manera de obtener una malla en el entorno en tiempo de ejecución, aparte de cargar un modelo.

## <a name="loading-models"></a>Carga de modelos

Una vez que se convierte un modelo, se puede cargar desde Azure Blob Storage en el entorno en tiempo de ejecución.

Hay dos funciones de carga distintas que difieren en la forma en que se trata el recurso en Blob Storage:

* El modelo se puede direccionar directamente mediante parámetros de Blob Storage, en caso de que [Blob Storage esté vinculado a la cuenta](../how-tos/create-an-account.md#link-storage-accounts). En este caso, la función de carga pertinente es `LoadModelAsync` con el parámetro `LoadModelOptions`.
* El modelo se puede direccionar desde su URI de SAS. La función de carga pertinente es `LoadModelFromSasAsync` con el parámetro `LoadModelFromSasOptions`. Use también esta variante al cargar [modelos integrados](../samples/sample-model.md).

Los fragmentos de código siguientes muestran cómo cargar modelos con ambas funciones. Para cargar un modelo usando sus parámetros de almacenamiento de blobs, use código como el siguiente:


```cs
async void LoadModel(RenderingSession session, Entity modelParent, string storageAccount, string containerName, string assetFilePath)
{
    // load a model that will be parented to modelParent
    var modelOptions = new LoadModelOptions(
        storageAccount, // storage account name + '.blob.core.windows.net', e.g., 'mystorageaccount.blob.core.windows.net'
        containerName,  // name of the container in your storage account, e.g., 'mytestcontainer'
        assetFilePath,  // the file path to the asset within the container, e.g., 'path/to/file/myAsset.arrAsset'
        modelParent
    );

    var loadOp = session.Connection.LoadModelAsync(modelOptions, (float progress) =>
    {
        Debug.WriteLine($"Loading: {progress * 100.0f}%");
    });

    await loadOp;
}
```

```cpp
void LoadModel(ApiHandle<RenderingSession> session, ApiHandle<Entity> modelParent, std::string storageAccount, std::string containerName, std::string assetFilePath)
{
    LoadModelOptions modelOptions;
    modelOptions.Parent = modelParent;
    modelOptions.Blob.StorageAccountName = std::move(storageAccount);
    modelOptions.Blob.BlobContainerName = std::move(containerName);
    modelOptions.Blob.AssetPath = std::move(assetFilePath);

    ApiHandle<LoadModelResult> result;
    session->Connection()->LoadModelAsync(modelOptions,
        // completion callback
        [](Status status, ApiHandle<LoadModelResult> result)
        {
            printf("Loading: finished.");
        },
        // progress callback
        [](float progress)
        {
            printf("Loading: %.1f%%", progress * 100.f);
        }
    );
}
```

Si quiere cargar un modelo mediante un token de SAS, use código similar al siguiente fragmento de código:

```cs
async void LoadModel(RenderingSession session, Entity modelParent, string modelUri)
{
    // load a model that will be parented to modelParent
    var modelOptions = new LoadModelFromSasOptions(modelUri, modelParent);

    var loadOp = session.Connection.LoadModelFromSasAsync(modelOptions, (float progress) =>
    {
        Debug.WriteLine($"Loading: {progress * 100.0f}%");
    });

    await loadOp;
}
```

```cpp
void LoadModel(ApiHandle<RenderingSession> session, ApiHandle<Entity> modelParent, std::string modelUri)
{
    LoadModelFromSasOptions modelOptions;
    modelOptions.ModelUri = modelUri;
    modelOptions.Parent = modelParent;

    ApiHandle<LoadModelResult> result;
    session->Connection()->LoadModelFromSasAsync(modelOptions,
        // completion callback
        [](Status status, ApiHandle<LoadModelResult> result)
        {
            printf("Loading: finished.");
        },
        // progress callback
        [](float progress)
        {
            printf("Loading: %.1f%%", progress * 100.f);
        }
    );
}
```

Después, puede atravesar la jerarquía de entidades y modificar las entidades y los componentes. Al cargar el mismo modelo varias veces, se crean varias instancias, cada una con su propia copia de la estructura de entidad/componente. A pesar de que las mallas, los materiales y las texturas son [recursos compartidos](../concepts/lifetime.md), sus datos no se volverán a cargar. Por lo tanto, la creación de instancias de un modelo más de una vez incurre en una sobrecarga de memoria relativamente escasa.

## <a name="api-documentation"></a>Documentación de la API

* [RenderingConnection.LoadModelAsync() de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadmodelasync)
* [RenderingConnection.LoadModelFromSasAsync() de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadmodelfromsasasync)
* [RenderingConnection::LoadModelAsync() de C++](/cpp/api/remote-rendering/renderingconnection#loadmodelasync)
* [RenderingConnection::LoadModelFromSasAsync() de C++](/cpp/api/remote-rendering/renderingconnection#loadmodelfromsasasync)

## <a name="next-steps"></a>Pasos siguientes

* [Entidades](entities.md)
* [Mallas](meshes.md)