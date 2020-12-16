---
title: 'Azure ExpressRoute: Restablecimiento del emparejamiento del circuito mediante Azure Portal'
description: Aprenda a deshabilitar y habilitar emparejamientos de un circuito de Azure ExpressRoute mediante Azure Portal.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 11/30/2020
ms.author: duau
ms.openlocfilehash: d4d6b0b0cce4f5304f7c5790ef2bda05633be52f
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581371"
---
# <a name="reset-expressroute-circuit-peerings-use-the-azure-portal"></a>Restablecimiento de emparejamientos de circuito ExpressRoute mediante Azure Portal

En este artículo se describe cómo deshabilitar y habilitar los emparejamientos de un circuito ExpressRoute mediante Azure Portal. Cuando se deshabilita un emparejamiento, la sesión de BGP en la conexión principal y en la conexión secundaria del circuito de ExpressRoute se cerrará. Perderá la conectividad mediante este emparejamiento a Microsoft. Cuando se habilita un emparejamiento, la sesión de BGP en la conexión principal y en la conexión secundaria del circuito ExpressRoute se desactivará. Volverá a establecer la conectividad mediante este emparejamiento a Microsoft. Puede habilitar y deshabilitar el emparejamiento de Microsoft y el emparejamiento privado de Azure en un circuito de ExpressRoute de forma independiente. La primera vez que configure los emparejamientos en el circuito ExpressRoute, estos se habilitan de forma predeterminada.

Hay un par de escenarios en los que puede resultarle útil restablecer los emparejamientos de ExpressRoute.
* Pruebe la implementación y el diseño de recuperación ante desastres. Por ejemplo, tiene dos circuitos de ExpressRoute. Puede deshabilitar los emparejamientos de un circuido y forzar la conmutar por error del tráfico de red a otro circuito.
* Habilite la detección de reenvío bidireccional (BFD) en el emparejamiento privado de Azure o el emparejamiento de Microsoft de su circuito de ExpressRoute. BFD se habilita de forma predeterminada en el emparejamiento privado de Azure si el circuito ExpressRoute se crea después del 1 de agosto de 2018 y en el emparejamiento de Microsoft. Si el circuito ExpressRoute se crea después del 10 de enero de 2020. Si el circuito se creó antes de esa fecha, BFD no estará habilitado. Para habilitar BFD, debe deshabilitar el emparejamiento y volverlo a habilitar. 

### <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

## <a name="reset-a-peering"></a>Restablecimiento de un emparejamiento

1. Seleccione el circuito en el que quiere realizar cambios en la configuración de emparejamiento.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit-list.png" alt-text="Lista de circuitos ExpressRoute":::

1. Seleccione la configuración de emparejamiento que quiere habilitar o deshabilitar.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit.png" alt-text="Información general del circuito ExpressRoute":::

1. Desactive **Enable Peering** (Habilitar emparejamiento) y seleccione **Guardar** para deshabilitar la configuración de emparejamiento.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/disable-peering.png" alt-text="Deshabilitación del emparejamiento privado":::

1. Para volver a habilitar el emparejamiento, active **Enable Peering** (Habilitar emparejamiento) y seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes
Si necesita ayuda para solucionar un problema de ExpressRoute, consulte los artículos siguientes:
* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solución de problemas de rendimiento de red](expressroute-troubleshooting-network-performance.md)
