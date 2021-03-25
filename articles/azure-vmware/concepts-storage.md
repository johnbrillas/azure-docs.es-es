---
title: Conceptos sobre almacenamiento
description: Obtenga información sobre las principales funcionalidades de almacenamiento de las nubes privadas de Azure VMware Solution.
ms.topic: conceptual
ms.date: 03/13/2021
ms.openlocfilehash: 83a4a52f8414e656b5bc688796db6e93a53d4d76
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104801628"
---
#  <a name="azure-vmware-solution-storage-concepts"></a>Conceptos de almacenamiento de Azure VMware Solution

Las nubes privadas de Azure VMware Solution proporcionan almacenamiento nativo en todo el clúster con vSAN de VMware. Todo el almacenamiento local de cada host de un clúster se usa en un almacén de datos de vSAN, y el cifrado de datos en reposo está disponible y habilitado de forma predeterminada. Puede usar recursos de Azure Storage para ampliar las funcionalidades de almacenamiento de las nubes privadas.

## <a name="vsan-clusters"></a>Clústeres de vSAN

El almacenamiento local en cada host de un clúster se usa como parte de un almacén de datos de vSAN. Todos los grupos de discos usan un nivel de caché de NVMe de 1,6 TB con la capacidad sin procesar, por host y basada en SSD de 15,4 TB. El tamaño del nivel de capacidad sin procesar de un clúster es la capacidad por host multiplicada por la cantidad de hosts. Por ejemplo, un clúster de cuatro hosts proporcionará una capacidad sin procesar de 61,6 TB en el nivel de capacidad de vSAN.

El almacenamiento local en los hosts de un clúster se usa en el almacén de datos de vSAN en todo el clúster. Todos los almacenes de datos se crean como parte de una implementación de nube privada y están disponibles para su uso inmediato. El usuario cloudadmin y todos los usuarios del grupo CloudAdmin pueden administrar almacenes de datos con estos privilegios de vSAN:

- Datastore.AllocateSpace
- Datastore.Browse
- Datastore.Config
- Datastore.DeleteFile
- Datastore.FileManagement
- Datastore.UpdateVirtualMachineMetadata

## <a name="data-at-rest-encryption"></a>Cifrado de datos en reposo

Los almacenes de datos de vSAN usan el cifrado de datos en reposo de forma predeterminada. La solución de cifrado se basa en KMS y admite operaciones de vCenter para la administración de claves. Las claves se almacenan cifradas, encapsuladas por una clave maestra de Azure Key Vault. Cuando se quita un host de un clúster, los datos de los discos SSD se invalidan de inmediato.

## <a name="scaling"></a>Ampliación

La capacidad nativa de almacenamiento de un clúster se escala mediante la adición de hosts al clúster. En el caso de los clústeres que usan hosts AVS36, la capacidad sin procesar en todo el clúster aumenta 15,4 TB con cada host agregado. Los hosts tardan unos 10 minutos en agregarse a un clúster.  Para instrucciones sobre el escalado de clústeres, consulte el [tutorial sobre escalado de nubes privadas][tutorial-scale-private-cloud].

## <a name="azure-storage-integration"></a>Integración del almacenamiento de Azure

Puede usar los servicios de almacenamiento de Azure en las cargas de trabajo que se ejecutan en su nube privada. Entre los servicios de almacenamiento de Azure se incluyen cuenta de almacenamiento, Table Storage y Blob Storage. La conexión de las cargas de trabajo a los servicios de almacenamiento de Azure no atraviesa Internet. Esta conectividad ofrece más seguridad y le permite usar los servicios de almacenamiento de Azure basados en SLA en las cargas de trabajo de su nube privada.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha visto los conceptos de Azure VMware Solution, puede que quiera obtener información sobre:

- [Conceptos de identidad de nube privada](concepts-identity.md).
- [Control de acceso basado en rol de vSphere para Azure VMware Solution](concepts-identity.md).
- [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md).
- [Azure NetApp Files con Azure VMware Solution](netapp-files-with-azure-vmware-solution.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
