---
title: Arquitecturas de Media Services
titleSuffix: Azure Media Services
description: En este artículo se describen arquitecturas para Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: inhenkel
ms.openlocfilehash: ff8704feb2997cd89bbd2bf8cb8801818112732f
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879214"
---
# <a name="media-services-architectures"></a>Arquitecturas de Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="live-streaming-digital-media"></a>Medios digitales con streaming en vivo

Una solución de streaming en vivo permite capturar vídeo en tiempo real y difundirlo a los consumidores también en tiempo real, como entrevistas en streaming, conferencias y eventos deportivos en línea. En esta solución, el vídeo se captura mediante una cámara de vídeo y se envía a un punto de conexión de entrada del canal. El canal recibe el flujo de entrada en vivo y lo deja a disposición del streaming a través de un punto de conexión de streaming a un explorador web y la aplicación móvil. El canal también proporciona un punto de conexión de supervisión de vista previa para obtener una vista previa y validar el flujo antes de un procesamiento y entrega ulteriores. El canal también puede registrar y almacenar el contenido ingerido para que se pueda transmitir posteriormente (vídeo bajo demanda).

Esta solución se basa en los servicios administrados de Azure: Media Services y Content Delivery Network. Estos servicios se ejecutan en un entorno de alta disponibilidad, revisado y compatible, lo que le permite centrarse en la solución, en lugar de en el entorno en que se ejecutan.

Consulte [Medios digitales con streaming en vivo](/azure/architecture/solution-ideas/articles/digital-media-live-stream) en el Centro de arquitectura de Azure.

## <a name="video-on-demand-digital-media"></a>Medios digitales con vídeo bajo demanda

Una solución básica de vídeo bajo demanda que ofrece la posibilidad de transmitir contenido de vídeo grabado —como películas, clips de noticias, segmentos deportivos, vídeos de aprendizaje y tutoriales de soporte técnico al cliente— a cualquier dispositivo de punto de conexión, aplicación móvil o explorador de escritorio compatible con vídeo. Los archivos de vídeo se cargan en Azure Blob Storage, se codifican en un formato estándar de múltiples velocidades de bits y, a continuación, se distribuyen a través de los principales protocolos de streaming con velocidad de bits adaptable (HLS, MPEG-DASH, Smooth) al cliente Azure Media Player.

Esta solución se basa en los servicios administrados de Azure: Blob Storage, Content Delivery Network y Azure Media Player. Estos servicios se ejecutan en un entorno de alta disponibilidad, revisado y compatible, lo que le permite centrarse en la solución, en lugar de en el entorno en que se ejecutan.

Consulte [Medios digitales con vídeo bajo demanda](/azure/architecture/solution-ideas/articles/digital-media-video) en el Centro de arquitectura de Azure.

## <a name="gridwich-media-processing-system"></a>Sistema de procesamiento de elementos multimedia de Gridwich

El sistema Gridwich es un ejemplo de los procedimientos recomendados para el procesamiento y la entrega de recursos multimedia en Azure. Aunque es específico para elementos multimedia, la plataforma de creación de eventos y procesamiento de mensajes se puede aplicar a cualquier flujo de trabajo de procesamiento de eventos sin estado.

Consulte [Sistema de procesamiento de elementos multimedia de Gridwich](/azure/architecture/reference-architectures/media-services/gridwich-architecture) en el Centro de arquitectura de Azure.

## <a name="next-steps"></a>Pasos siguientes

> [Introducción a Azure Media Services](media-services-overview.md)