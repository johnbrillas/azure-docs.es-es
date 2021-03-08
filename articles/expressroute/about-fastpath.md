---
title: Acerca de Azure ExpressRoute FastPath
description: Más información acerca de Azure ExpressRoute FastPath para enviar tráfico de red omitiendo la puerta de enlace
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: duau
ms.openlocfilehash: ba23319c35aed1d09da652e6f84b60e5f8e9495e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101740892"
---
# <a name="about-expressroute-fastpath"></a>Acerca de FastPath de ExpressRoute

La puerta de enlace de red virtual de ExpressRoute está diseñada para intercambiar las rutas de red y enrutar el tráfico de red. FastPath está diseñado para mejorar el rendimiento de las rutas de acceso a los datos entre la red local y una red virtual. Cuando está habilitado, FastPath envía el tráfico de red directamente a las máquinas virtuales que están en la red, omitiendo la puerta de enlace.

## <a name="requirements"></a>Requisitos

### <a name="circuits"></a>Circuitos

FastPath está disponible en todos los circuitos de ExpressRoute.

### <a name="gateways"></a>Puertas de enlace

FastPath de todos modos necesita que se cree una puerta de enlace de red virtual para intercambiar rutas entre la red virtual y una red local. Para más información acerca de las puertas de enlace de red virtual y ExpressRoute, incluidas las SKU de puerta de enlace y la información sobre el rendimiento, consulte [Puerta de enlace de red virtual de ExpressRoute](expressroute-about-virtual-network-gateways.md).

Para configurar FastPath, la puerta de enlace de red virtual debe ser una de las siguientes:

* Ultrarrendimiento
* ErGw3AZ

> [!IMPORTANT]
> Si tiene previsto usar FastPath con el emparejamiento privado basado en IPv6 a través de ExpressRoute, asegúrese de seleccionar ErGw3AZ en **SKU**.
> 
>

## <a name="limitations"></a>Limitaciones

Aunque FastPath admite la mayoría de las configuraciones, no es compatible con las siguientes características:

* UDR en la subred de puerta de enlace: si aplica un UDR a la subred de puerta de enlace de la red virtual, el tráfico de la red local se seguirá enviando a la puerta de enlace de red virtual.

* Emparejamiento de redes virtuales: si tiene otras redes virtuales emparejadas con la que está conectada a ExpressRoute, el tráfico de la red local a las otras redes virtuales (es decir, las llamadas redes virtuales de "radio") se seguirá enviado a la puerta de enlace de red virtual. La solución es conectar todas las redes virtuales directamente al circuito de ExpressRoute.

* Equilibrador de carga básico: si implementa un equilibrador de carga interno básico en la red virtual, o el servicio PaaS de Azure que implementa en la red virtual usa un equilibrador de carga interno básico, el tráfico de la red local a las direcciones IP virtuales hospedadas en el equilibrador de carga básico se enviará a la puerta de enlace de red virtual. La solución es actualizar el equilibrador de carga básico a un [equilibrador de carga estándar](../load-balancer/load-balancer-overview.md).

* Vínculo privado: si se conecta con un [punto de conexión privado](../private-link/private-link-overview.md) de la red virtual desde la red local, la conexión pasará por la puerta de enlace de red virtual.
 
## <a name="next-steps"></a>Pasos siguientes

Para habilitar FastPath, consulte [Conexión de una red virtual con un circuito de ExpressRoute mediante el portal](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
