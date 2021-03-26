---
title: 'Azure ExpressRoute: restablecimiento de los emparejamientos de circuitos mediante Azure Portal'
description: Aprenda a deshabilitar y habilitar los emparejamientos de un circuito de Azure ExpressRoute mediante Azure Portal.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 11/30/2020
ms.author: duau
ms.openlocfilehash: 432ecedbbb8965926499380eb1165fdf43018426
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97680284"
---
# <a name="reset-expressroute-circuit-peerings-by-using-the-azure-portal"></a>Restablecimiento de los emparejamientos de circuitos ExpressRoute mediante Azure Portal

En este artículo se describe cómo deshabilitar y habilitar los emparejamientos de un circuito de Azure ExpressRoute mediante Azure Portal. Cuando se deshabilita un emparejamiento, la sesión de Protocolo de puerta de enlace de borde (BGP) en la conexión principal y secundaria del circuito ExpressRoute se cierra. Cuando se habilita un emparejamiento, la sesión BGP en la conexión principal y secundaria del circuito ExpressRoute se restablece.

> [!Note]
> La primera vez que configure los emparejamientos en el circuito ExpressRoute, estos se habilitan de forma predeterminada.

El restablecimiento de los emparejamientos de ExpressRoute puede ser útil en los escenarios siguientes:

* Está probando la implementación y el diseño de la recuperación ante desastres. Por ejemplo, suponga que tiene dos circuitos ExpressRoute. Puede deshabilitar los emparejamientos de un circuido y obligar al tráfico de red a usar el otro circuito.

* Quiere habilitar la detección de reenvío bidireccional (BFD) en el emparejamiento privado de Azure o el emparejamiento de Microsoft. Si el circuito ExpressRoute se creó antes del 1 de agosto de 2018, en el emparejamiento privado de Azure, o antes del 10 de enero de 2020, en el emparejamiento de Microsoft, BFD no estaba habilitado de forma predeterminada. Restablezca el emparejamiento para habilitar BFD.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

En un explorador, vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

## <a name="reset-a-peering"></a>Restablecimiento de un emparejamiento

Puede restablecer el emparejamiento de Microsoft y el emparejamiento privado de Azure en un circuito ExpressRoute de forma independiente.

1. Elija el circuito que quiere cambiar.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit-list.png" alt-text="Captura de pantalla que muestra la elección de un circuito en la lista de circuitos ExpressRoute.":::

1. Elija la configuración de emparejamiento que quiere restablecer.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit.png" alt-text="Captura de pantalla que muestra cómo elegir un emparejamiento en la información general del circuito ExpressRoute.":::

1. Desactive la casilla **Enable Peering** (Habilitar emparejamiento) y, luego, seleccione **Guardar** para deshabilitar la configuración de emparejamiento.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/disable-peering.png" alt-text="Captura de pantalla que muestra la desactivación de la casilla Enable Peering (Habilitar emparejamiento).":::

1. Active la casilla **Enable Peering** (Habilitar emparejamiento) y, luego, seleccione **Guardar** para volver a habilitar la configuración de emparejamiento.

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas de ExpressRoute, consulte los siguientes artículos:

* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solución de problemas de rendimiento de red](expressroute-troubleshooting-network-performance.md)
