---
ms.openlocfilehash: df6137e4e00ebd089ef7fb5efa163a513fb30728
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2021
ms.locfileid: "98061210"
---
A medida que configura los recursos de Azure de este inicio rápido, se copiará un vídeo (corto) de un área de aparcamiento en la máquina virtual Linux en Azure que se usa como dispositivo IoT Edge. Este archivo de vídeo se usará para simular un streaming en vivo para este tutorial.

Abra una aplicación como [VLC Media Player](https://www.videolan.org/vlc/), seleccione Ctrl + N y pegue [este vínculo](https://lvamedia.blob.core.windows.net/public/lots_015.mkv) en el vídeo sobre el área de aparcamiento para iniciar la reproducción. Hacia la marca de 5 segundos, un coche blanco se desplaza por el aparcamiento.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LUbN]

Realice los pasos que se indican a continuación para usar Live Video Analytics en IoT Edge para detectar el movimiento del coche y grabar un clip de vídeo que empiece en torno a la marca de los 5 segundos.
