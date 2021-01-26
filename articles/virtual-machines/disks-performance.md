---
title: Rendimiento de la máquina virtual y del disco
description: Obtenga más información sobre cómo funcionan las máquinas virtuales y sus discos conectados en combinación con el fin de obtener rendimiento.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 5e9f6ecc733eccf317e3013752ee2f5b0586ea78
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540418"
---
# <a name="virtual-machine-and-disk-performance"></a>Rendimiento de la máquina virtual y del disco
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
Las máquinas virtuales que están habilitadas tanto para Premium Storage como para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Ahora se examinará la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](dv3-dsv3-series.md) y la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

Se ejecutará una prueba comparativa en esta combinación de máquina virtual y disco que crea la actividad de E/S. Para obtener información sobre cómo realizar pruebas comparativas de E/S de almacenamiento en Azure, vea [Pruebas comparativas de la aplicación en Azure Disk Storage](disks-benchmarks.md). Desde la herramienta de pruebas comparativas, puede ver que la combinación de discos y máquinas virtuales es capaz de lograr 22 800 IOPS:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-3.md)]
