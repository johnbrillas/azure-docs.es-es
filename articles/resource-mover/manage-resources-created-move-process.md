---
title: Administración de los recursos creados durante el proceso de movimiento de máquinas virtuales en Azure Resource Mover
description: Aprenda a administrar los recursos que se crean durante el proceso de movimiento de las máquinas virtuales en Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: d3c4c4e86e2461ea1d05af284e724a5a2991f040
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727046"
---
# <a name="manage-resources-created-for-the-vm-move"></a>Administración de recursos creados para el movimiento de máquinas virtuales

En este artículo se describe cómo administrar recursos creados explícitamente por [Azure Resource Mover](overview.md) para facilitar el proceso de movimiento de las máquinas virtuales. 

Después de mover las máquinas virtuales entre regiones, hay una serie de recursos creados por Resource Mover que deben limpiarse manualmente.

## <a name="delete-resources-created-for-vm-move"></a>Eliminación de recursos creados para el movimiento de máquinas virtuales

Elimine manualmente la colección de movimientos y los recursos de Site Recovery creados para el movimiento de máquinas virtuales.

1. Revise los recursos pertenecientes al grupo ```ResourceMoverRG-<sourceregion>-<target-region>-<metadataRegionShortName>```.
2. Compruebe que la máquina virtual y todos los demás recursos de origen de la colección de movimientos se han movido o eliminado. De este modo, se garantiza que no haya recursos pendientes que los utilicen.
2. Elimine estos recursos.

    - El nombre de la colección de traslado es ```movecollection-<sourceregion>-<target-region>-<metadata-region>```.
    - El nombre de la cuenta de almacenamiento en caché es ```resmovecache<guid>```.
    - El nombre del almacén es ```ResourceMove-<sourceregion>-<target-region>-GUID```.

## <a name="next-steps"></a>Pasos siguientes

Pruebe a [migrar una máquina virtual](tutorial-move-region-virtual-machines.md) a otra región con Resource Mover.
