---
title: Compatibilidad de FTP activo con Azure Firewall
description: De manera predeterminada, el FTP activo está deshabilitado en Azure Firewall. Puede habilitarlo mediante PowerShell, la CLI y las plantillas de ARM.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 03/05/2021
ms.author: victorh
ms.openlocfilehash: adbc2a9eb6cd3b054df84911604143ddb711ad20
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102499142"
---
# <a name="azure-firewall-active-ftp-support"></a>Compatibilidad de FTP activo con Azure Firewall

Con el FTP activo, el servidor FTP inicia la conexión de datos con puerto de datos del cliente FTP designado. Normalmente, los firewalls de la red del cliente bloquean una solicitud de conexión externa a un puerto de cliente interno. Para más información, consulte [FTP activo frente a FTP pasivo, la explicación definitiva](https://slacksite.com/other/ftp.html).

De manera predeterminada, la compatibilidad con el FTP activo está deshabilitada en Azure Firewall para protegerse frente a ataques de devolución FTP mediante el comando `PORT` de FTP. Sin embargo, puede habilitar el FTP activo durante la implementación mediante Azure PowerShell, la CLI de Azure o una plantilla de Azure ARM.


## <a name="azure-powershell"></a>Azure PowerShell

Para realizar la implementación mediante Azure PowerShell, use el parámetro `AllowActiveFTP`. Para más información, consulte [Creación de un firewall con Permitir FTP activo](/powershell/module/az.network/new-azfirewall#16---create-a-firewall-with-allow-active-ftp-).

## <a name="azure-cli"></a>Azure CLI

Para realizar la implementación mediante la CLI de Azure, use el parámetro `--allow-active-ftp`. Para más información, consulte [az network firewall create](/cli/azure/ext/azure-firewall/network/firewall#ext_azure_firewall_az_network_firewall_create-optional-parameters). 

## <a name="azure-resource-manager-arm-template"></a>Plantilla de Azure Resource Manager (ARM)

Para realizar la implementación con una plantilla de ARM, use el campo `AdditionalProperties`:

```json
"additionalProperties": {
            "Network.FTP.AllowActiveFTP": "True"
        },
```
Para obtener más información, consulte [Microsoft.Network azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="next-steps"></a>Pasos siguientes

Para aprender a implementar una instancia de Azure Firewall, consulte [Implementación y configuración de Azure Firewall mediante Azure PowerShell](deploy-ps.md).