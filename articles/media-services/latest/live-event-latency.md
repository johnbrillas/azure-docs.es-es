---
title: Configuración de latencia baja de LiveEvent en Azure Media Services
description: En este tema se proporciona información general sobre la configuración de latencia baja de LiveEvent y se muestra cómo establecer una latencia baja.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: f9f4476e932df9a5f4c093968b2e7c4840e7ff39
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102122908"
---
# <a name="live-event-low-latency-settings"></a>Configuración de latencia baja en eventos en directo

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En este artículo se muestra cómo establecer una latencia baja en un objeto [LiveEvent](/rest/api/media/liveevents). También se describen los resultados típicos que se ven cuando se usa la configuración de latencia baja en varios reproductores. Los resultados varían en función de la latencia de red y CDN.

Para usar la nueva característica **LowLatency**, puede establecer **StreamOptionsFlag** en **LowLatency** en **LiveEvent**. Al crear [LiveOutput](/rest/api/media/liveoutputs) para la reproducción de HLS, establezca [LiveOutput.Hls.fragmentsPerTsSegment](/rest/api/media/liveoutputs/create#hls) en 1. Una vez que la transmisión esté en funcionamiento, puede usar la página de demostración de [Azure Media Player](https://ampdemo.azureedge.net/) (AMP) y establecer las opciones de reproducción para usar "Low Latency Heuristics Profile" (Perfil de heurística de baja latencia).

> [!NOTE]
> Actualmente, LowLatency HeuristicProfile en Azure Media Player está diseñado para reproducir secuencias en protocolo MPEG-DASH con formato CSF o CMAF (por ejemplo, `format=mdp-time-csf` o `format=mdp-time-cmaf`). 

El siguiente ejemplo de .NET muestra cómo establecer **LowLatency** en **LiveEvent**:

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#NewLiveEvent)]

Consulte el ejemplo completo: [LiveEventWithDVR](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/Live/LiveEventWithDVR/Program.cs).

## <a name="live-events-latency"></a>Latencia de objetos LiveEvent

En la tabla siguiente se muestran los resultados típicos de latencia (cuando está habilitada la marca LowLatency) en Media Services, medida desde el momento en que la fuente de contribución alcanza el servicio hasta cuando un usuario visualiza la reproducción en el reproductor. Para usar una latencia baja de forma óptima, debe ajustar la configuración del codificador a la longitud de grupo de imágenes (GOP) de 1 segundo. Cuando se usa una longitud GOP mayor, se minimiza el consumo de ancho de banda y se reduce la velocidad de bits en la misma velocidad de fotogramas. Esto resulta especialmente útil en vídeos con menos movimiento.

### <a name="pass-through"></a>Paso a través 

||Baja latencia de GOP de 2 s habilitada|Baja latencia de GOP de 1 s habilitada|
|---|---|---|
|**DASH en AMP**|10 s|8 s|
|**HLS en el reproductor de iOS nativo**|14 s|10 s|

### <a name="live-encoding"></a>Live Encoding

||Baja latencia de GOP de 2 s habilitada|Baja latencia de GOP de 1 s habilitada|
|---|---|---|
|**DASH en AMP**|14 s|10 s|
|**HLS en el reproductor de iOS nativo**|18 s|13 s|

> [!NOTE]
> Latencia de extremo a extremo puede variar según las condiciones de la red local o al introducir una capa de almacenamiento en caché de CDN. Debe probar las configuraciones exactas.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al streaming en vivo](live-streaming-overview.md)
- [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)
