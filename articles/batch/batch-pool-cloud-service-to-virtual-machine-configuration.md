---
title: Migración de la configuración del grupo de Batch de Cloud Services a Virtual Machines
description: Obtenga información acerca de cómo actualizar la configuración del grupo a la configuración más reciente y recomendada.
ms.topic: how-to
ms.date: 1/6/2021
ms.openlocfilehash: d987a185efb6593fd541dd14fa74b6c4d3ca41be
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98234314"
---
# <a name="migrate-batch-pool-configuration-from-cloud-services-to-virtual-machines"></a>Migración de la configuración del grupo de Batch de Cloud Services a Virtual Machines

Los grupos de Batch se pueden crear mediante [cloudServiceConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#cloudserviceconfiguration) o [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration). "virtualMachineConfiguration" es la configuración recomendada, ya que es compatible con todas las funcionalidades de Batch. Los grupos "cloudServiceConfiguration" no admiten todas las características y no se han planeado nuevas características.

Si usa grupos "cloudServiceConfiguration", se recomienda encarecidamente que se mueva para usar grupos "virtualMachineConfiguration". Esto le permitirá beneficiarse de todas las funcionalidades de Batch, como una ampliada [selección de serie de máquinas virtuales](batch-pool-vm-sizes.md), máquinas virtuales de Linux, [contenedores](batch-docker-container-workloads.md), [redes virtuales de Azure Resource Manager](batch-virtual-network.md) y [de cifrado de discos de nodo](disk-encryption.md).

En este artículo se describe cómo migrar a "virtualMachineConfiguration".

## <a name="new-pools-are-required"></a>Se requieren nuevos grupos

Los grupos activos existentes no se pueden actualizar de "cloudServiceConfiguration" a "virtualMachineConfiguration"; se deben crear nuevos grupos. La creación de grupos con "virtualMachineConfiguration" es compatible con todas las API de Batch, herramientas de línea de comandos, Azure Portal y la interfaz de usuario de Batch Explorer.

**Los tutoriales de [.NET](tutorial-parallel-dotnet.md) y [Python](tutorial-parallel-python.md) proporcionan ejemplos de creación de grupos con "virtualMachineConfiguration"** .

## <a name="pool-configuration-differences"></a>Diferencias de configuración de grupos

Al actualizar la configuración del grupo, debe tener en cuenta lo siguiente:

- Los nodos de grupo "cloudServiceConfiguration" siempre son sistema operativo Windows, mientras que los grupos "virtualMachineConfiguration" pueden ser sistema operativo Windows o Linux.
- En comparación con los grupos "cloudServiceConfiguration", los grupos "virtualMachineConfiguration" tienen un conjunto más completo de funcionalidades, como la compatibilidad con contenedores, los discos de datos y el cifrado de discos.
- Los nodos del grupo "virtualMachineConfiguration" usan discos de sistema operativo administrados. El [tipo de disco administrado](../virtual-machines/disks-types.md) que se usa para cada nodo depende del tamaño de máquina virtual elegido para el grupo. Si se especifica un tamaño de máquina virtual "s" para el grupo, por ejemplo, "Standard_D2s_v3", se usa una SSD Premium. Si se especifica un tamaño de máquina virtual "no s", por ejemplo, "Standard_D2_v3", se usa una HDD estándar.

   > [!IMPORTANT]
   > Al igual que con Virtual Machines y Virtual Machine Scale Sets, el disco administrado del SO que se usa para cada nodo conlleva un costo, que es adicional al costo de las máquinas virtuales. No hay ningún costo de disco del sistema operativo para los nodos de "cloudServiceConfiguration", ya que el disco del sistema operativo se crea en el SSD local de los nodos.

- Los tiempos de inicio y eliminación de grupos y nodos pueden diferir ligeramente entre los grupos "cloudServiceConfiguration" y "virtualMachineConfiguration".

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [configuraciones de grupo](nodes-and-pools.md#configurations).
- Obtenga más información sobre los [procedimientos recomendados de los grupos](best-practices.md#pools).
- Referencia de la API REST para la [adición de grupos](https://docs.microsoft.com/rest/api/batchservice/pool/add) y [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration).
