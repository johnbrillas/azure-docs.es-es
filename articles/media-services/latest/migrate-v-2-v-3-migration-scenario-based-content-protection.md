---
title: Guía de migración de protección de contenido
description: En este artículo se proporcionan instrucciones basadas en escenarios de protección de contenido que le ayudarán a migrar de Azure Media Services v2 a v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 9c0040c0ad34b019eaa90bbd1571377ad3539578
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98940109"
---
# <a name="content-protection-scenario-based-migration-guidance"></a>Guía de migración basada en escenarios de protección de contenido

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-4.svg)

En este artículo se proporcionan instrucciones basadas en escenarios de protección de contenido que le ayudarán a migrar de Azure Media Services v2 a v3.

## <a name="protect-content-in-v3-api"></a>Protección del contenido en API v3

Use la compatibilidad con las características de [varias claves](design-multi-drm-system-with-access-control.md) en la nueva API v3.

Consulte los conceptos, tutoriales y guías de procedimientos de protección de contenido a continuación para conocer los pasos específicos.

## <a name="content-protection-concepts-tutorials-and-how-to-guides"></a>Conceptos, tutoriales y guías de procedimientos de protección de contenido

### <a name="concepts"></a>Conceptos

- [Protección del contenido mediante el cifrado dinámico de Media Services](content-protection-overview.md)
- [Diseño del sistema de protección de contenido con DRM múltiple con control de acceso](design-multi-drm-system-with-access-control.md)
- [Media Services v3 con plantilla de licencia de PlayReady](playready-license-template-overview.md)
- [Introducción a Media Services v3 con plantilla de licencia de Widevine](widevine-license-template-overview.md)
- [Configuración y requisitos de licencia de FairPlay de Apple](fairplay-license-overview.md)
- [Directivas de streaming](streaming-policy-concept.md)
- [Directivas de claves de contenido](content-key-policy-concept.md)

### <a name="tutorials"></a>Tutoriales

[Inicio rápido: Uso del portal para cifrar contenido](encrypt-content-quickstart.md)

### <a name="how-to-guides"></a>Guías de procedimientos

- [Obtención de una clave de firma de la directiva existente](get-content-key-policy-dotnet-howto.md)
- [Streaming de FairPlay sin conexión para iOS con Media Services v3](offline-fairplay-for-ios.md)
- [Streaming de Widevine sin conexión para Android con Media Services v3](offline-widevine-for-android.md)
- [Streaming de PlayReady sin conexión para Windows 10 con Media Services v3](offline-plaready-streaming-for-windows-10.md)

## <a name="samples"></a>Ejemplos

También puede [comparar el código de la versión v2 y v3 en los ejemplos de código](migrate-v-2-v-3-migration-samples.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]