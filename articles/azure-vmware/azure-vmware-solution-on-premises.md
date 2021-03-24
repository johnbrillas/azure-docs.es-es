---
title: Conexión de Azure VMware Solution al entorno local
description: Aprenda a conectar Azure VMware Solution al entorno local.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 0b26dc4756cb37544c2b2f8c5a75df0ac1a9d629
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491799"
---
# <a name="connect-azure-vmware-solution-to-your-on-premises-environment"></a>Conexión de Azure VMware Solution al entorno local

En este artículo, podrá seguir usando la información [recopilada durante la planeación](production-ready-deployment-steps.md) para conectar Azure VMware Solution al entorno local.

Antes de comenzar, tiene que cumplir dos requisitos previos para conectar Azure VMware Solution al entorno local:

- Tener un circuito de ExpressRoute desde su entorno local a Azure.
- Tener un bloque de direcciones de red de tipo CIDR /29 no superpuesto para el emparejamiento de Global Reach de ExpressRoute que definió como parte de la [fase de planeación](production-ready-deployment-steps.md).

>[!NOTE]
> Puede conectarse mediante VPN, pero ese tema no se detalla en este documento de inicio rápido.

## <a name="establish-an-expressroute-global-reach-connection"></a>Establecimiento de una conexión de Global Reach de ExpressRoute

Para establecer la conectividad local a la nube privada de Azure VMware Solution con Global Reach de ExpressRoute, siga los pasos del tutorial [Entornos locales del mismo nivel en una nube privada](tutorial-expressroute-global-reach-private-cloud.md).

En este tutorial se produce una conexión como se muestra en el diagrama.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-on-premises-diagram.png" alt-text="Diagrama de conectividad de red local de Global Reach de ExpressRoute." lightbox="media/pre-deployment/azure-vmware-solution-on-premises-diagram.png" border="false":::

## <a name="verify-on-premises-network-connectivity"></a>Comprobación de la conectividad de red local

Ahora debería ver en el **enrutador perimetral local** el lugar donde ExpressRoute conecta los segmentos de red NSX-T y los segmentos de administración de Azure VMware Solution.

>[!IMPORTANT]
>Todos los usuarios tienen un entorno diferente, aunque algunos deberán permitir que estas rutas se propaguen de nuevo a la red local.  

Algunos entornos tienen firewalls que protegen el circuito de ExpressRoute.  Si no hay ningún firewall y no se produce ninguna eliminación de entradas, puede hacer ping en el servidor vCenter de Azure VMware Solution o en una [máquina virtual en el segmento NSX-T](deploy-azure-vmware-solution.md#add-a-vm-on-the-nsx-t-network-segment) desde el entorno local. Igualmente, puede llegar a los recursos del entorno local desde la VM del segmento NSX-T.

## <a name="next-steps"></a>Pasos siguientes

Continúe con la siguiente sección para implementar y configurar VMware HCX.

> [!div class="nextstepaction"]
> [Implementación y configuración de VMware HCX](tutorial-deploy-vmware-hcx.md)