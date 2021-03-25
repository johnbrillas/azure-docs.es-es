---
title: Versiones de software de VMware
description: Versiones de software de VMware compatibles con Azure VMware Solution.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: 9a1781d163e67c2f9b6a4f756a616e59a969696f
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462528"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Las versiones de software de VMware utilizadas en las nuevas implementaciones de clústeres de nubes privadas de Azure VMware Solution son:

| Software              |    Versión   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3    | 
| ESXi                  |    6.7 U3    | 
| vSAN                  |    6.7 U3    |
| NSX-T <br />**NOTA:** : La única versión admitida de NSX es NSX-T.               |      2.5     |


Si se agregan nuevos clústeres a una nube privada existente, se aplica la versión de software que se está ejecutando actualmente. Para obtener más información, consulte los [requisitos de la versión de software de VMware](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Las actualizaciones del conjunto de productos de software de la nube privada mantienen el software dentro de una de las versiones más recientes del conjunto de productos de software de VMware. Las versiones de software de la nube privada pueden diferir de las versiones más recientes de los componentes de software individuales (ESXi, NSX-T, vCenter, vSAN). Podrá encontrar las directivas y procesos de actualización generales del software de la plataforma Azure VMware Solution descritos en [Actualizaciones y mejoras de la nube privada](../concepts-upgrades.md).

