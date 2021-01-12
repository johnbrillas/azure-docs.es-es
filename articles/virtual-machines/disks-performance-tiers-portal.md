---
title: Cambio del rendimiento de los discos administrados de Azure mediante Azure Portal
description: Aprenda a cambiar los niveles de rendimiento de los discos administrados nuevos y existentes desde Azure Portal.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 01/05/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 625fb1e3dd0b433da6b60f995aa6b380c23ec9ce
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901041"
---
# <a name="change-your-performance-tier-using-the-azure-portal"></a>Cambio del nivel de rendimiento mediante Azure Portal

[!INCLUDE [virtual-machines-disks-performance-tiers-intro](../../includes/virtual-machines-disks-performance-tiers-intro.md)]

## <a name="restrictions"></a>Restricciones

[!INCLUDE [virtual-machines-disks-performance-tiers-restrictions](../../includes/virtual-machines-disks-performance-tiers-restrictions.md)]

## <a name="getting-started"></a>Introducción

### <a name="new-disks"></a>Discos nuevos

En los pasos siguientes se muestra cómo cambiar el nivel de rendimiento de un disco al crearlo:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Vaya a la máquina virtual para la que desea crear un disco.
1. Al seleccionar el disco nuevo, en primer lugar elija el tamaño que necesita.
1. Una vez que haya seleccionado el tamaño, seleccione un nivel de rendimiento diferente para cambiar su rendimiento.
1. Seleccione **Aceptar** para crear el disco.

:::image type="content" source="media/disks-performance-tiers-portal/new-disk-change-performance-tier.png" alt-text="Captura de pantalla de la hoja de creación de discos, en la que tanto un disco como la lista desplegable de niveles de rendimiento están resaltados." lightbox="media/disks-performance-tiers-portal/performance-tier-settings.png":::


## <a name="existing-disks"></a>Discos existentes

En los pasos siguientes se describe cómo cambiar el nivel de rendimiento de un disco existente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Vaya a la máquina virtual que contiene el disco que desea cambiar.
1. Desasigne la máquina virtual o desasocie el disco.
1. Seleccione el disco.
1. Seleccione **Tamaño y rendimiento**.
1. En la lista desplegable **Nivel de rendimiento**, seleccione un nivel distinto del nivel de rendimiento actual del disco.
1. Seleccione **Cambiar tamaño**.

:::image type="content" source="media/disks-performance-tiers-portal/change-tier-existing-disk.png" alt-text="Captura de pantalla de la hoja de tamaño y rendimiento, en la que hay un nivel de rendimiento resaltado." lightbox="media/disks-performance-tiers-portal/performance-tier-settings.png":::

## <a name="next-steps"></a>Pasos siguientes

Si necesita cambiar el tamaño de un disco para aprovechar las ventajas de los niveles de rendimiento superiores, consulte estos artículos:

- [Expansión de discos duros virtuales en una VM Linux con la CLI de Azure](linux/expand-disks.md)
- [Expansión de un disco administrado asociado a una máquina virtual de Windows](windows/expand-os-disk.md)
