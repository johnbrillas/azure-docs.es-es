---
ms.openlocfilehash: c99d2489efe7c46b8d50b08861fcbbcd6f8a1966
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97532147"
---
1. En Visual Studio Code, abra la pestaña **Extensiones** (o presione Ctrl + Mayús + X) y busque Azure IoT Hub.
1. Haga clic con el botón derecho y seleccione la **Configuración de la extensión**.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/extensions-tab.png" alt-text="Configuración de la extensión":::
1. Busque y habilite "Show Verbose Message" (Mostrar mensaje detallado).

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/show-verbose-message.png" alt-text="Show Verbose Message"::: (Mostrar mensaje detallado)
1. Para iniciar una sesión de depuración, seleccione la tecla F5. La ventana **TERMINAL** muestra algunos mensajes.
1. El código de *operations.json* llama a los métodos directos `GraphTopologyList` y `GraphInstanceList`. Si ha limpiado los recursos después de los inicios rápidos anteriores, este proceso devolverá listas vacías y, a continuación, se pausará. Seleccione la tecla Entrar.
    
    ```
    --------------------------------------------------------------------------
    Executing operation GraphTopologyList
    -----------------------  Request: GraphTopologyList  --------------------------------------------------
    {
      "@apiVersion": "2.0"
    }
    ---------------  Response: GraphTopologyList - Status: 200  ---------------
    {
      "value": []
    }
    --------------------------------------------------------------------------
    Executing operation WaitForInput
    Press Enter to continue
    ```
  
  La ventana **TERMINAL** muestra el siguiente conjunto de llamadas al método directo:  
  
  * Una llamada a `GraphTopologySet` que utiliza `topologyUrl`. 
  * Una llamada a `GraphInstanceSet` que usa el cuerpo siguiente:
  
  ```
  {
    "@apiVersion": "2.0",
    "name": "Sample-Graph",
    "properties": {
      "topologyName": "EVRToFilesOnMotionDetection",
      "description": "Sample graph description",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/lots_015.mkv"
        },
        {
          "name": "rtspUserName",
          "value": "testuser"
        },
        {
          "name": "rtspPassword",
          "value": "testpassword"
        }
      ]
    }
  }
  ```
    
  * Una llamada a `GraphInstanceActivate` que inicia la instancia del grafo y el flujo de vídeo.
  * Una segunda llamada a `GraphInstanceList` que muestra que la instancia del grafo está en ejecución.
1. La salida de la ventana **TERMINAL** se pone en pausa en `Press Enter to continue`. No seleccione Entrar todavía. Desplácese hacia arriba para ver las cargas de la respuesta JSON para los métodos directos que ha invocado.
1. Cambie a la ventana **SALIDA** de Visual Studio Code. Verá los mensajes que el módulo Live Video Analytics en IoT Edge envía al centro de IoT. En la siguiente sección de este inicio rápido se analizan estos mensajes.
1. El grafo multimedia continúa ejecutándose e imprimiendo los resultados. El simulador RTSP sigue recorriendo el vídeo de origen. Para detener el grafo multimedia, vuelva a la ventana **TERMINAL** y seleccione Entrar. 

    La siguiente serie de llamadas limpia los recursos:

    * Una llamada a `GraphInstanceDeactivate` desactiva la instancia del grafo.
    * Una llamada a `GraphInstanceDelete` elimina la instancia.
    * Una llamada a `GraphTopologyDelete` elimina la topología.
    * Una llamada final a `GraphTopologyList` muestra que la lista está ahora vacía.
