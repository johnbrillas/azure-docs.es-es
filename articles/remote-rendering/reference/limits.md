---
title: Limitaciones
description: Limitaciones de código para las características del SDK
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 68c0c04feba2779598a500c84b2ba4a9086b104d
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593962"
---
# <a name="limitations"></a>Limitaciones

Algunas características tienen limitaciones de tamaño, recuento u otras.

## <a name="azure-frontend"></a>Azure Frontend

Las siguientes limitaciones se aplican a la API de front-end (C++ y C#):
* Instancias totales de [RemoteRenderingClient](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient) por proceso: 16.
* Instancias totales de [RenderingSession](/dotnet/api/microsoft.azure.remoterendering.renderingsession) por [RemoteRenderingClient](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient): 16.

## <a name="objects"></a>Objetos

* Número total de objetos permitidos de un único tipo ([Entity](../concepts/entities.md), [CutPlaneComponent](../overview/features/cut-planes.md), etc.): 16 777 215.
* Número total de planos de corte activo permitidos: 8.

## <a name="geometry"></a>Geometría

* **Animación:** las animaciones se limitan a animar transformaciones individuales de [objetos de juego](../concepts/entities.md). No se admiten las animaciones esqueléticas con animaciones de piel y vértices. Las pistas de animación del archivo de recursos de origen no se conservan. En su lugar, el código cliente debe controlar las animaciones de transformación de objetos.
* **Sombreadores personalizados:** no se admite la creación de sombreadores personalizados. Solo se pueden usar [materiales de color](../overview/features/color-materials.md) o [materiales PBR](../overview/features/pbr-materials.md) integrados.
* **Número máximo de materiales distintos** en un recurso: 65 535. Para más información sobre la reducción automática del número de materiales, consulte el capítulo sobre la [desduplicación de materiales](../how-tos/conversion/configure-model-conversion.md#material-de-duplication).
* **Dimensión máxima de una sola textura**: 16 384 x 16 384. El proceso de conversión reducirá el tamaño de las texturas de origen más grandes.

### <a name="overall-number-of-polygons"></a>Número total de polígonos

El número de polígonos permitidos para todos los modelos cargados depende del tamaño de la máquina virtual que se pasa a [la API REST de administración de sesión](../how-tos/session-rest-api.md#create-a-session):

| Tamaño del servidor | Número máximo de polígonos |
|:--------|:------------------|
|estándar| 20 millones |
|prémium| ilimitado |

Para más información sobre esta limitación, consulte el capítulo sobre el [tamaño del servidor](../reference/vm-sizes.md).

## <a name="platform-limitations"></a>Limitaciones de la plataforma

**Escritorio de Windows 10**

* Win32/x64 es la única plataforma Win32 compatible. Win32/x86 no es compatible.

**HoloLens 2**

* No se admite la característica [representación desde la cámara PV](/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in).