---
ms.openlocfilehash: 882ba60e16f770651a1d9fe3b02b61be2b4c34c8
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99530904"
---
Va a utilizar el módulo de Live Video Analytics on IoT Edge para detectar el movimiento en la secuencia de vídeo en directo entrante y enviar eventos a IoT Hub. Para ver estos eventos, siga estos pasos:

1. Abra el panel del explorador en Visual Studio Code y busque Azure IoT Hub en la esquina inferior izquierda.
1. Expanda el nodo **Devices** (Dispositivos).
1. Haga clic con el botón derecho en **lva-sample-device** y seleccione **Iniciar la supervisión del punto de conexión de eventos integrado**.

    ![Inicio de la supervisión de un punto de conexión de eventos integrado](../../../media/quickstarts/start-monitoring-iothub-events.png)

> [!NOTE]
> Es posible que se le pida que proporcione información del punto de conexión integrado del centro de IoT. Para obtener esa información, en Azure Portal, vaya a su centro de IoT y busque la opción **Puntos de conexión integrados** en el panel de navegación izquierdo. Haga clic ahí y busque el **punto de conexión compatible con el centro de eventos** en la sección **Punto de conexión compatible con el centro de eventos**. Copie y use el texto del cuadro. El punto de conexión será similar a este:  
    ```
    Endpoint=sb://iothub-ns-xxx.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>
    ```
