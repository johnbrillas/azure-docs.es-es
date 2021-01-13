---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: f2443765ecc9116193cefbc729ced25fa5657e59
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/24/2020
ms.locfileid: "97763442"
---
Para poder implementar VM en el dispositivo Azure Stack Edge, debe configurar el cliente para que se conecte al dispositivo mediante Azure Resource Manager con Azure PowerShell. Para consultar los pasos detallados, vaya a [Conectar con Azure Resource Manager desde el dispositivo Azure Stack Edge](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md).


Asegúrese de que se pueden usar los siguientes pasos para acceder al dispositivo desde el cliente (completó esta configuración al conectarse a Azure Resource Manager; ahora solo está comprobando que se realizó correctamente): 

1. Compruebe que la comunicación de Azure Resource Manager funciona. Escriba:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Llame a las API del dispositivo local para autenticarse. Escriba: 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    Proporcione el nombre de usuario *EdgeARMuser* y la contraseña para conectarse mediante Azure Resource Manager.

1. Si configuró **Proceso** para Kubernetes, puede omitir este paso. Continúe para asegurarse de que ha habilitado una interfaz de red para el proceso. En la interfaz de usuario local, vaya a la configuración de **Proceso**. Seleccione la interfaz de red que usará para crear un conmutador virtual. Las VM que cree se adjuntarán a un conmutador virtual conectado a este puerto y a la red asociada. Asegúrese de elegir una red que coincida con la dirección IP que usará para la máquina virtual.  

    ![Habilitar la configuración del proceso 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Habilite el proceso en la interfaz de red. Azure Stack Edge creará y administrará un conmutador virtual correspondiente a esa interfaz de red. No especifique direcciones IP específicas para Kubernetes en este momento. La habilitación del proceso puede tardar varios minutos.

    > [!NOTE]
    > Si va a crear máquinas virtuales de GPU, seleccione una interfaz de red conectada a Internet, ya que ello le permite instalar la extensión de GPU en el dispositivo.


1. Habilitación del rol de máquina virtual desde Azure Portal. En este paso se crea una suscripción única para el dispositivo que se usa para crear máquinas virtuales a través de las API locales del dispositivo. 

    1. Para habilitar el rol de máquina virtual, en Azure Portal, vaya al recurso de Azure Stack Edge correspondiente a su dispositivo de Azure Stack Edge. Vaya a **Proceso perimetral > Máquinas virtuales**.

        ![Agregar imagen de VM 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

    1. Seleccione **Máquinas virtuales** para ir a la página de **información general**. **Habilite** la administración en la nube de máquinas virtuales.
        ![Agregar imagen de VM 2](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)