---
ms.openlocfilehash: e8d8300d2be7a2a48cb48a51b0f81c96f68c45aa
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750303"
---
1. Clone el repositorio desde esta ubicación: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. En Visual Studio Code, abra la carpeta en que se ha descargado.
1. En Visual Studio Code, vaya a la carpeta *src/cloud-to-device-console-app/operations.json*. Allí, cree un archivo y asígnele el nombre *appsettings.json*. Este archivo contendrá la configuración necesaria para ejecutar el programa.
1. Copie el contenido del archivo *~/clouddrive/lva-sample/appsettings.json* que generó anteriormente en este inicio rápido.

    El texto debería ser similar al siguiente.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Vaya a la carpeta *src/edge* y cree un archivo llamado *.env*.
1. Copie el contenido del archivo */clouddrive/lva-sample/edge-deployment/.env*. El texto debería ser similar al siguiente código.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    VIDEO_INPUT_FOLDER_ON_DEVICE="/home/lvaedgeuser/samples/input"  
    VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    