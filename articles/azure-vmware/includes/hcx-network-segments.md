---
title: Segmentos de red de VMware HCX
description: Hay cuatro redes necesarias para VMware HCX.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: e9b37c125db82a95c137ede8d642888fba8b6c80
ms.sourcegitcommit: 87a6587e1a0e242c2cfbbc51103e19ec47b49910
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622235"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Existen diferentes maneras de configurar los segmentos de red de VMware HCX en el entorno local. A continuación, se describe una configuración simple que admite un caso de uso de producción pequeña o piloto.  Al diseñar para cientos o miles de cargas de trabajo, es posible que sea necesario cambiar esta configuración, en función de las necesidades de la migración.  

Como preparación para que la implementación de VMware HCX admita el caso de uso de producción pequeña o piloto, identifique lo siguiente:

- **Red de administración:** al implementar VMware HCX local, deberá definir una red de administración.  Normalmente, es la misma red de administración que usa el clúster de VMware local.  Como mínimo, identifique **dos** direcciones IP en este segmento de red para VMware HCX. Es posible que necesite números mayores, en función de la escala de la implementación más allá del caso de uso pequeño o piloto.

> [!NOTE]
   > El método recomendado es crear una red /26, porque puede usar hasta 10 mallas de servicio y 60 extensores de red (-1 por cada malla de servicio). Se pueden expandir **ocho** redes por cada extensor de red mediante nubes privadas de soluciones de Azure VMware Solution.
   >
   
- **Red vMotion:** al implementar VMware HCX local, deberá definir una red vMotion.  Normalmente, es la misma red que usa el clúster de VMware local para vMotion.  Como mínimo, identifique **dos** direcciones IP en este segmento de red para VMware HCX. Es posible que necesite números mayores, en función de la escala de la implementación más allá del caso de uso pequeño o piloto.

   La red de vMotion debe exponerse en un conmutador virtual distribuido o vSwitch0. Si no es así, modifique el entorno para adaptarlo.

   > [!NOTE]
   > Muchos entornos de VMware utilizan segmentos de red no enrutados para vMotion, que no plantea ningún problema.

- **Red de vínculo superior:** al implementar VMware HCX local, deberá definir una red de vínculo superior. Use la red de administración definida como red de vínculo superior.
   
- **Red de replicación:** al implementar VMware HCX local, deberá definir una red de replicación. Use la red de administración definida como red de replicación.  Si los hosts del clúster local usan una red VMkernel de replicación dedicada, reserve **dos** direcciones IP en este segmento de red y use la red VMkernel de replicación para la red de replicación.
