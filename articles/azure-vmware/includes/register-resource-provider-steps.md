---
title: Registro del proveedor de recursos de Azure VMware Solution
description: Pasos para registrar el proveedor de recursos de Azure VMware Solution.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: cd4a6f3003945973f0fe5367eb198823595a412e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750599"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

Para usar Azure VMware Solution, primero debe registrar el proveedor de recursos con la suscripción. Para más información sobre los proveedores de recursos, consulte [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md).

### <a name="azure-cli"></a>Azure CLI 

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

### <a name="azure-portal"></a>Azure Portal
 
1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el menú de Azure Portal, seleccione **Todos los servicios**.

1. En el cuadro **Todos los servicios**, escriba **suscripción** y, a continuación, seleccione **Suscripciones**.

1. Seleccione la suscripción en la lista de suscripción para verla.

1. Seleccione **Proveedores de recursos** y escriba **Microsoft.AVS** en la búsqueda. 
 
1. Si el proveedor de recursos no está registrado, seleccione **Registrar**.