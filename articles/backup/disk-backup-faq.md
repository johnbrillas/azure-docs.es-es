---
title: Preguntas frecuentes sobre Azure Disk Backup
description: Obtenga respuestas a las preguntas más frecuentes sobre Azure Disk Backup
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 3ef18a7d178075194e24889477768583f05f0cdd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98734569"
---
# <a name="frequently-asked-questions-about-azure-disk-backup-in-preview"></a>Preguntas frecuentes sobre Azure Disk Backup (en versión preliminar)

>[!IMPORTANT]
>Azure Disk Backup está en versión preliminar y se ofrece sin Acuerdo de Nivel de Servicio, por lo que no se recomienda para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Para ver la disponibilidad de la región, consulte la [matriz de compatibilidad](disk-backup-support-matrix.md).
>
>[Rellene este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u) para suscribirse y obtener la versión preliminar.

En este artículo se responden las preguntas más frecuentes sobre Azure Disk Backup. Para más información sobre la disponibilidad de la disponibilidad de regiones, los escenarios admitidos y las limitaciones de [Azure Disk Backup](disk-backup-overview.md), consulte la [matriz de compatibilidad](disk-backup-support-matrix.md).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-back-up-the-disk-using-the-azure-disk-backup-solution-if-the-same-disk-is-backed-up-using-azure-virtual-machine-backup"></a>¿Se puede realizar una copia de seguridad del disco mediante la solución Azure Disk Backup si se realiza una copia de seguridad del mismo disco con la copia de seguridad de máquina virtual de Azure?

Azure Backup ofrece compatibilidad en paralelo con la copia de seguridad de los discos administrados mediante las soluciones de copia de seguridad de disco y de [copia de seguridad de máquina virtual de Azure](backup-azure-vms-introduction.md). Esto resulta útil cuando es necesario no solo realizar una copia de seguridad diaria de las máquinas virtuales, sino también copias de seguridad más frecuentes del disco del sistema operativo o de un disco de datos específico que sean coherentes con los bloqueos, sin que ello afecte al rendimiento de la aplicación de producción.

### <a name="how-do-i-find-the-snapshot-resource-group-that-i-used-to-configure-backup-for-a-disk"></a>¿Cómo se buscar el grupo de recursos de instantáneas que se ha usado para configurar la copia de seguridad de un disco?

En la pantalla **Copia de seguridad de instancia**, el campo Snapshot resource group (Grupo de recursos de instantáneas) se encuentra en la sección **Essentials**. Puede buscar y seleccionar la instancia de copia de seguridad del disco correspondiente en el Centro de copia de seguridad o en el almacén de Backup.

![Campo Snapshot resource group (Grupo de recursos de instantáneas)](./media/disk-backup-faq/snapshot-resource-group.png)

### <a name="what-is-a-snapshot-resource-group"></a>¿Qué es un grupo de recursos de instantáneas?

