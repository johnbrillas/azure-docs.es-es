---
title: 'Tutorial: Filtrado y análisis de los datos con proceso en Azure Stack Edge Pro con GPU | Microsoft Docs'
description: Aprenda a configurar el rol de proceso en Azure Stack Edge Pro con GPU y a usarlo para transformar los datos antes de enviarlos a Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/07/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge Pro so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: eb71db05a61a0e32f3f092f37a4da72bc04e581d
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99525761"
---
# <a name="tutorial-configure-compute-on-azure-stack-edge-pro-gpu-device"></a>Tutorial: Configuración del proceso en un dispositivo de Azure Stack Edge Pro con GPU

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Este tutorial describe cómo configurar el rol de proceso y crear un clúster de Kubernetes en un dispositivo de Azure Stack Edge Pro. 

Este procedimiento tarda aproximadamente entre 20 y 30 minutos en completarse.


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar el proceso
> * Obtener puntos de conexión de Kubernetes

 
## <a name="prerequisites"></a>Requisitos previos

Antes de configurar un rol de proceso en el dispositivo de Azure Stack Edge Pro, asegúrese de que:

- Ha activado un dispositivo Azure Stack Edge Pro como se describe en [Activación de Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-activate.md).
- Asegúrese de que ha seguido las instrucciones de [Habilitación de la red de proceso](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md#enable-compute-network) y:
    - Ha habilitado una interfaz de red para proceso.
    - Ha asignado direcciones IP de nodo de Kubernetes y direcciones IP de servicio externo de Kubernetes.

## <a name="configure-compute"></a>Configurar el proceso

[!INCLUDE [configure-compute](../../includes/azure-stack-edge-gateway-configure-compute.md)]

## <a name="get-kubernetes-endpoints"></a>Obtención de los puntos de conexión de Kubernetes

Para configurar un cliente para obtener acceso al clúster de Kubernetes, necesitará el punto de conexión de Kubernetes. Siga estos pasos para obtener el punto de conexión de la API de Kubernetes desde la interfaz de usuario local del dispositivo de Azure Stack Edge Pro.

1. En la interfaz de usuario web local del dispositivo, vaya a la página **Dispositivos**.
2. En **Puntos de conexión del dispositivo**, copie el punto de conexión del **servicio de API de Kubernetes**. Este punto de conexión es una cadena en el formato siguiente: `https://compute.<device-name>.<DNS-domain>[Kubernetes-cluster-IP-address]`. 

    ![Página del dispositivo en la interfaz de usuario local](./media/azure-stack-edge-j-series-create-kubernetes-cluster/device-kubernetes-endpoint-1.png)

3. Guarde la cadena de punto de conexión. Usará esta cadena de punto de conexión más adelante al configurar un cliente para acceder al clúster de Kubernetes mediante kubectl.

4. Mientras está en la interfaz de usuario web local, puede:

    - Ir a la API de Kubernetes, seleccionar **Advanced settings** (Configuración avanzada) y descargar un archivo de configuración avanzada para Kubernetes. 

        ![Página del dispositivo de la interfaz de usuario local 1](./media/azure-stack-edge-gpu-deploy-configure-compute/download-advanced-config-1.png)

        Si se le ha proporcionado una clave de Microsoft (seleccione los usuarios que pueden tener una clave), puede usar este archivo de configuración.

        ![Página del dispositivo de la interfaz de usuario local 2](./media/azure-stack-edge-gpu-deploy-configure-compute/download-advanced-config-2.png)

    - También puede ir al punto de conexión del **panel de Kubernetes** y descargar un archivo de configuración de `aseuser`. 
    
        ![Página del dispositivo de la interfaz de usuario local 3](./media/azure-stack-edge-gpu-deploy-configure-compute/download-aseuser-config-1.png)

        Puede usar este archivo de configuración para iniciar sesión en el panel de Kubernetes o para depurar cualquier problema en el clúster de Kubernetes. Para más información, consulte [Acceso al panel de Kubernetes](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#access-dashboard). 


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Configurar el proceso
> * Obtención de los puntos de conexión de Kubernetes


Para aprender a administrar el dispositivo de Azure Stack Edge Pro, consulte:

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar Azure Stack Edge Pro](azure-stack-edge-manage-access-power-connectivity-mode.md).
