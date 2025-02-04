---
title: 'Ejemplo de script de la CLI de Azure: administrar el tráfico web | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: administrar el tráfico de web con una puerta de enlace de aplicaciones y un conjunto de escalado de máquinas virtuales.'
services: application-gateway
documentationcenter: networking
author: vhorne
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 2e766bcdee2afa3ff4ed75476c619326c4c0e0c2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99591674"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Administrar el tráfico web mediante la CLI de Azure

Este script crea una puerta de enlace de aplicaciones que usa un conjunto de escalado de máquinas virtuales para los servidores back-end. La puerta de enlace de aplicaciones se puede configurar para administrar el tráfico web. Después de ejecutar el script, se puede probar la puerta de enlace de aplicaciones mediante la dirección IP pública.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la puerta de enlace de aplicaciones y todos los recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az network vnet create](/cli/azure/network/vnet) | Crea una red virtual. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Crea una subred en una red virtual. |
| [az network public-ip create](/cli/azure/network/public-ip) | Crea la dirección IP pública de la puerta de enlace de aplicaciones. |
| [az network application-gateway create](/cli/azure/network/application-gateway) | Crear una puerta de enlace de aplicaciones. |
| [az vmss create](/cli/azure/vmss) | Crea un conjunto de escalado de máquinas virtuales. |
| [az network public-ip show](/cli/azure/network/public-ip) | Obtiene la dirección IP pública de la puerta de enlace de aplicaciones. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure/overview).

Encontrará más ejemplos de scripts de la CLI de la puerta de enlace de aplicaciones en la [documentación sobre VM de Windows de Azure](../cli-samples.md).
