---
title: 'Rendimiento de la máquina virtual y del disco: Linux'
description: Obtenga más información sobre cómo funcionan las máquinas virtuales y sus discos conectados en combinación con el fin de obtener un rendimiento en Linux.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 2c48672bcfd8c552b36e3ae0807135924669c1d9
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591861"
---
# <a name="virtual-machine-and-disk-performance-linux"></a>Rendimiento de la máquina virtual y del disco (Linux)
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
Las máquinas virtuales que están habilitadas tanto para Premium Storage como para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Ahora se examinará la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](../dv3-dsv3-series.md) y la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Se ejecutará una prueba comparativa en esta combinación de máquina virtual y disco que crea la actividad de E/S. Para obtener información sobre cómo realizar pruebas comparativas de E/S de almacenamiento en Azure, vea [Pruebas comparativas de la aplicación en Azure Disk Storage](disks-benchmarks.md). Desde la herramienta de pruebas comparativas, puede ver que la combinación de discos y máquinas virtuales es capaz de lograr 22 800 IOPS:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
