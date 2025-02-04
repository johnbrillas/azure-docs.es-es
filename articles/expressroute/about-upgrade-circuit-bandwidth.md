---
title: Acerca de la actualización del ancho de banda del circuito | Azure ExpressRoute
description: En este artículo, conocerá los procedimientos recomendados para actualizar el ancho de banda del circuito ExpressRoute.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: duau
ms.openlocfilehash: 7831e7944321e074c312853e1534c47970ebdfdb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "89397957"
---
# <a name="about-upgrading-expressroute-circuit-bandwidth"></a>Acerca de la actualización del ancho de banda del circuito ExpressRoute

ExpressRoute permite la conectividad dedicada y privada con la red global de Microsoft. La conectividad se facilita mediante la red de un asociado de ExpressRoute o una conexión directa a los dispositivos de Microsoft Enterprise Edge (MSEE). Una vez que se ha configurado y probado la conectividad física, puede habilitar la conectividad de capa 2 y capa 3 creando un circuito ExpressRoute y configurando el emparejamiento.

## <a name="upgrade-circuit-bandwidth"></a><a name="upgrade"></a>Actualización del ancho de banda del circuito

Para actualizar el ancho de banda del circuito, el asociado de ExpressRoute Direct o de ExpressRoute debe tener [suficiente ancho de banda disponible](#considerations) para que la actualización se realice correctamente.

Si la capacidad está disponible, puede actualizar el circuito mediante los métodos siguientes:

* [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [PowerShell](expressroute-howto-circuit-arm.md#modify)
* [CLI de Azure](howto-circuit-cli.md#modify)

## <a name="capacity-considerations"></a><a name="considerations"></a>Consideraciones sobre la capacidad

### <a name="insufficient-expressroute-partner-bandwidth"></a><a name="bandwidth"></a>Ancho de banda insuficiente del asociado de ExpressRoute

Si el asociado de ExpressRoute no tiene suficiente capacidad, debe crear un circuito, configurado con el ancho de banda deseado. Para mantener la conectividad, no elimine el circuito anterior hasta que se haya aprovisionado el circuito recién creado, se haya configurado el emparejamiento y, en relación con el emparejamiento privado, se haya aprovisionado el objeto de conexión a la puerta de enlace de red virtual de ExpressRoute.

Si el asociado de ExpressRoute no tiene suficiente capacidad disponible, debe solicitar capacidad adicional en la ubicación de emparejamiento deseada. Una vez aprovisionada la nueva capacidad, puede usar los pasos que se incluyen en los artículos de la sección [Actualización del ancho de banda del circuito](#upgrade) para crear un circuito, configurar la conectividad y eliminar el circuito anterior.


### <a name="insufficient-expressroute-direct-bandwidth"></a><a name="bandwidth"></a>Ancho de banda insuficiente de ExpressRoute Direct

Si ExpressRoute Direct no tiene suficiente capacidad, puede eliminar los circuitos asociados al recurso de ExpressRoute Direct que ya no son necesarios, o bien crear otro recurso de ExpressRoute Direct. Para instrucciones sobre cómo administrar el recurso de ExpressRoute Direct, consulte [Configuración de ExpressRoute Direct](how-to-expressroute-direct-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación y modificación de un circuito](expressroute-howto-circuit-portal-resource-manager.md)
* [Creación y modificación de la configuración de emparejamiento](expressroute-howto-routing-portal-resource-manager.md)
* [Vinculación de una red virtual a un circuito ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
