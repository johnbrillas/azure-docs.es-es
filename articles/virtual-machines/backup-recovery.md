---
title: Introducción a las opciones de copia de seguridad para máquinas virtuales
description: Información general sobre las opciones de copia de seguridad para máquinas virtuales de Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: recovery
ms.topic: conceptual
ms.date: 8/03/2020
ms.author: cynthn
ms.openlocfilehash: c4116ef8d02bd47d6375371be9381553f8c22eda
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102565451"
---
# <a name="backup-and-restore-options-for-virtual-machines-in-azure"></a>Opciones de copia de seguridad y restauración de máquinas virtuales en Azure

Para proteger sus datos realice copias de seguridad a intervalos regulares. Hay varias opciones de copia de seguridad disponibles para máquinas virtuales, según el caso de uso.

## <a name="azure-backup"></a>Azure Backup

Para realizar copias de seguridad de máquinas virtuales de Azure que ejecutan cargas de trabajo de producción, use Azure Backup. Azure Backup admite las copias de seguridad coherentes con la aplicación para máquinas virtuales Windows y Linux. Azure Backup crea puntos de recuperación que se almacenan en almacenes de recuperación con redundancia geográfica. Cuando se realiza una restauración desde un punto de recuperación, se puede restaurar toda una máquina virtual o solo determinados archivos. 

Para obtener una introducción sencilla y práctica sobre Azure Backup para máquinas virtuales de Azure, consulte el [inicio rápido de Azure Backup](../backup/quick-backup-vm-portal.md).

Para más información sobre cómo funciona Azure Backup, consulte [Planeamiento de la infraestructura de copia de seguridad de máquinas virtuales en Azure](../backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery permite proteger las máquinas virtuales en un escenario grave de desastre en el que toda una región experimenta una interrupción debido a un desastre natural importante o a una interrupción del servicio generalizada. Puede configurar Azure Site Recovery para las VM de modo que pueda recuperar la aplicación con un solo clic en cuestión de minutos. Puede realizar replicaciones en la región de Azure que elija, no solo en regiones emparejadas. 

Puede ejecutar maniobras de recuperación ante desastres con conmutaciones por error de prueba a petición, sin que las cargas de trabajo de producción o la replicación en curso se vean afectadas. Cree planes de recuperación para orquestar la conmutación por error y la conmutación por recuperación de toda la aplicación que se ejecuta en varias máquinas virtuales. La característica de los planes de recuperación está integrada con runbooks de Azure Automation.

Puede empezar por [replicar las máquinas virtuales](../site-recovery/azure-to-azure-quickstart.md). 

## <a name="managed-snapshots"></a>Instantáneas administradas 

En entornos de desarrollo y prueba, las instantáneas proporcionan una opción rápida y sencilla para realizar copias de seguridad de máquinas virtuales que usan Managed Disks. Una instantánea administrada es una copia completa de solo lectura de un disco administrado. Las instantáneas existen independientemente del disco de origen y se pueden usar para recompilar una máquina virtual. Se facturan según la porción usada del disco. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GB y el tamaño de datos usado real es de 10 GB, solo se le cobrará por el tamaño de datos usado de 10 GB.  

Para más información sobre la creación de instantáneas, consulte:

* [Creación de una copia del disco duro virtual que se almacene como un disco administrado mediante instantáneas en Windows](./windows/snapshot-copy-managed-disk.md)
* [Creación de una copia del disco duro virtual que se almacene como un disco administrado mediante instantáneas en Linux](./linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Pasos siguientes
Puede probar Azure Backup en el [inicio rápido de Azure Backup](../backup/quick-backup-vm-portal.md).