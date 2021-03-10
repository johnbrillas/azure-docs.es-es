---
title: Creación y configuración de un almacén de claves para Azure Disk Encryption
description: En este artículo se proporcionan los pasos necesarios para crear y configurar un almacén de claves para su uso con Azure Disk Encryption
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 3d30d506ae3b49b2e247ee77ef17d101f94854c7
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102501024"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Creación y configuración de un almacén de claves para Azure Disk Encryption

Azure Disk Encryption usa Azure Key Vault para controlar y administrar las claves y los secretos de cifrado de discos.  Para más información sobre los almacenes de claves, consulte [Introducción a Azure Key Vault](../key-vault/general/overview.md) y [Protección de un almacén de claves](../key-vault/general/secure-your-key-vault.md).

La creación y configuración de un almacén de claves para su uso con Azure Disk Encryption conlleva estos tres pasos:

1. Creación de un grupo de recursos, en caso de que sea necesario.
2. Creación de un almacén de claves. 
3. Establecimiento de directivas de acceso avanzadas del almacén de claves.

Estos pasos se muestran en los siguientes tutoriales de inicio rápido:

Si lo desea, también puede generar o importar una clave de cifrado de claves (KEK).

## <a name="install-tools-and-connect-to-azure"></a>Instalación de herramientas y conexión a Azure

Los pasos que se describen en este artículo se pueden completar con la [CLI de Azure](/cli/azure/), el [módulo Az de Azure PowerShell](/powershell/azure/) o [Azure Portal](https://portal.azure.com).

### <a name="connect-to-your-azure-account"></a>Conexión a la cuenta de Azure

Antes de usar la CLI de Azure o Azure PowerShell, es preciso conectarse a su suscripción de Azure. Para ello, [inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli), [inicie sesión con Azure PowerShell](/powershell/azure/authenticate-azureps) o escriba sus credenciales en Azure Portal cuando se le soliciten.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Disk Encryption](disk-encryption-overview.md)
- [Cifrado de conjuntos de escalado de máquinas virtuales mediante la CLI de Azure](disk-encryption-cli.md)
- [Cifrado de conjuntos de escalado de máquinas virtuales mediante Azure PowerShell](disk-encryption-powershell.md)
