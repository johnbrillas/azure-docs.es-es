---
title: Sistema de reescritura de direcciones URL de Azure Media Player
description: Azure Media Player reescribirá una dirección URL determinada de Azure Media Services para proporcionar secuencias para SMOOTH, DASH, HLS v3 y HLS v4.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.custom: devx-track-js
ms.openlocfilehash: b037eec13fda0b5ec16a4f2f53ad2a64fb5f8da1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91275564"
---
# <a name="url-rewriter"></a>Sistema de reescritura de URL #

De manera predeterminada, Azure Media Player reescribirá una dirección URL determinada de Azure Media Services para proporcionar secuencias para SMOOTH, DASH, HLS v3 y HLS v4. Por ejemplo, si el origen se proporciona como se muestra a continuación, Azure Media Player se asegurará de que intenta reproducir todos los protocolos anteriores:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" />
    </video>
```

Sin embargo, si desea no usar el sistema de reescritura de URL, puede hacerlo agregando la propiedad `disableUrlRewriter` al parámetro. Esto significa que toda la información que se pasa a los orígenes se pasa directamente al reproductor sin modificación alguna.  Este es un ejemplo de cómo agregar dos orígenes al reproductor, uno DASH y otro SMOOTH streaming.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)" type="application/dash+xml" data-setup='{"disableUrlRewriter": true}'/>
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"disableUrlRewriter": true}'/>
    </video>
```

or

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)", type: "application/dash+xml", disableUrlRewriter: true },
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", disableUrlRewriter: true }
    ]);
```

Además, si lo desea, puede especificar los formatos de streaming específicos que desea que Azure Media Player reescriba mediante el parámetro `streamingFormats`. Entre las opciones se incluyen `DASH`, `SMOOTH`, `HLSv3`, `HLSv4` y `HLS`. La diferencia entre HLS y HLSv3 y v4 es que el formato HLS admite la reproducción de contenido de FairPlay. Ni la versión 3 ni la versión 4 admiten FairPlay. Esto resulta útil si no tiene disponible una directiva de entrega para un protocolo determinado.  Este es un ejemplo de un momento en que un protocolo DASH no está habilitado con su recurso.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"streamingFormats": ["SMOOTH", "HLS","HLS-V3", "HLS-V4"] }'/>
    </video>
```

or

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", streamingFormats: ["SMOOTH", "HLS","HLS-V3", "HLS-V4"]},
    ]);
```

Los dos anteriores se pueden combinar entre sí para varias circunstancias en función de su recurso concreto.

> [!NOTE]
> La información de protección de Widevine solo se conserva en el protocolo DASH.

## <a name="next-steps"></a>Pasos siguientes ##

- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)