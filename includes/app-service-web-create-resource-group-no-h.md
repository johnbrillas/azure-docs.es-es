---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 520a32ed44951a711dcb1d0975fb452829530c4f
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244805"
---
[!INCLUDE [resource group intro text](resource-group.md)]

En Cloud Shell, cree un grupo de recursos con el comando [`az group create`](/cli/azure/group). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *Oeste de Europa*. Para ver todas las ubicaciones que se admiten en App Service en el nivel **Gratis**, ejecute el comando [`az appservice list-locations --sku FREE`](/cli/azure/appservice).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Generalmente se crean el grupo de recursos y los recursos en una región cercana. 

Cuando finaliza el comando, una salida de JSON muestra las propiedades del grupo de recursos.