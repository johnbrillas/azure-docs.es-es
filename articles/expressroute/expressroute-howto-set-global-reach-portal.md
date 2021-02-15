---
title: 'Azure ExpressRoute: Configuración de Global Reach con Azure Portal'
description: Este artículo le ayudará a vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales y habilitar Global Reach mediante Azure Portal.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 01/11/2021
ms.author: duau
ms.openlocfilehash: 8366978d50875389ce872c2d1402f0defa2a7371
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539356"
---
# <a name="configure-expressroute-global-reach-using-the-azure-portal"></a>Configuración de Global Reach de ExpressRoute mediante Azure Portal

Este artículo le ayuda a configurar Global Reach de ExpressRoute con PowerShell. Para más información, consulte [Global Reach de ExpressRoute](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Antes de empezar

Antes de comenzar la configuración, confirme los siguientes criterios:

* Conoce los [flujos de trabajo](expressroute-workflows.md) del aprovisionamiento de circuitos ExpressRoute.
* Los circuitos ExpressRoute están en estado aprovisionado.
* El emparejamiento privado de Azure está configurado en los circuitos ExpressRoute.
* Si quiere ejecutar PowerShell localmente, compruebe que la versión más reciente de Azure PowerShell está instalada en el equipo.

## <a name="identify-circuits"></a>Identificar circuitos

1. Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

2. Identifique los circuitos ExpressRoute que quiere usar. Puede habilitar Global Reach de ExpressRoute entre el emparejamiento privado de dos circuitos ExpressRoute cualesquiera, siempre que se encuentren en los países o regiones admitidos. Los circuitos se deben crear en diferentes ubicaciones de emparejamiento. 

   * Si ambos circuitos pertenecen a la suscripción, puede elegir cualquiera de ellos para ejecutar la configuración en las secciones siguientes.
   * Si los circuitos están en diferentes suscripciones de Azure, necesita la autorización de una suscripción de Azure. Luego debe usar la clave de autorización al ejecutar el comando de configuración en la otra suscripción de Azure.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-global-reach-list.png" alt-text="Lista de circuitos ExpressRoute":::

## <a name="enable-connectivity"></a>Habilitación de la conectividad

Habilite la conectividad entre las redes locales. Existen diferentes conjuntos de instrucciones para los circuitos de la misma suscripción de Azure y los circuitos de distintas suscripciones.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Circuitos ExpressRoute en la misma suscripción de Azure

1. Seleccione la configuración de emparejamiento **Azure privado**. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Información general del emparejamiento de ExpressRoute":::

1. Active la casilla **Enable Global Reach** (habilitar Global Reach) y, a continuación, seleccione **Add Global Reach** (agregar Global Reach) para abrir la página de configuración *Agregar Global Reach*.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Habilitar Global Reach desde el emparejamiento privado":::

1. En la página de configuración *Agregar Global Reach*, asigne un nombre a esta configuración. Seleccione el *circuito ExpressRoute* al que quiere conectar este circuito y escriba en una dirección **/29 IPv4** para la *subred de Global Reach*. Las direcciones IP de esta subred se usan para establecer conectividad entre los dos circuitos ExpressRoute. No use las direcciones de esta subred en las redes virtuales de Azure ni en la red local. Seleccione **Agregar** para agregar el circuito a la configuración de emparejamiento privado.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration.png" alt-text="Página de configuración de Global Reach":::

1. Seleccione **Guardar** para completar la configuración de Global Reach. Una vez concluida la operación, tendrá conectividad entre las dos redes locales a través de ambos circuitos ExpressRoute.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Guardado de la configuración de emparejamiento privado":::

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos ExpressRoute en distintas suscripciones de Azure

Si los dos circuitos no están en la misma suscripción de Azure, necesitará autorización. En la siguiente configuración, la autorización se genera a partir de la suscripción del circuito 2. A continuación, se pasa la clave de autorización al circuito 1.

1. Genere una clave de autorización.

   :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/create-authorization-expressroute-circuit.png" alt-text="Generación de una clave de autorización"::: 

   Anote el identificador de recurso del circuito 2 y la clave de autorización.

1. Seleccione la configuración de emparejamiento **Azure privado**. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Información general del emparejamiento de circuito 1":::

1. Active la casilla **Enable Global Reach** (habilitar Global Reach) y, a continuación, seleccione **Add Global Reach** (agregar Global Reach) para abrir la página de configuración *Agregar Global Reach*.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Habilitación de Global Reach desde el circuito 1":::

1. En la página de configuración *Agregar Global Reach*, asigne un nombre a esta configuración. Seleccione la casilla **Canjear autorización**. Escriba la **Clave de autorización** y el **Id. de circuito ExpressRoute** generados y obtenidos en el paso 1. A continuación, proporcione un **/29 IPv4** para la *subred de Global Reach*. Las direcciones IP de esta subred se usan para establecer conectividad entre los dos circuitos ExpressRoute. No use las direcciones de esta subred en las redes virtuales de Azure ni en la red local. Seleccione **Agregar** para agregar el circuito a la configuración de emparejamiento privado.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration-with-authorization.png" alt-text="Incorporación de Global Reach con la clave de autorización":::

1. Seleccione **Guardar** para completar la configuración de Global Reach. Una vez concluida la operación, tendrá conectividad entre las dos redes locales a través de ambos circuitos ExpressRoute.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Guardado de la configuración de emparejamiento privado en el circuito 1":::

## <a name="verify-the-configuration"></a>Comprobación de la configuración

Compruebe la configuración de Global Reach al seleccionar el *emparejamiento privado* en la configuración del circuito ExpressRoute. Cuando se configura correctamente, las opciones deben ser similares a lo siguiente:

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/verify-global-reach-configuration.png" alt-text="Comprobación de la configuración de Global Reach":::

## <a name="disable-connectivity"></a>Deshabilitar la conectividad

Tiene dos opciones para deshabilitar Global Reach. Para deshabilitar la conectividad entre todos los circuitos, desactive **Enable Global Reach** (habilitar Global Reach) para deshabilitar la conectividad entre todos los circuitos. Para deshabilitar la conectividad entre un circuito individual, seleccione el botón Eliminar situado junto al *nombre de Global Reach* para quitar la conectividad entre ellos. A continuación, seleccione **Guardar** para completar la operación.

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/disable-global-reach-configuration.png" alt-text="Deshabilitación de la configuración de Global Reach":::

Tras finalizar la operación, dejará de tener conectividad dentro de la red local a través de los circuitos ExpressRoute.

## <a name="next-steps"></a>Pasos siguientes
1. [Más información acerca de Global Reach de ExpressRoute](expressroute-global-reach.md)
2. [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
3. [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)
