---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: 872164dfee9f35881fca97b0339bd95c64a2f5ff
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99070221"
---
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Use el cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) de Azure PowerShell para crear un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*. 

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```
