---
title: Creación de un grupo con ubicación por proximidad mediante la CLI de Azure
description: Obtenga información sobre la creación y el uso de los grupos de selección de ubicación de proximidad para máquinas virtuales en Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: proximity-placement-groups
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 3/8/2021
ms.author: cynthn
ms.openlocfilehash: a347c9284608340811f9c2388df26129baeb8837
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102505654"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Implementación de máquinas virtuales en grupos de selección de ubicación de proximidad con la CLI de Azure

Para acercar las máquinas virtuales lo máximo posible con la menor latencia, debe implementarlas dentro de un [grupo de selección de ubicación de proximidad](../co-location.md#proximity-placement-groups).

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.


## <a name="create-the-proximity-placement-group"></a>Creación de un grupo de selección de ubicación de proximidad
Cree un grupo de selección de ubicación de proximidad con [`az ppg create`](/cli/azure/ppg#az-ppg-create). 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Enumeración de los grupos de selección de ubicación de proximidad

Puede enumerar todos los grupos de selección de ubicación de proximidad con [az ppg list](/cli/azure/ppg#az-ppg-list).

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Crear una VM

Cree una máquina virtual dentro del grupo de selección de ubicación de proximidad con [new az vm](/cli/azure/vm#az-vm-create).

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Puede ver la máquina virtual en el grupo de selección de ubicación de proximidad mediante [az ppg show](/cli/azure/ppg#az-ppg-show).

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Conjuntos de disponibilidad
También puede crear un conjunto de disponibilidad en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `--ppg` con [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create) para crear un conjunto de disponibilidad, y todas las máquinas virtuales del conjunto de disponibilidad también se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="scale-sets"></a>Conjuntos de escalado

También puede crear un conjunto de escalado en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `--ppg` con [az vmss create](/cli/azure/vmss#az_vmss_create) para crear un conjunto de escalado, y todas las instancias se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los comandos de la [CLI de Azure](/cli/azure/ppg) para los grupos de selección de ubicación de proximidad.