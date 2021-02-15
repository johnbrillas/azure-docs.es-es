---
title: Seguridad de disco administrado
description: Obtenga información sobre la expansión de disco para discos de Azure y para máquinas virtuales de Azure
author: albecker1
ms.author: albecker
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: dcdbf94e547581cb9ff885ac5896467abdf316ae
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99576200"
---
# <a name="managed-disk-bursting"></a>Seguridad de disco administrado
[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Seguridad en el nivel de máquina virtual
La expansión a nivel de VM está habilitada en las siguientes series de VM en todas las regiones en las que se admiten:
- [Serie Lsv2](lsv2-series.md)
- [Serie Dsv3](dv3-dsv3-series.md)
- [Serie Esv3](ev3-esv3-series.md)

La seguridad está habilitada de forma predeterminada para las máquinas virtuales que la admiten.

## <a name="disk-level-bursting"></a>Seguridad en el nivel de disco
La seguridad también está disponible en las unidades [SSD Premium](disks-types.md#premium-ssd) para tamaños de disco P20 e inferiores en todas las regiones de la nube pública de Azure, la nube de administración pública y la nube de China. La ampliación del disco está habilitada de forma predeterminada en las implementaciones nuevas y actuales de los tamaños del disco que la admiten. 

[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting-2.md)]
