---
title: Uso compartido de imágenes de la galería entre inquilinos
description: Obtenga información sobre cómo crear conjuntos de escalado con imágenes que se comparten entre los inquilinos de Azure mediante instancias de Shared Image Gallery.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: imaging
ms.date: 04/05/2019
ms.reviewer: akjosh
ms.custom: akjosh, devx-track-azurecli
ms.openlocfilehash: ea61b3bd76fc4ada48a8a2fb734a841b8a969272
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91323488"
---
# <a name="share-images-across-tenants-with-shared-image-gallery"></a>Imágenes compartidas entre inquilinos con Shared Image Gallery

[!INCLUDE [virtual-machines-share-images-across-tenants](../../includes/virtual-machines-share-images-across-tenants.md)]


## <a name="create-a-scale-set-using-azure-cli"></a>Creación de un conjunto de escalado con la CLI de Azure

Inicie sesión en la entidad de servicio del inquilino 1 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 1. Puede usar `az account show --query "tenantId"` para obtener el identificador del inquilino, si es necesario.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Inicie sesión en la entidad de servicio del inquilino 2 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Cree el conjunto de escalado. Reemplace la información del ejemplo por la suya.

```azurecli-interactive
az vmss create \
  -g myResourceGroup \
  -n myScaleSet \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Pasos siguientes

Si experimenta algún problema, puede [solucionar problemas de galerías de imágenes compartidas](../virtual-machines/troubleshooting-shared-images.md).