Azure Disk Backup ofrece una copia de seguridad del nivel operativo para discos administrados. Es decir, las instantáneas que se crean durante las operaciones de copia de seguridad programadas y a petición se almacenan en un grupo de recursos de la suscripción. Azure Backup ofrece restauración al instante, ya que las instantáneas incrementales se almacenan en la suscripción. Este grupo de recursos se conoce como el grupo de recursos de instantáneas. Para más información, consulte el apartado sobre la [configuración de copias de seguridad](backup-managed-disks.md#configure-backup).

### <a name="why-must-the-snapshot-resource-group-be-in-same-subscription-as-that-of-the-disk-being-backed-up"></a>¿Por qué debe estar el grupo de recursos de instantáneas en la misma suscripción que el disco del que se realiza la copia de seguridad?

No se puede crear una instantánea incremental de un disco concreto que se encuentre fuera de la suscripción de ese disco. Por consiguiente, elija el grupo de recursos en la misma suscripción que el disco del que se va a realizar la copia de seguridad. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados.

### <a name="why-do-i-need-to-provide-role-assignments-to-be-able-to-configure-backups-perform-scheduled-and-on-demand-backups-and-restore-operations"></a>¿Por qué es necesario proporcionar asignaciones de roles para poder configurar copias de seguridad, realizar copias de seguridad programadas y a petición y operaciones de restauración?

Azure Disk Backup usa el enfoque de privilegios mínimos para detectar, proteger y restaurar los discos administrados de las suscripciones. Para ello, Azure Backup usa la identidad administrada del [almacén de Backup](backup-vault-overview.md) para acceder a otros recursos de Azure. Ningún recurso puede tener más de una identidad administrada asignada por el sistema, y cada una de ellas está asociada al ciclo de vida del recurso. Puede conceder permisos a la identidad administrada mediante el control de acceso basado en roles de Azure (Azure RBAC). Tenga en cuenta que una identidad administrada es una entidad de servicio de un tipo especial que solo se puede usar con recursos de Azure. Obtenga más información sobre las [identidades administradas](../active-directory/managed-identities-azure-resources/overview.md). De forma predeterminada, el almacén de Backup no tendrá permiso para acceder al disco del que se va a realizar la copia de seguridad, crear instantáneas periódicas, eliminar instantáneas después del período de retención ni restaurar un disco a partir de una copia de seguridad. Al conceder de forma explícita asignaciones de roles a la identidad administrada del almacén de Backup, tiene el control de la administración de permisos en los recursos de las suscripciones.

### <a name="why-does-backup-policy-limit-the-retention-duration"></a>¿Por qué motivo la directiva de copia de seguridad limita la duración de la retención?

Azure Disk Backup usa instantáneas incrementales, que se limitan a 200 instantáneas por disco. Para que pueda realizar copias de seguridad a petición además de las copias de seguridad programadas, la directiva de copia de seguridad limita el número total de copias de seguridad a 180. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados.

### <a name="how-does-the-hourly-and-daily-backup-frequency-work-in-the-backup-policy"></a>¿Cómo funciona la frecuencia de copia de seguridad por hora y diaria en la directiva de copia de seguridad?

Azure Disk Backup ofrece varias copias de seguridad al día. Si necesita copias de seguridad más frecuentes, elija la opción **Cada hora**. Las copias de seguridad se programan en función del intervalo de **tiempo** seleccionado. Por ejemplo, si selecciona **Cada 4 horas**, las copias de seguridad se realizan aproximadamente cada 4 horas, por lo que se distribuyen de forma equitativa a lo largo del día. Si basta con realizar una copia de seguridad al día, elija la opción **Diaria**. En la frecuencia de copia de seguridad diaria, puede especificar la hora del día en que se realizarán las copias de seguridad. Es importante tener en cuenta que la hora del día indica la hora de inicio de la copia de seguridad, no la hora en que se completa la misma. El tiempo necesario para completar la operación de copia de seguridad depende de varios factores, como el abandono entre copias de seguridad consecutivas. No obstante, Azure Disk Backup es una opción de copias de seguridad sin agente que usa [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md) que no afectan al rendimiento de la aplicación de producción.

### <a name="why-does-the-backup-vaults-redundancy-setting-not-apply-to-the-backups-stored-in-operational-tier-the-snapshot-resource-group"></a>¿Por qué el valor de redundancia del almacén de Backup no se aplica a las copias de seguridad almacenadas en el nivel operativo (el grupo de recursos de instantáneas)?

Azure Backup usa [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados que solo almacenan los cambios diferenciales en los discos desde la última instantánea en almacenamiento HDD estándar, independientemente del tipo de almacenamiento del disco principal. Para aumentar la confiabilidad, las instantáneas incrementales se guardan de forma predeterminada en el almacenamiento con redundancia de zona (ZRS) en aquellas regiones que lo admiten. Actualmente, Azure Disk Backup admite copias de seguridad operativas de discos administrados que no copian las copias de seguridad en el almacén de Backup. Por consiguiente, el valor de redundancia del almacenamiento de copia de seguridad no se aplica a los puntos de recuperación.

### <a name="can-i-use-backup-center-to-configure-backups-and-manage-backup-instances-for-azure-disks"></a>¿Se puede usar el Centro de copia de seguridad para configurar copias de seguridad y administrar las instancias de copia de seguridad de Azure Disks?

Sí, Azure Disk Backup está integrado con el **Centro de copias de seguridad**, lo que proporciona una [experiencia única de administración unificada](backup-center-overview.md) en Azure para que las empresas controlen, supervisen, administren y analicen las copias de seguridad a gran escala. También puede usar el almacén de Backup para realizar copias de seguridad, restaurar y administrar las instancias de copia de seguridad que están protegidas en el almacén.

### <a name="why-do-i-need-to-create-a-backup-vault-and-not-use-a-recovery-services-vault"></a>¿Por qué es necesario crear un almacén de Backup y no usar un almacén de Recovery Services?

Un almacén de Backup es una entidad de almacenamiento de Azure que hospeda los datos de copia de seguridad para ciertas cargas de trabajo más recientes que Azure Backup admite. Los almacenes de Backup se pueden utilizar para almacenar los datos de las copias de seguridad de varios servicios de Azure, como servidores de Azure Database for PostgreSQL, Azure Disks y las cargas de trabajo más recientes que Azure Backup admitirá. Los almacenes de Backup facilitan la tarea de organizar los datos de copia de seguridad, al mismo tiempo que reducen al mínimo la sobrecarga administrativa. Para más información, consulte [Introducción a almacenes de Backup](./backup-vault-overview.md).

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-subscriptions"></a>¿Pueden está en distintas suscripciones el disco del que se va a realizar una copia de seguridad y el almacén de Backup en distintas suscripciones?

Sí, el disco administrado por el origen del que se va a realizar una copia de seguridad y el almacén de Backup pueden estar en distintas suscripciones.

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-regions"></a>¿Pueden está en distintas regiones el disco del que se va a realizar una copia de seguridad y el almacén de Backup en distintas suscripciones?

No, actualmente el disco administrado por el origen del que se va a realizar una copia de seguridad y el almacén de Backup deben estar en la misma región.

### <a name="can-i-restore-a-disk-into-a-different-subscription"></a>¿Puedo restaurar un disco en una suscripción diferente?

Sí, el disco se puede restaurar en una suscripción diferente a la del disco administrado por origen del que se realiza la copia de seguridad.

### <a name="can-i-back-up-multiple-disks-together"></a>¿Se pueden realizar copias de seguridad de varios discos juntos?

No se admiten las instantáneas en un momento dado de varios discos que estén conectados a una máquina virtual. Para más información, consulte el apartado en el que se explica cómo [configurar](backup-managed-disks.md#configure-backup) copias de seguridad, y para más información acerca de las limitaciones, consulte la [matriz de compatibilidad](disk-backup-support-matrix.md).

### <a name="what-are-my-options-to-back-up-disks-across-multiple-subscriptions"></a>¿Qué opciones tengo si quiero hacer una copia de seguridad de discos en varias suscripciones?

Actualmente, el uso de Azure Portal para configurar la copia de seguridad de discos está limitado a un máximo de 20 discos de la misma suscripción.

### <a name="what-is-a-target-resource-group"></a>¿Qué es un grupo de recursos de destino?

En una operación de restauración, puede elegir la suscripción y un grupo de recursos en el que desea restaurar el disco. Azure Backup creará discos a partir del punto de recuperación del grupo de recursos seleccionado. Este se conoce como grupo de recursos de destino. Tenga en cuenta que la identidad administrada del almacén de Backup requiere que la asignación de roles del grupo de recursos de destino pueda realizar la operación de restauración correctamente. Para más información, consulte la [documentación sobre restauración](restore-managed-disks.md).

### <a name="what-are-the-permissions-used-by-azure-backup-during-backup-and-restore-operation"></a>¿Qué permisos usa Azure Backup durante la operación de copia de seguridad y restauración?

A continuación se indican las acciones que se usan en el rol **Lector de copia de seguridad de disco** asignado al **disco** del que se va a hacer una copia de seguridad:

"Microsoft.Compute/disks/read"

"Microsoft.Compute/disks/beginGetAccess/action"

A continuación se indican las acciones que se usan en el rol **Colaborador de instantáneas de disco** asignado al **grupo de recursos de instantáneas**:

"Microsoft.Compute/snapshots/delete"

"Microsoft.Compute/snapshots/write"

“Microsoft.Compute/snapshots/read"

"Microsoft.Storage/storageAccounts/write"

"Microsoft.Storage/storageAccounts/read"

"Microsoft.Storage/storageAccounts/delete"

"Microsoft.Resources/subscriptions/resourceGroups/read"

"Microsoft.Storage/storageAccounts/listkeys/action"

"Microsoft.Compute/snapshots/beginGetAccess/action"

"Microsoft.Compute/snapshots/endGetAccess/action"

"Microsoft.Compute/disks/beginGetAccess/action"

A continuación, se indican las acciones que se usan en el rol **Colaborador de instantáneas de disco** asignado al **grupo de recursos de instantáneas**:

"Microsoft.Compute/disks/write"

"Microsoft.Compute/disks/read"

"Microsoft.Resources/subscriptions/resourceGroups/read"

>[!NOTE]
>Los permisos de estos roles pueden cambiar en el futuro, en función de las características que agregue el servicio Azure Backup.

## <a name="next-steps"></a>Pasos siguientes

- [Matriz de compatibilidad de Azure Disk Backup](disk-backup-support-matrix.md)