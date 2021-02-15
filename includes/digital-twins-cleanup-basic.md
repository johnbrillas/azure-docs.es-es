---
author: baanders
description: archivo de inclusión para limpiar una instancia de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: c3c1b814b357a2e4b724590261657e485852f99c
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575702"
---
* **Si no necesita ninguno de los recursos que creó en este tutorial**, puede eliminar la instancia de Azure Digital Twins y todos los demás recursos de este artículo con el comando [az group delete](/cli/azure/group?preserve-view=true&view=azure-cli-latest#az-group-delete). Esto permite eliminar todos los recursos de Azure de un grupo de recursos, así como el grupo en sí.
    
    > [!IMPORTANT]
    > La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados.
    
    Abra [Azure Cloud Shell](https://shell.azure.com) y ejecute el siguiente comando para eliminar el grupo de recursos y todo lo que contiene.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```