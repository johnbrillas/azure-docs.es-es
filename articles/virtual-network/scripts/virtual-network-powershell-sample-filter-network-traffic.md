---
title: 'Filtrado del tráfico de red de una máquina virtual: ejemplo de script de Azure PowerShell'
description: 'Filtro del tráfico de red de VM entrante y saliente: ejemplo de script de Azure PowerShell.'
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e2a6c3a656065d6168280705d3cc27adabceb5d2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96491390"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic-script-sample"></a>Ejemplo de script para filtrar el tráfico de red entrante y saliente de una máquina virtual

Este ejemplo de script crea una red virtual con subredes de front-end y back-end. El tráfico de red entrante hacia la subred de front-end se limita a HTTP y HTTPS, mientras que el tráfico saliente hacia Internet desde la subred back-end no está permitido. Después de ejecutar el script, tendrá una máquina virtual con dos NIC. Cada NIC se conecta a una subred diferente.

Puede ejecutar el script desde Azure [Cloud Shell](https://shell.azure.com/powershell) o desde una instalación de PowerShell local. Si usa PowerShell de forma local, este script requiere la versión del módulo de Azure PowerShell 1.0.0 o posterior. Ejecute `Get-InstalledModule -Name Az` para ver cuál es la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la VM y todos los recursos relacionados:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una red virtual y grupos de seguridad de red. Cada comando de la tabla siguiente crea un vínculo a documentación específica del comando:

| Get-Help | Notas |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Crea un objeto de configuración de subred. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Crea una subred de front-end y una red virtual de Azure. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Crea reglas de seguridad que se asignarán a un grupo de seguridad de red. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) |Crea reglas NSG que permiten o bloquean puertos específicos para subredes concretas. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Asocia NSG a subredes. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Crea una dirección IP pública para acceder a la VM desde Internet. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Crea interfaces de red virtual y las asocia a subredes de front-end y back-end de la red virtual. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Crea una configuración de máquina virtual. Esta configuración incluye diversa información, como el nombre de la máquina virtual, sistema el operativo y las credenciales administrativas. La configuración se utiliza durante la creación de las máquinas virtuales. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Cree una máquina virtual. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/).

Puede encontrar más ejemplos de script de PowerShell de red virtual en [ejemplos de PowerShell de red virtual](../powershell-samples.md).
