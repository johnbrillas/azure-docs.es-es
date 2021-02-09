---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/15/2021
ms.author: alkohli
ms.openlocfilehash: 8f3031669723cd61715c12a42f99869ac0eaf3bc
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99500454"
---
Para poder implementar VM en el dispositivo Azure Stack Edge, debe configurar el cliente para que se conecte al dispositivo mediante Azure Resource Manager con Azure PowerShell. Para consultar los pasos detallados, vaya a [Conectar con Azure Resource Manager desde el dispositivo Azure Stack Edge](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md).

Asegúrese de que se pueden usar los pasos siguientes para acceder al dispositivo desde el cliente. (Ha realizado esta configuración al conectarse a Azure Resource Manager. Ahora solo está comprobando que la configuración se realizó correctamente). 

1. Compruebe que la comunicación de Azure Resource Manager funciona. Especifique:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Llame a las API del dispositivo local para autenticarse. Especifique: 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    Proporcione el nombre de usuario *EdgeARMuser* y la contraseña para conectarse mediante Azure Resource Manager.

1. Si configuró **Proceso** para Kubernetes, puede omitir este paso. Continúe para asegurarse de que ha habilitado una interfaz de red para el proceso. En la interfaz de usuario local, vaya a los valores de **Proceso**. Seleccione la interfaz de red que desea usar para crear un conmutador virtual. Las VM que cree se adjuntarán a un conmutador virtual conectado a este puerto y a la red asociada. Asegúrese de elegir una red que coincida con la dirección IP que usará para la máquina virtual.  

    ![Captura de pantalla que muestra cómo habilitar los valores de Proceso.](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Habilite el proceso en la interfaz de red. Azure Stack Edge creará y administrará un conmutador virtual correspondiente a esa interfaz de red. No especifique direcciones IP específicas para Kubernetes en este momento. La habilitación del proceso puede tardar varios minutos.

    > [!NOTE]
    > Si va a crear máquinas virtuales de GPU, seleccione una interfaz de red conectada a Internet, ya que ello le permite instalar una extensión de GPU en el dispositivo.


