---
title: La API REST de conversión de recursos
description: Describe cómo convertir un recurso a través de la API REST
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: f33e5717cd5556e72d996e7e943867c16805e71b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101705184"
---
# <a name="use-the-model-conversion-rest-api"></a>Uso de la API REST de conversión de modelos

El servicio de [conversión de ](model-conversion.md) se controla mediante una [API REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Esta API se puede usar para crear conversiones, obtener propiedades de conversión y enumerar las conversiones existentes.

## <a name="rest-api-reference"></a>Referencia de la API REST

La documentación referencia de la API REST Remote Rendering se puede encontrar [aquí](/rest/api/mixedreality/2021-01-01preview/remoterendering) y las definiciones de Swagger [aquí](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality).

Proporcionamos un script de PowerShell en el [repositorio de ejemplos de ARR](https://github.com/Azure/azure-remote-rendering) en la carpeta *Scripts* que se denomina *Conversion.ps1* y muestra el uso de nuestro servicio. El script y su configuración se describen aquí: [Scripts de PowerShell de ejemplo](../../samples/powershell-example-scripts.md). También proporcionamos SDK para [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java y Python.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de Azure Blob Storage para la conversión de modelos](blob-storage.md)
- [Conversión de modelos](model-conversion.md)