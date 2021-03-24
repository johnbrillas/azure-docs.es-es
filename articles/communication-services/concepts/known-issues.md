---
title: 'Azure Communication Services: preguntas frecuentes y problemas conocidos'
description: Obtenga información acerca de Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: 2c6ac34d8daf00578cb1d03833a28eb8535708b7
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103493652"
---
# <a name="faq--known-issues"></a>Preguntas frecuentes y problemas conocidos
En este artículo se proporciona información sobre problemas conocidos y preguntas frecuentes en relación con Azure Communication Services.

## <a name="faq"></a>Preguntas más frecuentes

### <a name="why-is-the-quality-of-my-video-degraded"></a>¿Por qué se ha degradado la calidad del vídeo?

La calidad de las secuencias de vídeo depende del tamaño del representador del lado cliente que se haya usado para iniciar esa secuencia. Al suscribirse a una secuencia remota, un receptor determinará su propia resolución en función de las dimensiones del representador del lado cliente del remitente.

### <a name="why-is-it-not-possible-to-enumerateselect-micspeaker-devices-on-safari"></a>¿Por qué no es posible enumerar o seleccionar dispositivos micrófonos o altavoces en Safari?

Las aplicaciones no pueden enumerar ni seleccionar micrófonos ni altavoces (por ejemplo, Bluetooth) MIC/SPEAKER (como Bluetooth) en Safari en iOS o iPad. Se trata de una limitación del sistema operativo: siempre hay un solo dispositivo.

En el caso de Safari en macOS, la aplicación no puede enumerar ni seleccionar un altavoz mediante el administrador de dispositivos de Communication Services; debe seleccionarse mediante el sistema operativo. Si usa Chrome en macOS, la aplicación sí puede enumerar y seleccionar dispositivos mediante el administrador de dispositivos de Communication Services.

## <a name="known-issues"></a>Problemas conocidos

En esta sección, se proporciona información sobre los problemas conocidos asociados a Azure Communication Services.

### <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>El cambio repetido de dispositivos de los dispositivos de vídeo puede provocar la detención temporal del streaming de vídeo

El cambio entre dispositivos de vídeo puede hacer que la secuencia de vídeo se pause mientras se adquiere la secuencia desde el dispositivo seleccionado.

#### <a name="possible-causes"></a>Causas posibles
El streaming desde los dispositivos multimedia y el cambio entre estos consume muchos recursos informáticos. Los cambios frecuentes pueden provocar una degradación del rendimiento. Se recomienda a los desarrolladores detener una secuencia del dispositivo antes de iniciar otra.
