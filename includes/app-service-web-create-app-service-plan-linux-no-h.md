---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 12/20/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: f3d558736751d3c50e3c007e3aebb369093ef856
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244543"
---
En Cloud Shell, cree un plan de App Service en el grupo de recursos con el comando [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create).

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

En el ejemplo siguiente se crea un plan de App Service denominado `myAppServicePlan` en el plan de tarifa **Gratis** (`--sku F1`) en un contenedor Linux (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku F1 --is-linux
```

Cuando se crea el plan de App Service, la CLI de Azure muestra información similar al ejemplo siguiente:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  <JSON data removed for brevity.>
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
