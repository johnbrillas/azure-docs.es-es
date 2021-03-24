---
title: Rendimiento de la máquina virtual y del disco
description: Obtenga más información sobre cómo funcionan las máquinas virtuales y sus discos conectados en combinación con el fin de obtener rendimiento.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: d3a89e7733cc033792056b8de5232232b8327025
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100580375"
---
# <a name="virtual-machine-and-disk-performance"></a>Rendimiento de la máquina virtual y del disco
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
Las máquinas virtuales que están habilitadas tanto para Premium Storage como para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Ahora se examinará la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](dv3-dsv3-series.md) y la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

