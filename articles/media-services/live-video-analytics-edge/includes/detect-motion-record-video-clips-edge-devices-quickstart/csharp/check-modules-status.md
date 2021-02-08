---
ms.openlocfilehash: 53052097fa6616f889b710c58488a9f7a616168d
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98956359"
---
En el paso [Generación e implementación del manifiesto de implementación de IoT Edge](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest), en Visual Studio Code, expanda el nodo **Iva-sample-device** en **AZURE IOT HUB** (en la sección situada en la parte inferior izquierda). Debería ver los siguientes módulos implementados:

* El módulo de Live Video Analytics, denominado `lvaEdge`.
* El módulo `rtspsim`, que simula un servidor RTSP, que actúa como el origen de una fuente de vídeo en directo.

  ![Módulos](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> En los pasos anteriores se supone que usa la máquina virtual creada mediante el script de configuración. Si usa su propio dispositivo perimetral, acceda a él y ejecute los siguientes comandos con **derechos de administrador** a fin de extraer y almacenar el archivo de vídeo de ejemplo que se usa en este inicio rápido:  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
