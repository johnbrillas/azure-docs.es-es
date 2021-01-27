---
title: Inicio rápido de Azure Media Player
description: Conozca los pasos básicos para configurar Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: quickstart
ms.date: 04/20/2020
ms.openlocfilehash: 1634731381be6ebbb8750746f88b8ff9e761025d
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879197"
---
# <a name="azure-media-player-quickstart"></a>Inicio rápido de Azure Media Player
Azure Media Player es fácil de configurar. Solo se tarda unos minutos en obtener la reproducción básica de contenido multimedia desde la cuenta de Azure Media Services. En esta sección se muestran los pasos básicos sin entrar en detalles. En las secciones siguientes se proporcionan detalles sobre cómo configurar Azure Media Player.  Simplemente agregue los siguientes include en la sección `<head>` de su documento:

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> **NO** use la versión `latest` en producción, porque está sujeta a cambios a petición. Reemplace `latest` por una versión de Azure Media Player; por ejemplo, reemplace `latest` por `1.0.0`. Las versiones de Azure Media Player pueden consultarse desde [aquí](azure-media-player-changelog.md).

## <a name="use-the-video-element"></a>Uso del elemento de vídeo

A continuación, simplemente use el elemento `<video>` como lo haría normalmente, pero con un atributo `data-setup` adicional que contenga cualquier opción. Estas opciones pueden incluir cualquier opción de Azure Media Services de un objeto JSON válido.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Si no desea utilizar la configuración automática, puede omitir el atributo `data-setup` e inicializar un elemento de vídeo manualmente.

```html
    var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: false,
            controls: true,
            width: "640",
            height: "400",
            poster: ""
        }, function() {
              console.log('Good to go!');
               // add an event listener
              this.addEventListener('ended', function() {
                console.log('Finished!');
            }
          }
    );
    myPlayer.src([{
        src: "http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

## <a name="next-steps"></a>Pasos siguientes ##

- [Configuración completa de Azure Media Player](./azure-media-player-full-setup.md)