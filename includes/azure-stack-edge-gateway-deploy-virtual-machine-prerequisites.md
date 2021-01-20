---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/15/2021
ms.author: alkohli
ms.openlocfilehash: 56fc24966fa60c3a5e91f92b57332ae2f6a525ff
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98256540"
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


