---
title: Creación de una máquina virtual a partir de una imagen generalizada mediante la CLI de Azure
description: Cree una máquina virtual a partir de una versión de imagen generalizada mediante la CLI de Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.custom: devx-track-azurecli
ms.openlocfilehash: 2bcaf85f61a4d8cf4d23c9c5be7f46d765d77dbb
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102551049"
---
# <a name="create-a-vm-from-a-generalized-image-version-using-the-cli"></a>Creación de una máquina virtual a partir de una versión de imagen generalizada mediante la CLI

Cree una máquina virtual a partir de una versión de imagen generalizada almacenada en [Shared Image Gallery](./shared-image-galleries.md#generalized-and-specialized-images). Si desea crear una máquina virtual mediante una imagen especializada, consulte [Creación de una máquina virtual a partir de una imagen especializada](vm-specialized-image-version-powershell.md). 


## <a name="get-the-image-id"></a>Obtención del identificador de la imagen

Enumere las definiciones de imagen en una galería mediante [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list) para ver el nombre y el identificador de las definiciones.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list --resource-group $resourceGroup --gallery-name $gallery --query "[].[name, id]" --output tsv
```

## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree una máquina virtual mediante [az vm create](/cli/azure/vm#az-vm-create). Para usar la versión más reciente de la imagen, establezca `--image` en el identificador de la definición de la imagen. 

Reemplace los nombres de los recursos según sea necesario en este ejemplo. 

```azurecli-interactive 
imgDef="/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition"
vmResourceGroup=myResourceGroup
location=eastus
vmName=myVM
adminUsername=azureuser


az group create --name $vmResourceGroup --location $location

az vm create\
   --resource-group $vmResourceGroup \
   --name $vmName \
   --image $imgDef \
   --admin-username $adminUsername \
   --generate-ssh-keys
```

También puede usar una versión específica con el identificador de la versión de imagen del parámetro `--image`. Por ejemplo, para usar la versión de imagen *1.0.0*, escriba: `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`.

## <a name="next-steps"></a>Pasos siguientes

[Azure Image Builder (versión preliminar)](./image-builder-overview.md) puede ayudar a automatizar la creación de versiones de la imagen, incluso se puede usar para actualizar y [crear una nueva versión de la imagen a partir de una versión de imagen existente](./linux/image-builder-gallery-update-image-version.md).