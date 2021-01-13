---
title: Versiones de software de VMware
description: Versiones de software de VMware compatibles con Azure VMware Solution.
ms.topic: include
ms.date: 12/29/2020
ms.openlocfilehash: c6ba2904bab6c6f44001cafed1bd4cbdeb786373
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/30/2020
ms.locfileid: "97825082"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Las versiones actuales del software de VMware que se usan en los clústeres de nube privada de Azure VMware Solution son:

| Software              |    Versión   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3    | 
| ESXi                  |    6.7 U3    | 
| vSAN                  |    6.7 U3    |
| NSX-T                 |      2.5     |


>[!NOTE]
>La única versión admitida de NSX es NSX-T.

En el caso de cualquier clúster nuevo en una nube privada, la versión del software coincide con la que se está ejecutando actualmente. Para cualquier nube privada nueva en una suscripción, se instala la versión más reciente de la pila de software. Para obtener más información, consulte los [requisitos de la versión de software de VMware](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Las actualizaciones del conjunto de productos de software de la nube privada mantienen el software dentro de una de las versiones más recientes del conjunto de productos de software de VMware. Las versiones de software de la nube privada pueden diferir de las versiones más recientes de los componentes de software individuales (ESXi, NSX-T, vCenter, vSAN). Podrá encontrar las directivas y procesos de actualización generales del software de la plataforma Azure VMware Solution descritos en [Actualizaciones y mejoras de la nube privada](../concepts-upgrades.md).

