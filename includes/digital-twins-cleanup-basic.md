---
author: baanders
description: archivo de inclusión para limpiar una instancia de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: 9a02c4f5c5699b4a6308bfaa519fa9eb776414d6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244778"
---
* **Si no necesita ninguno de los recursos que creó en este tutorial**, puede eliminar la instancia de Azure Digital Twins y todos los demás recursos de este artículo con el comando [az group delete](/cli/azure/group#az-group-delete). Esto permite eliminar todos los recursos de Azure de un grupo de recursos, así como el grupo en sí.
    
    > [!IMPORTANT]
    > La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados.
    
    Abra [Azure Cloud Shell](https://shell.azure.com) y ejecute el siguiente comando para eliminar el grupo de recursos y todo lo que contiene.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```