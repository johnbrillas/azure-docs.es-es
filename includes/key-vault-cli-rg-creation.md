---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: 97fd969ddae2f2c222209259cccd6f6f55272524
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99070274"
---
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Utilice el comando [az group create](/cli/azure/group#az_group_create) para crear un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"
```
