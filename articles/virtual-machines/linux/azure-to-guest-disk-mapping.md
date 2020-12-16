---
title: Asignación de discos de Azure a discos invitados de una máquina virtual Linux
description: Determinación de los discos de Azure que subyacen a los discos invitados de una máquina virtual Linux.
author: timbasham
ms.service: virtual-machines-linux
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/17/2020
ms.author: tibasham
ms.openlocfilehash: 4f0e48bf1c14728c54d4e89f30700017b0420d7d
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523367"
---
# <a name="how-to-map-azure-disks-to-linux-vm-guest-disks"></a>Asignación de discos de Azure a discos invitados de una máquina virtual Linux

Es posible que tenga que determinar los discos de Azure que respaldan a los discos invitados de una máquina virtual. En algunos escenarios, puede comparar el tamaño del disco o del volumen con el tamaño de los discos de Azure conectados. En los escenarios en que hay varios discos de Azure del mismo tamaño conectados a la máquina virtual, debe usar el número de unidad lógica (LUN) de los discos de datos. 

## <a name="what-is-a-lun"></a>¿Qué es un número de unidad lógica?

Un número de unidad lógica (LUN) es un número que se usa para identificar un dispositivo de almacenamiento específico. A cada dispositivo de almacenamiento se le asigna un identificador numérico único, comenzando en el cero. La ruta de acceso completa a un dispositivo se representa mediante el número de bus, el número de identificador de destino y el número de unidad lógica (LUN). 

Por ejemplo: ***Número de bus 0, Identificador de destino 0, LUN 3** _

En nuestro ejercicio, solo tiene que usar el LUN.

## <a name="finding-the-lun"></a>Búsqueda del LUN

A continuación se muestran dos métodos para buscar el LUN de un disco en Linux.

### <a name="lsscsi"></a>lsscsi

1. Conexión a la máquina virtual
1. `sudo lsscsi`

La primera columna que se muestra contendrá el LUN, el formato es [Host:Canal:Destino: _ *LUN**].

### <a name="listing-block-devices"></a>Lista de dispositivos de bloqueo

1. Conexión a la máquina virtual
1. `sudo ls -l /sys/block/*/device`

La última columna que se muestra contendrá el LUN, el formato es [Host:Canal:Destino:**LUN**]

## <a name="finding-the-lun-for-the-azure-disks"></a>Búsqueda del LUN en los discos de Azure

Para buscar el LUN de un disco de Azure se usan Azure Portal o la CLI de Azure.

### <a name="finding-an-azure-disks-lun-in-the-azure-portal"></a>Búsqueda del LUN de un disco de Azure en Azure Portal

1. En Azure Portal, seleccione "Máquinas virtuales" para mostrar una lista de sus máquinas virtuales
1. Seleccione la máquina virtual
1. Seleccione "Discos"
1. Seleccione un disco de datos en la lista de discos conectados.
1. El LUN del disco se mostrará en el panel de detalles del disco. El LUN que se muestra aquí se correlaciona con los que buscó en el invitado mediante lsscsi, o en la lista de dispositivos de bloqueo.

### <a name="finding-an-azure-disks-lun-using-azure-cli"></a>Búsqueda del LUN de un disco de Azure mediante la CLI de Azure

```azurecli-interactive
az vm show -g myResourceGroup -n myVM --query "storageProfile.dataDisks"
```
