---
title: Límites de objetos
description: Se explica cómo se pueden consultar los límites de objetos espaciales.
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: df04b767035dffb62fde89d1e74b808d62fcc943
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594491"
---
# <a name="object-bounds"></a>Límites de objetos

Los límites de objetos representan el volumen que ocupa una [entidad](entities.md) y sus elementos secundarios. En Azure Remote Rendering, los límites de objetos siempre se dan como *cuadros de límite alineados por el eje* (AABB). Los objetos enlazados pueden estar en el *espacio local* o en el *espacio universal*. En cualquier caso, siempre están alineados por el eje, lo que significa que las extensiones y el volumen pueden diferir entre la representación del espacio local y el espacio universal.

## <a name="querying-object-bounds"></a>Consulta de los límites de objetos

El rectángulo de selección alineado con el eje local de una [malla](meshes.md) se puede consultar directamente desde el recurso de malla. Estos límites se pueden transformar en el espacio local o en el espacio universal de una entidad mediante la transformación de la entidad.

Es posible calcular los límites de una jerarquía de objetos completa de esta manera, pero para ello es necesario recorrer la jerarquía, consultar los límites de cada malla y combinarlos manualmente. Esta operación es tanto tediosa como ineficaz.

Una manera mejor es llamar a `QueryLocalBoundsAsync` o `QueryWorldBoundsAsync` en una entidad. El cálculo se descarga luego en el servidor y se devuelve con un retardo mínimo.

```cs
public async void GetBounds(Entity entity)
{
    try
    {
        Task<Bounds> boundsQuery = entity.QueryWorldBoundsAsync();
        Bounds result = await boundsQuery;
    
        Double3 aabbMin = result.Min;
        Double3 aabbMax = result.Max;
        // ...
    }
    catch (RRException ex)
    {
    }
}
```

```cpp
void GetBounds(ApiHandle<Entity> entity)
{
    entity->QueryWorldBoundsAsync(
        // completion callback:
        [](Status status, Bounds bounds)
        {
           if (status == Status::OK)
            {
                Double3 aabbMin = bounds.Min;
                Double3 aabbMax = bounds.Max;
                // ...
            }
        }
    );
}
```

## <a name="api-documentation"></a>Documentación de la API

* [C# Entity.QueryLocalBoundsAsync](/dotnet/api/microsoft.azure.remoterendering.entity.querylocalboundsasync)
* [C++ Entity::QueryLocalBoundsAsync](/cpp/api/remote-rendering/entity#querylocalboundsasync)

## <a name="next-steps"></a>Pasos siguientes

* [Consultas espaciales](../overview/features/spatial-queries.md)