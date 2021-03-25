---
title: Preguntas frecuentes sobre Azure Resource Mover
description: Obtenga las respuestas a las preguntas más frecuentes sobre Azure Resource Mover.
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 02/21/2021
ms.author: raynew
ms.openlocfilehash: e900250aea84b4a9c9112fa54632a2be8b9cb49c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102564278"
---
# <a name="common-questions"></a>Preguntas frecuentes

En este artículo se responden a las preguntas más frecuentes sobre [Azure Resource Mover](overview.md).


## <a name="moving-across-regions"></a>Movimiento entre regiones

### <a name="can-i-move-resources-across-any-regions"></a>¿Puedo mover recursos entre cualquier región?

Actualmente, puede mover recursos de cualquier región pública de origen a cualquier región pública de destino, en función de los [tipos de recursos disponibles en esa región](https://azure.microsoft.com/global-infrastructure/services/). Actualmente no se admite el movimiento de recursos en regiones de Azure Government.

### <a name="what-regions-are-currently-supported"></a>¿Qué regiones se admiten actualmente?

Azure Resource Mover está disponible de la siguiente manera actualmente:

**Soporte técnico** | **Detalles**
--- | ---
Compatibilidad con el movimiento | Los recursos de Azure que se admiten para mover con Resource Mover se pueden mover desde cualquier región pública a otra región pública.
Compatibilidad con metadatos |  Entre las regiones admitidas para almacenar metadatos sobre las máquinas que se van a migrar se incluyen Este de EE. UU. 2, Norte de Europa, Sudeste Asiático, Japón Oriental, Sur de Reino Unido y Este de Australia como regiones de metadatos. <br/><br/> El traslado de recursos dentro de la región de Azure China también es compatible con la región de metadatos Norte de China 2.

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>¿Qué recursos puedo mover entre regiones con Resource Mover?

Actualmente, con Resource Mover puede mover los siguientes recursos entre regiones:

- Máquinas virtuales de Azure y discos asociados
- NIC
- Conjuntos de disponibilidad 
- Redes virtuales de Azure 
- Direcciones IP públicas
- Grupos de seguridad de red (NSG)
- Equilibradores de carga internos y públicos 
- Bases de datos de Azure SQL y grupo de bases de datos elásticas

### <a name="can-i-move-disks-across-regions"></a>¿Se pueden mover discos entre regiones?

No se pueden seleccionar discos como recursos para moverlos entre regiones. Sin embargo, los discos se pueden mover como parte del desplazamiento de una máquina virtual.

### <a name="what-does-it-mean-to-move-a-resource-group"></a>¿Qué significa mover un grupo de recursos?

Cuando se selecciona un recurso para moverlo, el grupo de recursos correspondiente se agrega automáticamente. Esto es para que el recurso de destino se pueda colocar en un grupo de recursos. Puede optar por personalizar y proporcionar un grupo de recursos existente, una vez que se haya agregado para el traslado. El traslado de un grupo de recursos no implica necesariamente que se vayan a mover todos los recursos del grupo de recursos de origen.

### <a name="can-i-move-resources-across-subscriptions-when-i-move-them-across-regions"></a>¿Puedo mover los recursos entre suscripciones cuando los muevo entre regiones?

Puede cambiar la suscripción después de mover los recursos a la región de destino. [Más información](../azure-resource-manager/management/move-resource-group-and-subscription.md) sobre cómo mover recursos a otra suscripción. 

### <a name="does-azure-resource-mover-store-customer-data"></a>¿Azure Resource Mover almacena datos de clientes? 
No. El servicio Resource Mover no almacena datos de clientes, solo almacena información de metadatos que facilita el seguimiento y progreso de los recursos que el cliente mueve.

### <a name="where-is-the-metadata-for-moving-across-regions-stored"></a>¿Dónde se almacenan los metadatos para mover entre regiones?

Se almacenan en una base de datos de [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) y en [Azure Blob Storage](../storage/common/storage-service-encryption.md), en una suscripción de Microsoft. Actualmente, los metadatos se almacenan en el Este de EE. UU. 2 y en el Norte de Europa. Expandiremos esta cobertura a otras regiones. Esto no impide el movimiento de recursos en todas las regiones públicas.

### <a name="is-the-collected-metadata-encrypted"></a>¿Se cifran los metadatos recopilados?

Sí, tanto en tránsito como en reposo.
- Durante el tránsito, los metadatos se envían de forma segura al servicio Resource Mover a través de Internet mediante HTTPS.
- En almacenamiento, los metadatos se cifran.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>¿Cómo se usa la identidad administrada en Resource Mover?

La [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md), anteriormente denominada Managed Service Identity (MSI), proporciona a los servicios de Azure una identidad administrada automáticamente en Azure AD.
- Resource Mover usa la identidad administrada para poder acceder a las suscripciones de Azure a fin de mover recursos entre regiones.
- Una colección de movimiento necesita una identidad asignada por el sistema, con acceso a la suscripción que contiene los recursos que se van a mover.

- Si mueve recursos entre regiones del portal, este proceso se produce automáticamente.
- Si mueve recursos mediante PowerShell, ejecute cmdlets para asignar una identidad asignada por el sistema a la colección y, a continuación, asigne un rol con los permisos de suscripción correctos a la entidad de seguridad de identidades. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>¿Qué permisos de identidad administrados necesita Resource Mover? 

La identidad administrada de Azure Resource Mover necesita al menos estos permisos: 

- Permiso para escribir o crear recursos en la suscripción de usuario, disponibles con el rol *Colaborador*. 
- Permiso para crear asignaciones de roles Normalmente disponible con los roles *Propietario* o *Administrador de acceso de usuario*, con un rol personalizado que tenga asignado el permiso *Microsoft.Authorization/asignaciones de roles/permiso de escritura*. Este permiso no es necesario si ya se concedió a la identidad administrada del recurso compartido de datos acceso al almacén de datos de Azure. 
 
Al agregar recursos en el centro de Resource Mover en el portal, los permisos se controlan automáticamente siempre que el usuario tenga los permisos descritos anteriormente. Si agrega recursos con PowerShell, asigne los permisos manualmente.

> [!IMPORTANT]
> Le recomendamos encarecidamente que no modifique ni quite las asignaciones de roles de identidad. 

### <a name="what-if-i-dont-have-permissions-to-assign-role-identity"></a>¿Qué debo hacer si no tengo permisos para asignar la identidad de rol?

Hay un par de razones por las que puede que no tenga permisos.

**Causa posible** | **Recomendación**
--- | ---
No es *Colaborador* ni *Administrador de acceso de usuario* (o *Propietario*) al agregar un recurso por primera vez. | Use una cuenta con permisos de *Colaborador* y *Administrador de acceso de usuario* (o *Propietario*) para la suscripción.
La identidad administrada de Resource Mover no tiene el rol requerido. | Agregue los roles "Colaborador" y "Administrador de acceso de usuario".
La identidad administrada de Resource Mover se restableció en *Ninguno*. | Vuelva a habilitar una identidad asignada por el sistema en la configuración Mover colección > **Identidad**. También puede volver a agregar el recurso en **Agregar recursos**, que hace lo mismo.  
La suscripción se ha movido a otro inquilino. | Deshabilite y vuelva a habilitar la identidad administrada para la colección de movimiento.

### <a name="how-can-i-do-multiple-moves-together"></a>¿Cómo se pueden realizar varios movimientos a la vez?

Cambie las combinaciones de origen y destino según sea necesario mediante la opción de cambiar en el portal.

### <a name="what-happens-when-i-remove-a-resource-from-a-list-of-move-resources"></a>¿Qué pasa si se quita un recurso de una lista de recursos que se van a mover?

Los recursos que se han agregado a la lista se pueden quitar. El comportamiento exacto de eliminación depende del estado de los recursos. [Más información](remove-move-resources.md#vm-resource-state-after-removing).



## <a name="next-steps"></a>Pasos siguientes

[Más información](about-move-process.md) sobre los componentes de Resource Mover y el proceso de movimiento.
