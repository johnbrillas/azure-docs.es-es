---
title: 'Conceptos: Actualizaciones y mejoras de la nube privada'
description: Conozca los procesos de actualización y las características principales de Azure VMware Solution.
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: d93453cbf6ad744844a04cd298cc18ad181cc0b0
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100634999"
---
# <a name="azure-vmware-solution-private-cloud-updates-and-upgrades"></a>Actualizaciones y mejoras de la nube privada de Azure VMware Solution

Una de las ventajas de las nubes privadas de Azure VMware Solution es que la plataforma se mantiene automáticamente. El mantenimiento incluye actualizaciones automatizadas de un conjunto de productos de software validado de VMware, lo que ayuda a garantizar que se usa la versión más reciente del software de nube privada de Azure VMware Solution.

En concreto, una nube privada de Azure VMware Solution incluye:

- Nodos de servidor sin sistema operativo dedicados aprovisionados con el hipervisor de VMware ESXi 
- Servidor vCenter para administrar ESXi y vSAN 
- Redes definidas por software de NSX-T de VMware para máquinas virtuales de carga de trabajo de vSphere  
- Almacén de datos de vSAN de VMware para máquinas virtuales de carga de trabajo de vSphere  
- HCX de VMware para la movilidad de la carga de trabajo  

Una nube privada de Azure VMware Solution incluye también recursos en el componente subyacente de Azure necesario para la conectividad y para operar la nube privada. Azure VMware Solution supervisa continuamente el estado de los componentes subyacentes y de VMware. Cuando Azure VMware Solution detecta un error, toma medidas para reparar los componentes que no funcionan. 

## <a name="what-components-get-updated"></a>¿Qué componentes se actualizan?   

Azure VMware Solution actualiza los siguientes componentes de VMware: 

- vCenter Server y ESXi se ejecutan en los nodos de servidor sin sistema operativo 
- vSAN 
- NSX-T 

Azure VMware Solution también actualiza el software del componente subyacente, como controladores, software de los conmutadores de red y firmware de los nodos sin sistema operativo. 

## <a name="types-of-updates"></a>Tipos de actualizaciones

Azure VMware Solution aplica los siguientes tipos de actualizaciones a componentes de VMware:

- Revisiones: Revisiones de seguridad y correcciones de errores publicadas por VMware. 
- Actualizaciones: Actualizaciones de versiones secundarias de uno o más varios de VMware. 
- Mejoras: Actualizaciones de versiones principales de uno o varios componentes de VMware.

Recibirá una notificación antes y después de que se apliquen las revisiones a las nubes privadas. También trabajaremos con usted para programar una ventana de mantenimiento antes de aplicar actualizaciones o mejoras a la nube privada. 

## <a name="vmware-appliance-backup"></a>Copia de seguridad de dispositivos de VMware 

Azure VMware Solution también realiza una copia de seguridad de la configuración de los siguientes componentes de VMware:

- vCenter Server 
- NSX-T Manager 

Cuando se produzcan errores, Azure VMware Solution puede restaurar estos componentes desde la copia de seguridad de la configuración. 

Para más información sobre las versiones de software de VMware, consulte el [artículo sobre los conceptos de nubes privadas y clústeres](concepts-private-clouds-clusters.md) y las [preguntas frecuentes](faq.yml).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya conoce los procesos de actualización y las características principales de Azure VMware Solution (AVS), puede obtener información sobre los temas siguientes:

- [Creación de una nube privada](tutorial-create-private-cloud.md).
- [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
