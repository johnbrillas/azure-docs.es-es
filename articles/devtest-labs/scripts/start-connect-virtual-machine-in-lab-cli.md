---
title: 'Ejemplo de script de la CLI de Azure: Inicio de una máquina virtual en un laboratorio | Microsoft Docs'
description: Este script de la CLI de Azure inicia una máquina virtual en un laboratorio de Azure DevTest Labs.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: 8a3308a4e13b82cd90e00b6c25edadf4cc8aa4ee
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198173"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Use la CLI de Azure para iniciar una máquina virtual en un laboratorio de Azure DevTest Labs.

Este script de la CLI de Azure inicia una máquina virtual en un laboratorio. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos:

| Get-Help | Notas |
|---|---|
| [az lab vm start](/cli/azure/lab/vm#az-lab-vm-start) | Inicia una máquina virtual en un laboratorio. Esta operación puede tardar varios minutos en completarse. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de Azure Lab Services en los [ejemplos de la CLI de Azure Lab Services](../samples-cli.md).
