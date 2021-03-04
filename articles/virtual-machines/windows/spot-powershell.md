---
title: Uso de PowerShell para implementar máquinas virtuales de acceso puntual de Azure
description: Aprenda a usar Azure PowerShell para implementar máquinas virtuales de acceso puntual de Azure para ahorrar costos.
author: cynthn
ms.service: virtual-machines
ms.subservice: spot
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 33172004ac4361de51b92389fbf56bd699f7124f
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096452"
---
# <a name="deploy-azure-spot-virtual-machines-using-azure-powershell"></a>Implementación de máquinas virtuales de acceso puntual de Azure con Azure PowerShell


El uso de [máquinas virtuales de acceso puntual de Azure](../spot-vms.md) permite aprovechar las ventajas de nuestra capacidad no utilizada con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de acceso puntual de Azure. Por lo tanto, las máquinas virtuales de acceso puntual de Azure son excelentes para cargas de trabajo que puedan soportar interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo/pruebas, las grandes cargas de trabajo de proceso, etc.

Los precios de las máquinas virtuales de acceso puntual de Azure son variables, en función de la región y la SKU. Para más información, consulte precios de las máquinas virtuales para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Para obtener más información sobre cómo se establece el precio máximo, consulte [Máquinas virtuales de acceso puntual de Azure: Precios](../spot-vms.md#pricing).

Puede establecer el precio máximo por hora que esté dispuesto por la máquina virtual. El precio máximo de una máquina virtual de acceso puntual de Azure se puede establecer en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la máquina virtual no se expulsará por precio. El precio de la máquina virtual será el actual de Spot o el de una máquina virtual estándar, el menor de los dos, siempre que haya capacidad y cuota disponibles.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree una máquina virtual de Spot y sírvase de [New-AzVmConfig](/powershell/module/az.compute/new-azvmconfig) para crear la configuración. Incluya `-Priority Spot` y establezca `-MaxPrice` en:
- `-1` para que la máquina virtual no se expulse en función del precio
- o en una cantidad de dólares, de hasta 5 dígitos. Por ejemplo, `-MaxPrice .98765` significa que la máquina virtual se desasignará cuando el precio de una máquina virtual de Spot sea de aproximadamente 0,98765 USD por hora.


En este ejemplo se crea una máquina virtual de Spot que no se desasigna por precio (solo cuando Azure requiera la capacidad). La directiva de expulsión se establece para desasignar la máquina virtual, de modo que se pueda reiniciar en otro momento. Si quiere eliminar la máquina virtual y el disco subyacente cuando se expulsa la máquina virtual, establezca `-EvictionPolicy` en `Delete` en `New-AzVMConfig`.


```azurepowershell-interactive
$resourceGroup = "mySpotRG"
$location = "eastus"
$vmName = "mySpotVM"
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."
New-AzResourceGroup -Name $resourceGroup -Location $location
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup `
   -Location $location -Name MYvNET -AddressPrefix 192.168.0.0/16 `
   -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration and set this to be an Azure Spot Virtual Machine

$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1 -Priority "Spot" -MaxPrice -1 -EvictionPolicy Deallocate | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzVMNetworkInterface -Id $nic.Id

New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Una vez creada la máquina virtual, puede realizar una consulta para ver el precio máximo de todas las máquinas virtuales del grupo de recursos.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName $resourceGroup | `
   Select-Object Name,@{Name="maxPrice"; Expression={$_.BillingProfile.MaxPrice}}
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

## <a name="next-steps"></a>Pasos siguientes

También puede crear una máquina virtual de acceso puntual de Azure mediante la [CLI de Azure](../linux/spot-cli.md), el [portal](../spot-portal.md) o una [plantilla](../linux/spot-template.md).

Consulte la información sobre precios con la [API de precios de venta directa de Azure](/rest/api/cost-management/retail-prices/azure-retail-prices) para conocer los precios de máquinas virtuales de acceso puntual de Azure. Tanto `meterName` como `skuName` contendrán `Spot`.

Si se produce un error, consulte [Códigos de error](../error-codes-spot.md).
