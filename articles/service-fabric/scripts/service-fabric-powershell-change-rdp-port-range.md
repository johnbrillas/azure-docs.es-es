---
title: 'Ejemplo de script de Azure PowerShell: cambiar el intervalo de puertos RDP | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: cambiar el intervalo de puertos RDP de un clúster implementado.'
services: service-fabric
tags: azure-service-management
author: athinanthny
ms.author: atsenthi
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 03/19/2018
ms.openlocfilehash: 750a2100d34e02cac7c613cc6b95160fc348b535
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96576373"
---
# <a name="update-the-rdp-port-range-values"></a>Actualización de los valores del intervalo de puertos RDP

Este script de ejemplo cambia los valores del intervalo de puertos RDP en las VM del nodo de clúster una vez implementado el clúster.  Azure PowerShell se utiliza para que las máquinas virtuales subyacentes no hagan el ciclo.  El script obtiene el recurso `Microsoft.Network/loadBalancers` del clúster grupo de recursos del clúster y actualiza los valores `inboundNatPools.frontendPortRangeStart` y `inboundNatPools.frontendPortRangeEnd`. Personalice los parámetros según sea necesario.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si es necesario, instale PowerShell con la instrucción que se encuentra en la [Guía de instalación de Azure PowerShell](/powershell/azure/).

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtiene el recurso `Microsoft.Network/loadBalancers`. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Actualiza el recurso `Microsoft.Network/loadBalancers`.|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/).

Puede encontrar ejemplos de Azure PowerShell para Azure Service Fabric en los [ejemplos de PowerShell](../service-fabric-powershell-samples.md).
