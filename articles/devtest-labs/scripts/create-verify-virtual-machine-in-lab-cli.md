---
title: 'CLI de Azure: creación y comprobación de una máquina virtual en un laboratorio'
description: Este script de la CLI de Azure crea una máquina virtual en un laboratorio y comprueba que está disponible.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: c7625f62d7897d61903f864b216ccf9aa13648ea
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198428"
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Uso de la CLI de Azure para crear y comprobar la disponibilidad de una máquina virtual en un laboratorio de Azure DevTest Labs

Este script de la CLI de Azure crea una máquina virtual en un laboratorio. La máquina virtual basada en una imagen de Marketplace con autenticación ssh. El script comprueba entonces que la máquina virtual está disponible para su uso. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos:

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az lab vm create](/cli/azure/lab/vm#az-lab-vm-create) | Crea una máquina virtual en un laboratorio. |
| [az lab vm show](/cli/azure/lab/vm#az-lab-vm-show) | Muestra el estado de la máquina virtual en un laboratorio. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de Azure Lab Services en los [ejemplos de la CLI de Azure Lab Services](../samples-cli.md).
