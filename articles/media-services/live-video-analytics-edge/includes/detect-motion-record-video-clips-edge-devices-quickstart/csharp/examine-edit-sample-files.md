---
ms.openlocfilehash: 426c735dfd0d015cdc1a734edde9d336fb88cfbc
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97486880"
---
Como parte de los requisitos previos de este inicio rápido, ha descargado el código de ejemplo en una carpeta. Siga estos pasos para examinar y editar el código de ejemplo.

1. En Visual Studio Code, vaya a *src/edge*. Puede ver el archivo *.env* y algunos archivos de plantilla de implementación.

    La plantilla de implementación hace referencia al manifiesto de implementación del dispositivo perimetral, en el que las variables se utilizan para algunas propiedades. El archivo *.env* incluye los valores de esas variables.
1. Vaya a la carpeta *src/cloud-to-device-console-app*. Aquí verá el archivo *appsettings.json* y algunos otros archivos:
    * ***c2d-console-app.csproj** _: archivo del proyecto de Visual Studio Code.
    _ ***operations.json**: la lista de operaciones que desea que ejecute el programa.
    _ ***Program.cs** _: código del programa de ejemplo. Este código:

        _ Carga la configuración de la aplicación.
        * Invoca los métodos directos que expone el módulo Live Video Analytics en IoT Edge. Puede usar el módulo para analizar secuencias de vídeo en directo mediante la invocación de sus [métodos directos](../../../direct-methods.md). 
        * Se pone en pausa para que pueda examinar la salida del programa en la ventana **TERMINAL** y los eventos generados por el módulo en la ventana **SALIDA**.
        * Invoca los métodos directos para limpiar los recursos.

1. Edite el archivo *operations.json*:
    * Cambie el vínculo a la topología del grafo:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/2.0/topology.json"`
    * En `GraphInstanceSet`, edite el nombre de la topología del grafo para que coincida con el valor del vínculo anterior:
    
      `"topologyName" : "EVRToFilesOnMotionDetection"`
    * Edite la dirección URL de RTSP para que apunte al archivo de vídeo:

        `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`
    * En `GraphTopologyDelete`, edite el nombre:

        `"name": "EVRToFilesOnMotionDetection"`
