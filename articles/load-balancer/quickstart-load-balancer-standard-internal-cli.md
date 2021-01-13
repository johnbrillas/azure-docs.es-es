---
title: 'Inicio rápido: Creación de un equilibrador de carga interno: CLI de Azure'
titleSuffix: Azure Load Balancer
description: En esta guía de inicio rápido se muestra cómo crear un equilibrador de carga interno mediante la CLI de Azure.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2020
ms.author: allensu
ms.custom: mvc, devx-track-js, devx-track-azurecli
ms.openlocfilehash: 15060a367bba2d50d7054730321f7f20d4c25e46
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916684"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-azure-cli"></a>Inicio rápido: Creación de un equilibrador de carga interno para equilibrar la carga de las máquinas virtuales con la CLI de Azure

Comience a usar Azure Load Balancer con la CLI de Azure para crear un equilibrador de carga interno y tres máquinas virtuales.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)] 

- Para realizar este inicio rápido es necesaria la versión 2.0.28 o superior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos con [az group create](/cli/azure/group#az_group_create):

* Llamado **CreateIntLBQS-rg**. 
* En la ubicación **eastus**.

```azurecli-interactive
  az group create \
    --name CreateIntLBQS-rg \
    --location eastus

```
---

# <a name="standard-sku"></a>[**SKU Estándar**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Se recomienda usar la SKU Estándar de Load Balancer para las cargas de trabajo de producción. Para más información sobre las SKU, consulte **[SKU de Azure Load Balancer](skus.md)** .

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal.png" alt-text="Recursos de Standard Load Balancer creados para el inicio rápido." border="false":::

## <a name="configure-virtual-network---standard"></a>Configuración de una red virtual: estándar

Antes de implementar las máquinas virtuales y el equilibrador de carga, cree los recursos de red virtual auxiliares.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual con [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create):

* Denominada **myVNet**.
* Con el prefijo de dirección **10.1.0.0/16**.
* Subred denominada **MyBackendSubnet**.
* Con el prefijo de subred **10.1.0.0/24**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Ubicación de **eastus**.

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

Use [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create) para crear una dirección IP pública para el host bastión:

* Cree una dirección IP pública con redundancia de zona estándar llamada **myBastionIP**.
* En **CreateIntLBQS-rg**.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Creación de una subred de Bastion

Use [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) para crear una subred bastión:

* Llamada **AzureBastionSubnet**.
* Con el prefijo de dirección **10.1.1.0/24**.
* En la red virtual **myVNet**.
* En el grupo de recursos **CreateIntLBQS-rg**.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg  \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Creación de un host bastión

Use [az network bastion create](/cli/azure/network/bastion#az-network-bastion-create) para crear un host bastión:

* Denominación **myBastionHost**.
* En **CreateIntLBQS-rg**.
* Asociado a la dirección IP pública **myBastionIP**.
* Asociado a la red virtual **myVNet**.
* En la ubicación **eastus**.

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

El host de Azure Bastion tarda unos minutos en implementarse.


### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

En el caso de un equilibrador de carga estándar, las VM de la dirección de back-end deben tener interfaces de red que pertenezcan a un grupo de seguridad de red. 

Cree un grupo de seguridad de red con [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create):

* Denominado **myNSG**.
* En el grupo de recursos **CreateIntLBQS-rg**.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Creación de una regla de grupo de seguridad de red

Cree una regla de grupo de seguridad de red con el [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create):

* Denominada **myNSGRuleHTTP**.
* En el grupo de seguridad de red que creó en el paso anterior, **myNSG**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Protocolo **(*)** .
* Dirección **Inbound**.
* Origen **(*)** .
* Destino: **(*)** .
* Puerto de destino **80**.
* Acceso: **Allow**.
* Prioridad **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

## <a name="create-backend-servers---standard"></a>Creación de servidores back-end: estándar

En esta sección, creará:

* Tres interfaces de red para las máquinas virtuales.
* Tres máquinas virtuales que se usarán como servidores back-end para el equilibrador de carga.

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Creación de interfaces de red para las máquinas virtuales

Cree tres interfaces de red con [az network nic create](/cli/azure/network/nic#az-network-nic-create):

* Sus nombres serán **myNicVM1**, **myNicVM2** y **myNicVM3**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* En la red virtual **myVNet**.
* En la subred **myBackendSubnet**.
* En el grupo de seguridad de red **myNSG**.

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree las máquinas virtuales con [az vm create](/cli/azure/vm#az-vm-create):

* Asígneles los nombres **myVM1**, **myVM2** y **myVM3**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Conectada a la interfaz de red **myNicVM1**, **myNicVM2** y **myNicVM3**.
* Con la imagen de máquina virtual **Win2019datacenter**.
* En la **Zona 1**, **Zona 2** y **zona 3**.

```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --zone $n \
    --no-wait
  done
```

Puede que las VM tarden unos minutos en implementarse.

## <a name="create-standard-load-balancer"></a>Creación de un equilibrador de carga estándar

En esta sección se detalla cómo se pueden crear y configurar los componentes siguientes del equilibrador de carga:

  * Un grupo de direcciones IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga.
  * Un grupo de direcciones IP de back-end al que el grupo de servidores front-end envía el tráfico de red de carga equilibrada.
  * Un sondeo de estado que determina el estado de las instancias de máquina virtual de back-end.
  * Una regla de equilibrador de carga que define cómo se distribuye el tráfico a las VM.

### <a name="create-the-load-balancer-resource"></a>Creación del recurso del equilibrador de carga

Cree un equilibrador de carga público con [az network lb create](/cli/azure/network/lb#az-network-lb-create):

* Denominado **myLoadBalancer**.
* Un grupo de front-end denominado **MyFrontEnd**.
* Un grupo de back-end denominado **myBackEndPool**
* Asociado a la red virtual **myVNet**.
* Asociado a la subred de back-end **myBackendSubnet**.

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Standard \
    --vnet-name myVnet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool
```

### <a name="create-the-health-probe"></a>Creación del sondeo de estado

Los sondeos de estado comprueban todas las instancias de máquina virtual para asegurarse de que pueden enviar tráfico de red. 

Una máquina virtual con una comprobación de sondeo con errores se quita del equilibrador de carga. La máquina virtual se agrega de nuevo al equilibrador de carga cuando se resuelve el error.

Cree un sondeo de estado con [az network lb probe create](/cli/azure/network/lb/probe#az-network-lb-probe-create):

* Supervisa el estado de las máquinas virtuales.
* Denominado **myHealthProbe**.
* Protocolo **TCP**.
* **Puerto 80** de supervisión.

```azurecli-interactive
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-the-load-balancer-rule"></a>Creación de la regla de equilibrador de carga

Una regla de equilibrador de carga define:

* La configuración de IP del front-end para el tráfico entrante.
* El grupo de IP de back-end para recibir el tráfico.
* Los puertos de origen y de destino requeridos. 

Cree una regla de equilibrador de carga con [az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create):

* Denominada **myHTTPRule**.
* Que escuche en el **puerto 80** en el grupo de front-end **myFrontEnd**.
* Que envíe el tráfico de red con equilibrio de carga al grupo de direcciones de back-end **myBackEndPool** a través del **Puerto 80**. 
* Mediante el sondeo de estado **myHealthProbe**.
* Protocolo **TCP**.
* Un tiempo de espera de inactividad de **15 minutos**.
* Habilite el restablecimiento de TCP.

```azurecli-interactive
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true \
    --idle-timeout 15 \
    --enable-tcp-reset true
```
>[!NOTE]
>Las máquinas virtuales del grupo de back-end no tendrán conectividad de salida a Internet con esta configuración. </br> Para más información acerca de cómo proporcionar conectividad de salida, consulte: </br> **[Conexiones salientes en Azure](load-balancer-outbound-connections.md)**</br> Opciones para proporcionar conectividad: </br> **[Configuración del equilibrador de carga solo de salida](egress-only.md)** </br> **[¿Qué es NAT de Virtual Network?](../virtual-network/nat-overview.md)**

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Adición de máquinas virtuales al grupo de back-end del equilibrador de carga

Agregue las máquinas virtuales al grupo de back-end con [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add):

* En el grupo de direcciones de back-end **myBackEndPool**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Asociada a la interfaz de red **myNicVM1**, **myNicVM2** y **myNicVM3**.
* Asociada al equilibrador de carga **myLoadBalancer**.

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```

# <a name="basic-sku"></a>[**SKU básica**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Se recomienda usar la SKU Estándar de Load Balancer para las cargas de trabajo de producción. Para más información sobre las SKU, consulte **[SKU de Azure Load Balancer](skus.md)** .

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal-basic.png" alt-text="Recursos de Basic Load Balancer creados en el inicio rápido." border="false":::

## <a name="configure-virtual-network---basic"></a>Configuración de una red virtual: básica

Antes de implementar las máquinas virtuales y el equilibrador de carga, cree los recursos de red virtual auxiliares.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual con [az network vnet create](/cli/azure/network/vnet#az-network-vnet-createt):

* Denominada **myVNet**.
* Con el prefijo de dirección **10.1.0.0/16**.
* Subred denominada **MyBackendSubnet**.
* Con el prefijo de subred **10.1.0.0/24**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Ubicación de **eastus**.

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

Use [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create) para crear una dirección IP pública para el host bastión:

* Cree una dirección IP pública con redundancia de zona estándar llamada **myBastionIP**.
* En **CreateIntLBQS-rg**.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Creación de una subred de Bastion

Use [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) para crear una subred bastión:

* Llamada **AzureBastionSubnet**.
* Con el prefijo de dirección **10.1.1.0/24**.
* En la red virtual **myVNet**.
* En el grupo de recursos **CreateIntLBQS-rg**.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Creación de un host bastión

Use [az network bastion create](/cli/azure/network/bastion#az-network-bastion-create) para crear un host bastión:

* Denominación **myBastionHost**.
* En **CreateIntLBQS-rg**.
* Asociado a la dirección IP pública **myBastionIP**.
* Asociado a la red virtual **myVNet**.
* En la ubicación **eastus**.

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

El host de Azure Bastion tarda unos minutos en implementarse.

### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

En el caso de un equilibrador de carga estándar, las VM de la dirección de back-end deben tener interfaces de red que pertenezcan a un grupo de seguridad de red. 

Cree un grupo de seguridad de red con [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create):

* Denominado **myNSG**.
* En el grupo de recursos **CreateIntLBQS-rg**.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Creación de una regla de grupo de seguridad de red

Cree una regla de grupo de seguridad de red con el [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create):

* Denominada **myNSGRuleHTTP**.
* En el grupo de seguridad de red que creó en el paso anterior, **myNSG**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Protocolo **(*)** .
* Dirección **Inbound**.
* Origen **(*)** .
* Destino: **(*)** .
* Puerto de destino **80**.
* Acceso: **Allow**.
* Prioridad **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

## <a name="create-backend-servers---basic"></a>Creación de servidores back-end: básica

En esta sección, creará:

* Tres interfaces de red para las máquinas virtuales.
* Conjunto de disponibilidad para máquinas virtuales
* Tres máquinas virtuales que se usarán como servidores back-end para el equilibrador de carga.

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Creación de interfaces de red para las máquinas virtuales

Cree tres interfaces de red con [az network nic create](/cli/azure/network/nic#az-network-nic-create):

* Sus nombres serán **myNicVM1**, **myNicVM2** y **myNicVM3**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* En la red virtual **myVNet**.
* En la subred **myBackendSubnet**.
* En el grupo de seguridad de red **myNSG**.

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-availability-set-for-virtual-machines"></a>Creación de un conjunto de disponibilidad para máquinas virtuales

Cree el conjunto de disponibilidad con [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create):

* Asígnele el nombre **myAvailabilitySet**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Ubicación **eastus**.

```azurecli-interactive
  az vm availability-set create \
    --name myAvailabilitySet \
    --resource-group CreateIntLBQS-rg \
    --location eastus 
    
```

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree las máquinas virtuales con [az vm create](/cli/azure/vm#az-vm-create):

* Asígneles los nombres **myVM1**, **myVM2** y **myVM3**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Conectada a la interfaz de red **myNicVM1**, **myNicVM2** y **myNicVM3**.
* Con la imagen de máquina virtual **Win2019datacenter**.
* En **myAvailabilitySet**.


```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --availability-set myAvailabilitySet \
    --no-wait
  done
```
Puede que las VM tarden unos minutos en implementarse.

## <a name="create-basic-load-balancer"></a>Creación de un equilibrador de carga básico

En esta sección se detalla cómo se pueden crear y configurar los componentes siguientes del equilibrador de carga:

  * Un grupo de direcciones IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga.
  * Un grupo de direcciones IP de back-end al que el grupo de servidores front-end envía el tráfico de red de carga equilibrada.
  * Un sondeo de estado que determina el estado de las instancias de máquina virtual de back-end.
  * Una regla de equilibrador de carga que define cómo se distribuye el tráfico a las VM.

### <a name="create-the-load-balancer-resource"></a>Creación del recurso del equilibrador de carga

Cree un equilibrador de carga público con [az network lb create](/cli/azure/network/lb#az-network-lb-create):

* Denominado **myLoadBalancer**.
* Un grupo de front-end denominado **MyFrontEnd**.
* Un grupo de back-end denominado **myBackEndPool**
* Asociado a la red virtual **myVNet**.
* Asociado a la subred de back-end **myBackendSubnet**.

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Basic \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool
```

### <a name="create-the-health-probe"></a>Creación del sondeo de estado

Los sondeos de estado comprueban todas las instancias de máquina virtual para asegurarse de que pueden enviar tráfico de red. 

Una máquina virtual con una comprobación de sondeo con errores se quita del equilibrador de carga. La máquina virtual se agrega de nuevo al equilibrador de carga cuando se resuelve el error.

Cree un sondeo de estado con [az network lb probe create](/cli/azure/network/lb/probe#az-network-lb-probe-create):

* Supervisa el estado de las máquinas virtuales.
* Denominado **myHealthProbe**.
* Protocolo **TCP**.
* **Puerto 80** de supervisión.

```azurecli-interactive
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-the-load-balancer-rule"></a>Creación de la regla de equilibrador de carga

Una regla de equilibrador de carga define:

* La configuración de IP del front-end para el tráfico entrante.
* El grupo de IP de back-end para recibir el tráfico.
* Los puertos de origen y de destino requeridos. 

Cree una regla de equilibrador de carga con [az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create):

* Denominada **myHTTPRule**.
* Que escuche en el **puerto 80** en el grupo de front-end **myFrontEnd**.
* Que envíe el tráfico de red con equilibrio de carga al grupo de direcciones de back-end **myBackEndPool** a través del **Puerto 80**. 
* Mediante el sondeo de estado **myHealthProbe**.
* Protocolo **TCP**.
* Un tiempo de espera de inactividad de **15 minutos**.

```azurecli-interactive
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 
```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Adición de máquinas virtuales al grupo de back-end del equilibrador de carga

Agregue las máquinas virtuales al grupo de back-end con [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add):

* En el grupo de direcciones de back-end **myBackEndPool**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Asociada a la interfaz de red **myNicVM1**, **myNicVM2** y **myNicVM3**.
* Asociada al equilibrador de carga **myLoadBalancer**.

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```
---

## <a name="test-the-load-balancer"></a>Prueba del equilibrador de carga

### <a name="create-test-virtual-machine"></a>Creación de una máquina virtual de prueba

Cree la interfaz de red con [az network nic create](/cli/azure/network/nic#az-network-nic-create):

* Llamada **myNicTestVM**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* En la red virtual **myVNet**.
* En la subred **myBackendSubnet**.
* En el grupo de seguridad de red **myNSG**.

```azurecli-interactive
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicTestVM \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
Cree la máquina virtual con [az vm create](/cli/azure/vm#az-vm-create):

* Llamada **myTestVM**.
* En el grupo de recursos **CreateIntLBQS-rg**.
* Conectada a la interfaz de red **myNicTestVM**.
* Con la imagen de máquina virtual **Win2019Datacenter**.

```azurecli-interactive
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myTestVM \
    --nics myNicTestVM \
    --image Win2019Datacenter \
    --admin-username azureuser \
    --no-wait
```
La implementación de la máquina virtual puede tardar unos minutos.

## <a name="install-iis"></a>Instalación de IIS

Utilice [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set) para instalar IIS en las máquinas virtuales y establecer el nombre del equipo como sitio web predeterminado.

```azurecli-interactive
  array=(myVM1 myVM2 myVM3)
    for vm in "${array[@]}"
    do
     az vm extension set \
       --publisher Microsoft.Compute \
       --version 1.8 \
       --name CustomScriptExtension \
       --vm-name $vm \
       --resource-group CreateIntLBQS-rg \
       --settings '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
  done

```

### <a name="test"></a>Prueba

1. [Inicie sesión](https://portal.azure.com) en Azure Portal.

2. Busque la dirección IP privada del equilibrador de carga en la pantalla **Información general**. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, **myLoadBalancer**.

3. Tome nota o copie la dirección que encontrará junto a **Dirección IP privada**, en la pestaña **Información general** de **myLoadBalancer**.

4. Seleccione **Todos los servicios** en el menú de la izquierda, seleccione **Todos los recursos** y, después, en la lista de recursos, seleccione **myTestVM**, que se encuentra en el grupo de recursos **CreateIntLBQS-rg**.

5. En la página **Introducción**, seleccione **Conectar** y después **Instancia de Bastion**.

6. Escriba el nombre de usuario y la contraseña especificados durante la creación de la máquina virtual.

7. Abra **Internet Explorer** en **myTestVM**.

8. Escriba la dirección IP del paso anterior en la barra de direcciones del explorador. La página predeterminada del servidor web de IIS se muestra en el explorador.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Crear un equilibrador de carga interno estándar" border="true":::
   
Para ver el tráfico distribuido por Load Balancer entre las tres máquinas virtuales, puede personalizar la página predeterminada de cada servidor web IIS de las máquinas virtuales y luego forzar una actualización del explorador web desde el equipo cliente.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, use el comando [az group delete](/cli/azure/group#az-group-delete) para quitar el grupo de recursos, el equilibrador de carga y todos los recursos relacionados.

```azurecli-interactive
  az group delete \
    --name CreateIntLBQS-rg
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido:

* Creó un equilibrador de carga estándar o público.
* Conectó máquinas virtuales. 
* Configuró la regla de tráfico del equilibrador de carga y el sondeo de estado.
* Probó el equilibrador de carga.

Para más información sobre Azure Load Balancer, vaya a:
> [!div class="nextstepaction"]
> [¿Qué es Azure Load Balancer?](load-balancer-overview.md)