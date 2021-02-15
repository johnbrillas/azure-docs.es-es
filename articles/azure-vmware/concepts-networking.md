---
title: 'Conceptos: interconectividad de red'
description: Conozca los aspectos clave y los casos de uso de redes e interconectividad en Azure VMware Solution.
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: bc8a2f8c07a2a4fe37c4899dc33d5173a99dc423
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99538982"
---
# <a name="azure-vmware-solution-networking-and-interconnectivity-concepts"></a>Conceptos de interconectividad y redes de Azure VMware Solution

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Una perspectiva útil sobre la interconectividad es considerar los dos tipos de implementaciones de nube privada de Azure VMware Solution:

1. La [**interconectividad básica solo de Azure**](#azure-virtual-network-interconnectivity) permite administrar y usar la nube privada con una sola red virtual en Azure. Esta implementación es más adecuada para las evaluaciones o implementaciones de Azure VMware Solution que no requieren acceso desde entornos locales.

1. La [**interconectividad completa entre el entorno local y la nube privada**](#on-premises-interconnectivity) amplía la implementación básica solo de Azure para incluir la interconectividad entre el entorno local y las nubes privadas de Azure VMware Solution.
 
En este artículo se tratan algunos conceptos clave que establecen las redes y la interconectividad, incluidos los requisitos y las limitaciones. También se tratarán con más detalle los dos tipos de implementaciones de interconectividad de la nube privada de Azure VMware Solution. En este artículo se proporciona la información necesaria para configurar correctamente las redes para que funcionen correctamente con Azure VMware Solution.

## <a name="azure-vmware-solution-private-cloud-use-cases"></a>Casos de uso de la nube privada de Azure VMware Solution

Los casos de uso de las nubes privadas de Azure VMware Solution incluyen los siguientes:
- nuevas cargas de trabajo de máquinas virtuales de VMware en la nube
- Ráfaga de cargas de trabajo de máquinas virtuales a la nube (solo del entorno local a Azure VMware Solution)
- Migración de cargas de trabajo de máquinas virtuales a la nube (solo del entorno local a Azure VMware Solution)
- Recuperación ante desastres (de Azure VMware Solution a Azure VMware Solution o del entorno local a Azure VMware Solution)
- Consumo de servicios de Azure

> [!TIP]
> En todos los casos de uso del servicio Azure VMware Solution se permite la conectividad del entorno local a la nube privada.

## <a name="azure-virtual-network-interconnectivity"></a>Conectividad de la red virtual de Azure

En la implementación de red virtual a nube privada, puede administrar la nube privada de Azure VMware Solution, usar cargas de trabajo de la nube privada y acceder a servicios de Azure mediante la conexión de ExpressRoute. 

En el diagrama siguiente se muestra la interconectividad de red básica establecida al implementar una nube privada. Muestra las redes lógicas basadas en ExpressRoute entre una red virtual de Azure y una nube privada. La interconectividad cumple tres de los casos de uso principales:
* Acceso de entrada a vCenter Server y a NSX-T Manager que es accesible desde las máquinas virtuales de la suscripción de Azure y no desde los sistemas locales. 
* Acceso de salida desde las máquinas virtuales a los servicios de Azure. 
* Acceso de entrada y consumo de cargas de trabajo que ejecutan una nube privada.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Conectividad de red virtual básica a nube privada" border="false":::

## <a name="on-premises-interconnectivity"></a>Interconectividad local

En la implementación de red virtual y local a nube privada completa, puede acceder a las nubes privadas de Azure VMware Solution desde entornos locales. Esta implementación es una extensión de la implementación básica descrita en la sección anterior. Al igual que la implementación básica, se requiere un circuito ExpressRoute pero, con esta implementación, se usa para conectarse desde entornos locales a la nube privada en Azure. 

En el diagrama siguiente se muestra la interconectividad local a nube privada, que permite los siguientes casos de uso:
* Cross-vCenter vMotion frecuente/poco frecuente
* Acceso de administración desde los entornos locales a la nube privada de Azure VMware Solution

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Red virtual y conectividad de nube privada local completa" border="false":::

Para una interconectividad total a la nube privada, habilite ExpressRoute Global Reach y solicite una clave de autorización y un identificador de emparejamiento privado de Global Reach en Azure Portal. La clave de autorización y el identificador de emparejamiento se usan para establecer Global Reach entre un circuito ExpressRoute de la suscripción y el circuito ExpressRoute de la nueva nube privada. Una vez vinculados, los dos circuitos ExpressRoute enrutan el tráfico de red entre los entornos locales a la nube privada.  Para conocer los procedimientos necesarios para solicitar y usar la clave de autorización y el identificador de emparejamiento, vea el [tutorial para crear un emparejamiento de ExpressRoute Global Reach con una nube privada](tutorial-expressroute-global-reach-private-cloud.md).

## <a name="next-steps"></a>Pasos siguientes 

Ahora que ha tratado estos conceptos de red e interconectividad de Azure VMware Solution, puede que quiera obtener información sobre:

- [Conceptos de almacenamiento de Azure VMware Solution](concepts-storage.md).
- [Conceptos de identidad de Azure VMware Solution](concepts-identity.md).
- [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md).

<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

