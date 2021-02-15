---
title: 'Azure Communication Services: problemas conocidos'
description: Más información sobre los problemas conocidos de Azure Communication Services
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 10/03/2020
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: e9e4b747d9d0ab39a1d0ecef6cf45e4cc0f9e2c5
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628120"
---
# <a name="known-issues-azure-communication-services"></a>Problemas conocidos: Azure Communication Services

En este artículo se proporciona información sobre los problemas conocidos asociados a Azure Communication Services.

## <a name="video-streaming-quality-on-chromeandroid"></a>Calidad del streaming de vídeo en Chrome para Android 

El rendimiento del streaming de vídeo se puede degradar en Chrome para Android.

### <a name="possible-causes"></a>Causas posibles
La calidad de las secuencias remotas depende de la resolución del representador del lado cliente que se haya usado para iniciar esa secuencia. Al suscribirse a una secuencia remota, un receptor determinará su propia resolución en función de las dimensiones del representador del lado cliente del remitente.

## <a name="bluetooth-headset-microphones-are-not-detected"></a>No se detectan auriculares con micrófono Bluetooth

Al conectar los auriculares Bluetooth a una llamada de Communication Services, puede experimentar problemas.

### <a name="possible-causes"></a>Causas posibles
No hay ninguna opción para seleccionar un micrófono Bluetooth en iOS.


## <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>El cambio repetido de dispositivos de los dispositivos de vídeo puede provocar la detención temporal del streaming de vídeo

El cambio entre dispositivos de vídeo puede hacer que la secuencia de vídeo se pause mientras se adquiere la secuencia desde el dispositivo seleccionado.

### <a name="possible-causes"></a>Causas posibles
El streaming desde los dispositivos multimedia y el cambio entre estos consume muchos recursos informáticos. Los cambios frecuentes pueden provocar una degradación del rendimiento. Se recomienda a los desarrolladores detener una secuencia del dispositivo antes de iniciar otra.
