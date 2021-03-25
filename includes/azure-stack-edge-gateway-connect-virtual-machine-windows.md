---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/04/2020
ms.author: alkohli
ms.openlocfilehash: 6e42fef2aa4415373ed0bac39284f36ba330a4d9
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730654"
---
Conéctese a la máquina virtual Windows mediante el Protocolo de escritorio remoto (RDP) a través de la dirección IP que pasó durante la creación de la máquina virtual.

1. En su cliente, abra RDP. 
1. Vaya a **Inicio** y escriba **mstcs**.
1. En el panel **Conexión a Escritorio remoto**, escriba la dirección IP de la máquina virtual y las credenciales de acceso que usó en el archivo de parámetros de la plantilla de la máquina virtual y, a continuación, seleccione **Conectar**.

   ![Captura de pantalla del panel Conexión a Escritorio remoto para conectarse a través de RDP a la máquina virtual de Windows.](media/azure-stack-edge-gateway-connect-vm-windows/connect-vm-rdp-1.png)

   > [!NOTE]
   > Es posible que tenga que aprobar la conexión a una máquina que no sea de confianza. 

Ahora está conectado a la máquina virtual que se ejecuta en el dispositivo. 
