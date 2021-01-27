---
title: Actualización o eliminación de la instancia de Azure Load Balancer existente utilizada por el conjunto de escalado de máquinas virtuales
titleSuffix: Update or delete existing Azure Load Balancer used by Virtual Machine Scale Set
description: Con este artículo de procedimientos, podrá empezar a usar Azure Standard Load Balancer y Virtual Machine Scale Sets.
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/30/2020
ms.author: irenehua
ms.openlocfilehash: d5614490bfd2cfb67b6b7afd7b7b8643bbf754bd
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98790096"
---
# <a name="how-to-updatedelete-azure-load-balancer-used-by-virtual-machine-scale-sets"></a>Cómo actualizar o eliminar la instancia de Azure Load Balancer usada por Virtual Machine Scale Sets

## <a name="how-to-set-up-azure-load-balancer-for-scaling-out-virtual-machine-scale-sets"></a>Cómo configurar Azure Load Balancer para el escalado horizontal de Virtual Machine Scale Sets
  * Asegúrese de que Load Balancer tenga configurado un [grupo NAT de entrada](/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest) y que el conjunto de escalado de máquinas virtuales esté colocado en el grupo de back-end de Load Balancer. Azure Load Balancer creará automáticamente nuevas reglas NAT de entrada en el grupo NAT de entrada cuando se agreguen nuevas instancias de máquina virtual al conjunto de escalado de máquinas virtuales. 
  * Para comprobar si el grupo NAT de entrada está correctamente configurado: 
  1. Inicie sesión en Azure Portal en https://portal.azure.com.
  
  1. Seleccione **Todos los recursos** en el menú de la izquierda y, a continuación, en la lista de recursos seleccione **MyLoadBalancer**.
  
  1. En **Configuración**, seleccione **Reglas NAT de entrada**.
Si ve en el panel derecho una lista de reglas creadas para cada instancia del conjunto de escalado de máquinas virtuales, significa que está todo listo para realizar el escalado vertical en cualquier momento.

