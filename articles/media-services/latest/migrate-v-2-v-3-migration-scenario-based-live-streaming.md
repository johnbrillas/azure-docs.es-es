---
title: Guía de migración de streaming en vivo de Media Services
description: En este artículo se proporcionan instrucciones basadas en escenarios de streaming en vivo que le ayudarán a migrar de Azure Media Services v2 a v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 6a2c6495ca3685aec1bc132ec7f8a88809ad2d87
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598293"
---
# <a name="live-streaming-scenario-based-migration-guidance"></a>Guía de migración basada en escenarios de streaming en vivo

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-4.svg)

Azure Portal ahora es compatible con la configuración y administración de eventos en directo.  Se recomienda probarlo mientras realiza pruebas para la migración de v2 a v3.

## <a name="test-the-v3-live-event-workflow"></a>Prueba del flujo de trabajo de eventos en directo de v3

> [!NOTE]
> Los canales y programas creados con v2 (que se asignan a eventos en directo y salidas en directo en v3) se pueden administrar con la API v3. Las instrucciones consisten en cambiar a eventos en directo de v3 y salidas en directo en un momento adecuado en el que pueda detener el canal v2 existente. Actualmente no es posible realizar la migración ininterrumpida de un canal en vivo en ejecución continua a los nuevos eventos en directo de v3. Por lo tanto, el tiempo de inactividad de mantenimiento debe coordinarse en el mejor momento posible para su público y su negocio.

Pruebe la nueva forma de entregar eventos en directo con Media Services antes de migrar el contenido de v2 a v3. Estas son las características de v3 con las que se puede trabajar y que debe tener en cuenta para la migración.

- Cree un nuevo [evento en directo](live-events-outputs-concept.md#live-events) de v3 para la codificación. Puede habilitar los [valores preestablecidos de codificación 1080P y 720P](live-event-types-comparison.md#system-presets).
- Use la entidad [salida en directo](live-events-outputs-concept.md#live-outputs) en lugar de programas.
- Cree [localizadores de streaming](streaming-locators-concept.md).
- Determine si necesita streaming en vivo [HLS y DASH](dynamic-packaging-overview.md).
- Si necesita del inicio rápido de eventos en directo, explore las nuevas características de [modo de espera](live-events-outputs-concept.md#standby-mode).
- Si quiere transcribir su evento en directo mientras sucede, explore la nueva característica [transcripción en directo](live-transcription.md).
- Cree eventos en directo ininterrumpidos en v3 si necesita un streaming de mayor duración.
- Use [Event Grid](monitoring/monitor-events-portal-how-to.md) para supervisar los eventos en directo.

Consulte los conceptos, tutoriales y guías de procedimientos de eventos en directo a continuación para conocer los pasos específicos.

## <a name="live-events-concepts-tutorials-and-how-to-guides"></a>Conceptos, tutoriales y guías de procedimientos de eventos en directo

### <a name="concepts"></a>Conceptos

- [Streaming en vivo con Azure Media Services v3](live-streaming-overview.md)
- [Eventos en directo y salidas activas en Media Services](live-events-outputs-concept.md)
- [Codificadores de streaming en vivo locales comprobados](recommended-on-premises-live-encoders.md)
- [Uso de cambio de tiempo y salidas activas para crear la reproducción de vídeo a petición](live-event-cloud-dvr.md)
- [Transcripción en directo (versión preliminar)](live-transcription.md)
- [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md)
- [Estados y facturación de eventos en directo](live-event-states-billing.md)
- [Configuración de latencia baja en eventos en directo](live-event-latency.md)
- [Códigos de error de eventos en directo de Media Services](live-event-error-codes.md)

### <a name="tutorials-and-quickstarts"></a>Tutoriales y guías de inicio rápido

- [Tutorial: Streaming en vivo con Media Services](stream-live-tutorial-with-api.md)
- [Creación de un streaming en vivo de Azure Media Services con OBS](live-events-obs-quickstart.md)
- [Inicio rápido: Carga, codificación y streaming de contenido con el portal](manage-assets-quickstart.md)
- [Inicio rápido: Creación de un streaming en vivo de Azure Media Services con Wirecast](live-events-wirecast-quickstart.md)

## <a name="samples"></a>Ejemplos

También puede [comparar el código de la versión v2 y v3 en los ejemplos de código](migrate-v-2-v-3-migration-samples.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
