---
title: Uso de la CLI para implementar máquinas virtuales de acceso puntual de Azure
description: Aprenda a usar la CLI para implementar máquinas virtuales de acceso puntual de Azure con la finalidad de ahorrar costos.
author: cynthn
ms.service: virtual-machines
ms.subservice: spot
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 0a7be682f921efdfae486e8f6545758964a941ae
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098866"
---
# <a name="deploy-azure-spot-virtual-machines-using-the-azure-cli"></a>Implementación de máquinas virtuales de acceso puntual de Azure con la CLI de Azure

El uso de [máquinas virtuales de acceso puntual de Azure](../spot-vms.md) permite aprovechar las ventajas de nuestra capacidad no utilizada con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de acceso puntual de Azure. Por lo tanto, las máquinas virtuales de acceso puntual de Azure son excelentes para cargas de trabajo que puedan soportar interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo/pruebas, las grandes cargas de trabajo de proceso, etc.

Los precios de las máquinas virtuales de acceso puntual de Azure son variables, en función de la región y la SKU. Para más información, consulte precios de las máquinas virtuales para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Puede establecer el precio máximo por hora que esté dispuesto por la máquina virtual. El precio máximo de una máquina virtual de acceso puntual de Azure se puede establecer en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la máquina virtual no se expulsará por precio. El precio de la máquina virtual será el precio actual de la máquina virtual de acceso puntual de Azure o, de ser menor, el de una máquina virtual estándar, siempre que haya capacidad y cuota disponibles. Para más información sobre cómo se establece el precio máximo, consulte [Máquinas virtuales de acceso puntual de Azure: precios](../spot-vms.md#pricing).

El proceso de creación de una máquina virtual de acceso puntual de Azure con la CLI de Azure es el mismo que el que se detalla en el [artículo de inicio rápido](./quick-create-cli.md). Tan solo agregue el parámetro "--priority Spot", establezca `--eviction-policy` en Desasignar (este es el valor predeterminado) o en `Delete` e indique un precio máximo o `-1`. 


## <a name="install-azure-cli"></a>Instalación de la CLI de Azure

Para crear máquinas virtuales de acceso puntual de Azure, debe ejecutar la versión 2.0.74 o posterior de la CLI de Azure. Para saber qué versión tiene, ejecute el comando **az --version**. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

Inicie sesión en Azure mediante [az login](/cli/azure/reference-index#az-login).

```azurecli
az login
```

## <a name="create-an-azure-spot-virtual-machine"></a>Creación de una máquina virtual de acceso puntual de Azure

En este ejemplo se muestra cómo implementar una máquina virtual de acceso puntual de Azure en Linux que no se expulse en función del precio. La directiva de expulsión se establece para desasignar la VM, de modo que se pueda reiniciar en otro momento. Si quiere eliminar la VM y el disco subyacente cuando se expulsa la VM, establezca `--eviction-policy` en `Delete`.

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 \
    --eviction-policy Deallocate
```



Una vez creada la máquina virtual, puede realizar una consulta para ver el precio máximo de facturación de todas las máquinas virtuales del grupo de recursos.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

## <a name="simulate-an-eviction"></a>Simulación de una expulsión

Puede [simular una expulsión](/rest/api/compute/virtualmachines/simulateeviction) de una máquina virtual de acceso puntual de Azure para probar de qué manera la aplicación responderá a una expulsión repentina. 

Reemplazar lo siguiente por su propia información: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```
`Response Code: 204` significa que la expulsión simulada se ha realizado correctamente. 

**Pasos siguientes**

También puede crear una máquina virtual de acceso puntual de Azure mediante [Azure PowerShell](../windows/spot-powershell.md), el [portal](../spot-portal.md) o una [plantilla](spot-template.md).

Para información sobre las máquinas virtuales de acceso puntual de Azure, consulte la información sobre precios usando la [API de precios de venta directa de Azure](/rest/api/cost-management/retail-prices/azure-retail-prices). Tanto `meterName` como `skuName` contendrán `Spot`.

Si se produce un error, consulte [Códigos de error](../error-codes-spot.md).
