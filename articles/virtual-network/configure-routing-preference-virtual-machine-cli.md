---
title: Configuración de la preferencia de enrutamiento de una VM - CLI de Azure
description: Obtenga información sobre cómo crear una VM con una dirección IP pública con preferencia de enrutamiento mediante la interfaz de la línea de comandos (CLI) de Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2021
ms.author: mnayak
ms.custom: devx-track-azurecli
ms.openlocfilehash: ad8f2d150c3cf17c4b24c6dc92188be9017dcfa9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101666002"
---
# <a name="configure-routing-preference-for-a-vm-using-azure-cli"></a>Configuración de la preferencia de enrutamiento de una VM mediante la CLI de Azure

En este artículo se muestra cómo configurar las preferencias de enrutamiento para una máquina virtual. El tráfico de Internet enlazado desde la VM se enrutará a través de la red del ISP cuando elija **Internet** como opción de preferencia de enrutamiento. El enrutamiento predeterminado se hará a través de la red global de Microsoft.

En este artículo se muestra cómo crear una máquina virtual con una dirección IP pública que esté configurada para enrutar el tráfico a través de la red pública de Internet mediante la CLI de Azure.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
1. Si usa Cloud Shell, continúe al paso 2. Abra una sesión de comandos e inicie sesión en Azure con `az login`.
2. Para crear un grupo de recursos, use el comando [az group create](/cli/azure/group#az-group-create). En el siguiente ejemplo se crea un grupo de recursos en la región Este de EE. UU. de Azure:

    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-public-ip-address"></a>Crear una dirección IP pública
Para obtener acceso a las máquinas virtuales desde Internet, necesita crear una dirección IP pública. Cree una dirección IP pública con [az network public-ip create](/cli/azure/network/public-ip). En el ejemplo siguiente se crea una dirección IP pública con una preferencia de enrutamiento de tipo *Internet* en la región del *Este de EE. UU.* :

```azurecli
az network public-ip create \
--name MyRoutingPrefIP \
--resource-group MyResourceGroup \
--location eastus \
--ip-tags 'RoutingPreference=Internet' \
--sku STANDARD \
--allocation-method static \
--version IPv4
```

## <a name="create-network-resources"></a>Crear recursos de red

Antes de implementar una VM, debe crear recursos de red compatibles, como un grupo de seguridad de red, una red virtual y un NIC virtual.

### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

Cree un grupo de seguridad de red para las reglas que van a controlar la comunicación entrante y saliente de la red virtual con [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create).

```azurecli
az network nsg create \
--name myNSG  \
--resource-group MyResourceGroup \
--location eastus
```

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree la red virtual con el comando [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). En el ejemplo siguiente se crea una red virtual denominada *myVnet* con las subredes *mySubnet*:

```azurecli
# Create a virtual network
az network vnet create \
--name myVNET \
--resource-group MyResourceGroup \
--location eastus

# Create a subnet
az network vnet subnet create \
--name mySubNet \
--resource-group MyResourceGroup \
--vnet-name myVNET \
--address-prefixes "10.0.0.0/24" \
--network-security-group myNSG
```

### <a name="create-a-nic"></a>Creación de un NIC

Cree un NIC virtual para la VM con [az network nic create](/cli/azure/network/nic#az-network-nic-create). En el ejemplo siguiente se crea un NIC virtual, que se conectará a la VM.

```azurecli-interactive
# Create a NIC
az network nic create \
--name mynic  \
--resource-group MyResourceGroup \
--network-security-group myNSG  \
--vnet-name myVNET \
--subnet mySubNet  \
--private-ip-address-version IPv4 \
--public-ip-address MyRoutingPrefIP
```

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

Cree la máquina virtual con [az vm create](/cli/azure/vm#az-vm-create). En el ejemplo siguiente, se crea una VM de Windows Server 2019 y los componentes de red virtual necesarios, si aún no existen.

```azurecli
az vm create \
--name myVM \
--resource-group MyResourceGroup \
--nics mynic \
--size Standard_A2 \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest \
--admin-username myUserName
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede utilizar [az group delete](/cli/azure/group#az-group-delete) para eliminar el grupo de recursos y todos los recursos que contenga:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [preferencia de enrutamiento en direcciones IP públicas](routing-preference-overview.md).
- Obtenga más información acerca de las [direcciones IP públicas](./public-ip-addresses.md#public-ip-addresses) en Azure.
- Obtenga más información sobre la [configuración de las direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).
