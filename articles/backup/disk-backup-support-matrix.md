---
title: Matriz de compatibilidad de Azure Disk Backup
description: Proporciona un resumen de opciones de compatibilidad y limitaciones para el servicio Azure Disk Backup.
ms.topic: conceptual
ms.date: 01/07/2021
ms.custom: references_regions
ms.openlocfilehash: 922bd532275cdd6b09df83b0e9d36fdec1da0b47
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101707224"
---
# <a name="azure-disk-backup-support-matrix-in-preview"></a>Matriz de compatibilidad de Azure Disk Backup (en versión preliminar)

>[!IMPORTANT]
>Azure Disk Backup está en su versión preliminar y se ofrece sin Acuerdo de Nivel de Servicio, por lo que no es aconsejable usarlo para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
>[Rellene este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u) para suscribirse a la versión preliminar.

Puede usar [Azure Backup](./backup-overview.md) para proteger los discos de Azure. En este artículo se resume la disponibilidad por regiones, los escenarios admitidos y las limitaciones.

## <a name="supported-regions"></a>Regiones admitidas

Azure Disk Backup está disponible en versión preliminar en las siguientes regiones: Oeste de EE. UU., Centro-oeste de EE. UU., Este de EE. UU. 2, Centro de Canadá, Oeste de Reino Unido, Norte de Suiza, Oeste de Suiza, Centro de Australia, Centro de Australia 2, Centro de Corea del Sur, Sur de Corea del Sur, Japón Occidental, Este de Asia, Norte de Emiratos Árabes Unidos, Sur de Brasil, Centro de la India. 

Se anunciarán más regiones cuando estén disponibles.

## <a name="limitations"></a>Limitaciones

- Azure Disk Backup es compatible con Azure Managed Disks, incluidos los discos compartidos (SSD prémium compartidas). No se admiten los discos no administrados. Actualmente, esta solución no es compatible con Discos Ultra, incluidos los Discos Ultra compartidos, debido a la falta de capacidad de instantáneas.

- Azure Disk Backup admite la copia de seguridad del disco Acelerador de escritura. Sin embargo, durante la restauración, el disco se restaurará como un disco normal. La memoria caché de escritura acelerada se puede habilitar en el disco después de montarla en una máquina virtual.

- Azure Backup proporciona una copia de seguridad de nivel operativo (instantánea) de los discos Azure Managed Disks con compatibilidad para varias copias de seguridad al día. Las copias de seguridad no se copian en el almacén de copia de seguridad.

- Actualmente, no se admite la opción Recuperación de la ubicación original (OLR) de la restauración mediante la sustitución de los discos de origen desde los que se realizaron las copias de seguridad. Puede realizar la restauración desde un punto de recuperación para crear un nuevo disco en el mismo grupo de recursos que el disco de origen desde el que se realizaron las copias de seguridad o en cualquier otro grupo de recursos. Esto se conoce como recuperación de ubicación alternativa (ALR).

- Azure Backup para Managed Disks usa instantáneas incrementales que se limitan a 200 instantáneas por disco. Para que pueda realizar copias de seguridad a petición además de las copias de seguridad programadas, la directiva de copia de seguridad limita el número total de copias de seguridad a 180. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados.

- Los [límites de servicio y suscripción de Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-machine-disk-limits) se aplican al número total de instantáneas de disco por región y suscripción.

- No se admiten las instantáneas en un momento dado de varios discos que estén conectados a una máquina virtual.

- El almacén Backup y los discos de los que se va a hacer una copia de seguridad pueden estar en la misma suscripción o en suscripciones diferentes. Sin embargo, el almacén de Backup y el disco del que se va a realizar la copia de seguridad deben estar en la misma región.

- Los discos de los que se va a realizar una copia de seguridad y el grupo de recursos de instantáneas en el que se almacenan localmente las instantáneas deben estar en la misma suscripción.

- Se admite la restauración de un disco a partir de una copia de seguridad en la misma suscripción o en otra diferente. Sin embargo, el disco restaurado se creará en la misma región de la instantánea.

- Los discos cifrados por ADE no se pueden proteger.

- Se admiten los discos cifrados con claves administradas por la plataforma o claves administradas por el cliente. Sin embargo, se producirá un error en la restauración de los puntos de restauración de un disco cifrado mediante claves administradas por el cliente (CMK) si la clave KeyVault del conjunto de cifrado de disco está deshabilitada o eliminada.

- Actualmente, la directiva de Backup no se puede modificar y el grupo de recursos de instantáneas que se asigna a una instancia de copia de seguridad cuando se configura la copia de seguridad de un disco no se puede cambiar.

- Actualmente, la experiencia de Azure Portal para configurar la copia de seguridad de discos está limitada a un máximo de 20 discos de la misma suscripción.

- Actualmente (durante la versión preliminar), no se admite el uso de PowerShell y la CLI de Azure para configurar la copia de seguridad y la restauración de discos.

- Al configurar la copia de seguridad, el disco seleccionado para realizar la copia de seguridad y el grupo de recursos de instantáneas en el que se almacenarán las instantáneas deben formar parte de la misma suscripción. No puede crear una instantánea incremental de un disco determinado fuera de la suscripción de ese disco. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) del disco administrado. Para obtener más información sobre cómo elegir un grupo de recursos de instantáneas, vea [Configuración de la copia de seguridad](backup-managed-disks.md#configure-backup).

- Para que las operaciones de copia de seguridad y restauración se completen correctamente, la identidad administrada del almacén de Backup requiere las asignaciones de roles. Use solo las definiciones de roles que se proporcionan en la documentación. No se admite el uso de otros roles como propietario, colaborador, etc. Puede encontrar problemas de permisos si inicia la configuración de operaciones de copia de seguridad o restauración poco después de asignar roles. Esto se debe a que las asignaciones de roles tardan unos minutos en surtir efecto.

- Managed Disks permite cambiar el nivel de rendimiento en la implementación o después sin cambiar el tamaño del disco. La solución Azure Disk Backup admite los cambios del nivel de rendimiento en el disco de origen del que se está haciendo la copia de seguridad. Durante la restauración, el nivel de rendimiento del disco restaurado será el mismo que el del disco de origen en el momento de la copia de seguridad. Siga la documentación [aquí](../virtual-machines/disks-performance-tiers-portal.md) para cambiar el nivel de rendimiento del disco después de la operación de restauración.

- La compatibilidad de las instancias de [Private Link](../virtual-machines/disks-enable-private-links-for-import-export-portal.md) con discos administrados permite restringir la exportación e importación de los discos administrados, con el fin de que solo se produzca dentro de una red virtual de Azure. Azure Disk Backup admite la copia de seguridad de discos que tienen habilitados los puntos de conexión privados. Esto no incluye los datos de la copia de seguridad ni las instantáneas que sean accesibles a través del punto de conexión privado.

- Durante la versión preliminar, no puede deshabilitar la copia de seguridad, por lo que la opción **Stop backup and retain backup data** (Detener copia de seguridad y retener datos de copia de seguridad) no se admite. Puede eliminar una instancia de copia de seguridad, que no solo detendrá la copia de seguridad, sino que también eliminará todos los datos de copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes

- [Copia de seguridad de Azure Managed Disks](backup-managed-disks.md)