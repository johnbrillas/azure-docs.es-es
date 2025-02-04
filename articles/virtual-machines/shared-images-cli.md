---
title: Creación de galerías de imágenes compartidas con la CLI de Azure
description: En este artículo, aprenderá a usar la CLI de Azure para crear una imagen compartida de una máquina virtual en Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.custom: devx-track-azurecli
ms.openlocfilehash: 991b5363b180651b775bd46a0a1353fd124d34e6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102557735"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Creación de una instancia de Shared Image Gallery con la CLI de Azure

Una [galería de imágenes compartidas](./shared-image-galleries.md) simplifica el uso compartido de imágenes personalizadas en toda una organización. Las imágenes personalizadas son como las imágenes de Marketplace, pero las puede crear usted mismo. Las imágenes personalizadas pueden usarse para configuraciones de arranque como la carga previa de aplicaciones, configuraciones de aplicaciones y otras configuraciones del sistema operativo. 

Shared Image Gallery le permite compartir sus imágenes de máquina virtual personalizadas con otras personas. Elija las imágenes que desea compartir, qué regiones desea que estén disponibles en ellas y con quién desea compartirlas. 

[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]


## <a name="next-steps"></a>Pasos siguientes

Cree una versión de la imagen a partir de una [máquina virtual](image-version-vm-cli.md) o una [imagen administrada](image-version-managed-image-cli.md) mediante la CLI de Azure.

Para más información sobre las galerías de imágenes compartidas, consulte la [Introducción](./shared-image-galleries.md). Si encuentra problemas, consulte [Solución de problemas de galerías de imágenes compartidas](troubleshooting-shared-images.md).