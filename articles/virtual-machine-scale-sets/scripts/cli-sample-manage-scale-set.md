---
title: Ejemplo de la CLI de Azure para la administración de un conjunto de escalado de máquinas virtuales
description: En este ejemplo se muestra cómo agregar discos a un conjunto de escalado de máquinas virtuales. Puede actualizar los discos y agregar las máquinas virtuales a la autenticación de Azure AD.
author: mimckitt
ms.author: mimckitt
ms.date: 02/04/2021
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1abdf7ae15753d78ac8728f57e9b0cd5dcd9165e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101672609"
---
# <a name="create-and-manage-virtual-machine-scale-set"></a>Creación y administración de un conjunto de escalado de máquinas virtuales

Use estos comandos de ejemplo para establecer prototipos de un conjunto de escalado de máquinas virtuales mediante la CLI de Azure.

Estos comandos de ejemplo demuestran las siguientes operaciones:

* Crea un conjunto de escalado de máquinas virtuales.
* Adición y actualización de discos nuevos o existentes a un conjunto de escalado o a una instancia de este.
* Adición de un conjunto de escalado a la autenticación de Azure Active Directory.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="sample-commands"></a>Comandos de ejemplo

```azurecli
# Create a resource group
az group create --name MyResourceGroup --location eastus

# Create virtual machine scale set
az vmss create --resource-group MyResourceGroup --name myScaleSet --instance-count 2 \
    --image UbuntuLTS --upgrade-policy-mode automatic --admin-username azureuser \
    --generate-ssh-keys

# Attach a new managed disk to your scale set
az vmss disk attach --resource-group MyResourceGroup --vmss-name myScaleSet --size-gb 50
```

Después de agregar un nuevo disco de datos, formatee y monte el disco. En el caso de máquinas virtuales Windows, consulte [Conexión de un disco de datos administrado a una máquina virtual Windows en Azure Portal](../../virtual-machines/windows/attach-managed-disk-portal.md). En el caso de máquinas virtuales Linux, consulte [Adición de un disco a una máquina virtual de Linux](../../virtual-machines/linux/add-disk.md).

```azurecli
# Attach an existing managed disk to a virtual machine instance in your scale set
az vmss disk attach --resource-group MyResourceGroup --disk myDataDisk \
    --vmss-name myScaleSet --instance-id 0

# See the instances in your virtual machine scale set
az vmss list-instances --resource-group MyResourceGroup --name myScaleSet --output table

# See the disks for your virtual machine
az disk list --resource-group MyResourceGroup \
    --query "[*].{Name:name,Gb:diskSizeGb,Tier:accountType}" --output table

# Deallocate the virtual machine
az vmss deallocate --resource-group MyResourceGroup --name myScaleSet --instance-ids 0 

# Resize the disk
az disk update --resource-group MyResourceGroup --name myDataDisk --size-gb 200

# Restart the disk
az vmss restart --resource-group MyResourceGroup --name myScaleSet --instance-ids 0
```

Para usar el disco expandido, expanda la partición subyacente. Para más información, consulte [Expansión de una partición de disco y del sistema de archivos](../../virtual-machines/linux/expand-disks.md#expand-a-disk-partition-and-filesystem).

En este ejemplo se ha cambiado el tamaño de un disco de datos. Puede usar el mismo procedimiento para actualizar un disco del sistema operativo. Para más información sobre máquinas virtuales de Windows, consulte [Cómo ampliar la unidad de sistema operativo de una máquina virtual](../../virtual-machines/windows/expand-os-disk.md). Para más información sobre máquinas virtuales Linux, consulte [Expansión de discos duros virtuales en una VM Linux con la CLI de Azure](../../virtual-machines/linux/expand-disks.md).

```azurecli
# Enable managed service identity on your scale set. This is required to authenticate and interact with other Azure services using bearer tokens.
az vmss identity assign --resource-group MyResourceGroup --name myScaleSet --role Owner \
    --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup

# Connect to Azure AD authentication
az vmss extension set --resource-group MyResourceGroup --name AADLoginForWindows \
    --publisher Microsoft.Azure.ActiveDirectory --vmss-name myScaleSet

# Upgrade one instance of a scale set virtual machine
az vmss update-instances --resource-group MyResourceGroup --name myScaleSet --instance-ids 0 

# Remove a managed disk from the scale set
az vmss disk detach --resource-group MyResourceGroup --vmss-name myScaleSet --lun 0

# Remove a managed disk from an instance
az vmss disk detach --resource-group MyResourceGroup --vmss-name myScaleSet --instance-id 1 --lun 0

# Delete the pre-existing disk
az disk delete --resource-group MyResourceGroup --disk myDataDisk
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de usar estos comandos, ejecute el siguiente comando para quitar el grupo de recursos y todos los recursos asociados a él.

```azurecli
az group delete --name MyResourceGroup
```

## <a name="azure-cli-references-used-in-this-article"></a>Referencias a la CLI de Azure que se usan en este artículo

* [az disk delete](/cli/azure/disk#az_disk_delete)
* [az disk list](/cli/azure/disk#az_disk_list)
* [az disk update](/cli/azure/disk#az_disk_update)
* [az group create](/cli/azure/group#az_group_create)
* [az vmss create](/cli/azure/vmss#az_vmss_create)
* [az virtual machine scale set deallocate](/cli/azure/vmss#az_vmss_deallocate)
* [az vmss disk attach](/cli/azure/vmss/disk#az_vmss_disk_attach)
* [az vmss disk detach](/cli/azure/vmss/disk#az_vmss_disk_detach)
* [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set)
* [az vmss identity assign](/cli/azure/vmss/identity#az_vmss_identity_assign)
* [az vmss list-instances](/cli/azure/vmss#az_vmss_list_instances)
* [az vmss restart](/cli/azure/vmss#az_vmss_restart)
* [az vmss update-instances](/cli/azure/vmss#az_vmss_update_instances)