## <a name="how-to-add-inbound-nat-rules"></a>¿Cómo se agregan reglas NAT de entrada? 
  * No se puede agregar una sola regla NAT de entrada. Sin embargo, se puede agregar un conjunto de reglas NAT de entrada con un intervalo de puertos de front-end y un puerto de back-end definidos para todas las instancias del conjunto de escalado de máquinas virtuales.
  * Para agregar un conjunto completo de reglas NAT de entrada para los conjuntos de escalado de máquinas virtuales, primero debe crear un grupo NAT de entrada en Load Balancer y, luego, hacer referencia a él desde el perfil de red del conjunto de escalado de máquinas virtuales. A continuación, se muestra un ejemplo completo que usa la CLI:
  * El nuevo grupo NAT de entrada no debe tener un intervalo de puertos de front-end superpuestos con grupos NAT de entrada existentes. Para ver los grupos NAT de entrada existentes configurados, puede usar este [comando de la CLI](/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest#az_network_lb_inbound_nat_pool_list).
```azurecli-interactive
az network lb inbound-nat-pool create 
        -g MyResourceGroup 
        --lb-name MyLb
        -n MyNatPool 
        --protocol Tcp 
        --frontend-port-range-start 80 
        --frontend-port-range-end 89 
        --backend-port 80 
        --frontend-ip-name MyFrontendIp
az vmss update 
        -g MyResourceGroup 
        -n myVMSS 
        --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools "{'id':'/subscriptions/mySubscriptionId/resourceGroups/MyResourceGroup/providers/Microsoft.Network/loadBalancers/MyLb/inboundNatPools/MyNatPool'}"
        
az vmss update-instances
        -–instance-ids *
        --resource-group MyResourceGroup
        --name MyVMSS
```
## <a name="how-to-update-inbound-nat-rules"></a>¿Cómo se actualizan las reglas NAT de entrada? 
  * No se puede actualizar una sola regla NAT de entrada. Sin embargo, se puede actualizar un conjunto de reglas NAT de entrada con un intervalo de puertos de front-end y un puerto de back-end definidos para todas las instancias del conjunto de escalado de máquinas virtuales.
  * Con el fin de actualizar un conjunto completo de reglas NAT de entrada para los conjuntos de escalado de máquinas virtuales, debe actualizar el grupo NAT de entrada en Load Balancer. 
```azurecli-interactive
az network lb inbound-nat-pool update 
        -g MyResourceGroup 
        --lb-name MyLb 
        -n MyNatPool
        --protocol Tcp 
        --backend-port 8080
```

## <a name="how-to-delete-inbound-nat-rules"></a>¿Cómo se eliminan las reglas NAT de entrada? 
* No se puede eliminar una sola regla NAT de entrada. Sin embargo, puede eliminar el conjunto completo de reglas NAT de entrada.
* Para eliminar el conjunto completo de reglas NAT de entrada usadas por el conjunto de escalado, primero debe quitar el grupo NAT del conjunto de escalado. A continuación se muestra un ejemplo completo que usa la CLI:
```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
   az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
  az network lb inbound-nat-pool delete
     --resource-group MyResourceGroup
     -–lb-name MyLoadBalancer
     --name MyNatPool
```

## <a name="how-to-add-multiple-ip-configurations"></a>Cómo agregar varias configuraciones de IP:
1. Seleccione **Todos los recursos** en el menú de la izquierda y, a continuación, en la lista de recursos seleccione **MyLoadBalancer**.
   
1. En **Configuración**, seleccione **Configuraciones de IP de front-end** y elija **Agregar**.
   
1. En la página **Agregar dirección IP de front-end**, escriba los valores y seleccione **Aceptar**.

1. Siga el [paso 5](./load-balancer-multiple-ip.md#step-5-configure-the-health-probe) y el [paso 6](./load-balancer-multiple-ip.md#step-5-configure-the-health-probe) de este tutorial si se necesitan nuevas reglas de equilibrio de carga

1. Si es necesario, cree un conjunto de reglas NAT de entrada con las configuraciones de IP de front-end recién creadas. En la [sección anterior] se puede encontrar un ejemplo.

## <a name="how-to-delete-frontend-ip-configuration-used-by-virtual-machine-scale-set"></a>Cómo eliminar la configuración de IP de front-end usada por el conjunto de escalado de máquinas virtuales: 
 1. Para eliminar la configuración de IP de front-end que usa el conjunto de escalado, primero debe eliminar el grupo NAT de entrada (conjunto de reglas NAT de entrada) que hace referencia a ella. Encontrará instrucciones sobre cómo eliminar reglas de entrada en la sección anterior.
 1. Elimine la regla de equilibrio de carga que hace referencia a la configuración de IP de front-end. 
 1. Elimine la configuración de IP de front-end.
 

## <a name="how-to-delete-azure-load-balancer-used-by-virtual-machine-scale-set"></a>Cómo eliminar la instancia de Azure Load Balancer usada por el conjunto de escalado de máquinas virtuales: 
 1. Para eliminar la configuración de IP de front-end que usa el conjunto de escalado, primero debe eliminar el grupo NAT de entrada (conjunto de reglas NAT de entrada) que hace referencia a ella. Encontrará instrucciones sobre cómo eliminar reglas de entrada en la sección anterior.
 
 1. Elimine la regla de equilibrio de carga que hace referencia al grupo de back-end que contiene el conjunto de escalado de máquinas virtuales.
 
 1. Quite la referencia loadBalancerBackendAddressPool del perfil de red del conjunto de escalado de máquinas virtuales. A continuación se muestra un ejemplo completo que usa la CLI:
 ```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
     --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools
  az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
```
Por último, elimine el recurso Load Balancer.
 
## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Load Balancer y el conjunto de escalado de máquinas virtuales, lea más sobre los conceptos.

> [Azure Load Balancer con el conjunto de escalado de máquinas virtuales de Azure](load-balancer-standard-virtual-machine-scale-sets.md)