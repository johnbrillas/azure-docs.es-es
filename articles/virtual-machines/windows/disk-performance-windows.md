---
title: Rendimiento de la máquina virtual y del disco - Windows
description: Obtenga más información sobre cómo funcionan las máquinas virtuales y sus discos conectados en combinación con el fin de obtener un rendimiento en Windows.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: dec8b0cc33a9fffa7cac1ac9261b3c38ef5a6449
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97584126"
---
# <a name="virtual-machine-and-disk-performance-windows"></a>Rendimiento de la máquina virtual y del disco (Windows)
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
 Las máquinas virtuales que están habilitadas para Premium Storage y para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Seguiremos examinando la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](../dv3-dsv3-series.md) y en ella se describe lo siguiente sobre la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Vamos a ejecutar una prueba comparativa en esta combinación de discos y máquinas virtuales que creará actividad de E/S; podrá obtener información sobre cómo realizar pruebas comparativas de E/S de almacenamiento en Azure [aquí](disks-benchmarks.md). Desde la herramienta de pruebas comparativas, puede ver que la combinación de discos y máquinas virtuales es capaz de lograr 22 800 IOPS:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]