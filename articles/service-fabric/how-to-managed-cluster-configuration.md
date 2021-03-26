---
title: Configuración de un clúster administrado de Service Fabric (versión preliminar)
description: Aprenda a configurar el clúster administrado de Service Fabric para las actualizaciones automáticas del sistema operativo, las reglas de grupo de seguridad de red y mucho más.
ms.topic: how-to
ms.date: 02/15/2021
ms.openlocfilehash: 44b1b949fe314231cb44f190c31b53903e47a904
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101732639"
---
# <a name="service-fabric-managed-cluster-preview-configuration-options"></a>Opciones de configuración del clúster administrado de Service Fabric (versión preliminar)

Además de seleccionar el [SKU de clúster administrado de Service Fabric](overview-managed-cluster.md#service-fabric-managed-cluster-skus) al crear el clúster, hay varias formas de configurarlo. En la versión preliminar actual, puede:

* Configurar las [opciones de red](how-to-managed-cluster-networking.md) para el clúster
* Agregar una [extensión del conjunto de escalado de máquinas virtuales](how-to-managed-cluster-vmss-extension.md) a un tipo de nodo de clúster administrado
* Configurar la [identidad administrada](how-to-managed-identity-managed-cluster-virtual-machine-scale-sets.md) en los tipos de nodo
* Habilitar las [actualizaciones automáticas del sistema operativo](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades) para los nodos
* Habilitar el [cifrado de discos de datos y sistema operativo](how-to-enable-managed-cluster-disk-encryption.md) en los nodos

## <a name="enable-automatic-os-image-upgrades"></a>Habilitar las actualizaciones automáticas de las imágenes del sistema operativo

Puede elegir habilitar las actualizaciones automáticas de imágenes del sistema operativo en las máquinas virtuales que ejecutan los nodos de clúster administrados. Aunque los recursos del conjunto de escalado de máquinas virtuales se administran en su nombre con los clústeres administrados, es su elección habilitar actualizaciones automáticas de imágenes de sistema operativo para los nodos del clúster. Al igual que con los clústeres de [Service fabric clásicos](service-fabric-best-practices-infrastructure-as-code.md#azure-virtual-machine-operating-system-automatic-upgrade-configuration), los nodos de clústeres administrados no se actualizan de forma predeterminada, con el fin de evitar interrupciones imprevistas en el clúster.

Para habilitar las actualizaciones automáticas del sistema operativo:

* Utilice la versión `2021-01-01-preview` (o posterior) de los recursos *Microsoft.ServiceFabric/managedclusters* y *Microsoft.ServiceFabric/managedclusters/nodetypes*.
* Establezca la propiedad `enableAutoOSUpgrade` del clúster en *true*.
* Establezca la propiedad del recurso nodeTypes del clúster `vmImageVersion` en *latest*.

Por ejemplo:

```json
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters",
      ...
      "properties": {
        ...
        "enableAutoOSUpgrade": true
      },
    },
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
       ...
      "properties": {
        ...
        "vmImageVersion": "latest",
        ...
      }
    }
}

```

Una vez habilitado, Service Fabric comenzará a consultar y realizar el seguimiento de las versiones de imagen del sistema operativo en el clúster administrado. Si hay disponible una nueva versión del sistema operativo, se actualizarán los tipos de nodo de clúster (conjuntos de escalado de máquinas virtuales), de uno en uno. Las actualizaciones en tiempo de ejecución de Service Fabric solo se realizan después de confirmar que no hay actualizaciones de imágenes de sistema operativo de nodo de clúster en curso.

Si se produce un error en una actualización, Service Fabric lo volverá a intentar después de 24 horas, con un máximo de tres reintentos. Al igual que las actualizaciones de Service Fabric clásico (no administradas), las aplicaciones o los nodos incorrectos pueden bloquear la actualización de la imagen del sistema operativo.

Para más información sobre las actualizaciones de imágenes, consulte [Actualizaciones automáticas de imágenes de sistema operativo con conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md).

## <a name="next-steps"></a>Pasos siguientes

[Información general de los clústeres administrados de Service Fabric](overview-managed-cluster.md)

[Plantillas de clúster de Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates)
