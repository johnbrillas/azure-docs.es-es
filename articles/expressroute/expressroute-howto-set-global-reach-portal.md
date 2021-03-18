---
title: 'Azure ExpressRoute: Configuración de Global Reach con Azure Portal'
description: Este artículo le ayudará a vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales y habilitar Global Reach mediante Azure Portal.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/05/2021
ms.author: duau
ms.openlocfilehash: 336bd4aaf881b7315921ef374c92a2ac95ff3c8c
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431322"
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

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-global-reach-list.png" alt-text="Captura de pantalla de la lista de circuitos de ExpressRoute.":::

## <a name="enable-connectivity"></a>Habilitación de la conectividad

Habilite la conectividad entre las redes locales. Existen diferentes conjuntos de instrucciones para los circuitos de la misma suscripción de Azure y los circuitos de distintas suscripciones.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Circuitos ExpressRoute en la misma suscripción de Azure

1. Seleccione la configuración de emparejamiento **Azure privado**. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Captura de pantalla de la página de información general de ExpressRoute.":::

1. Seleccione **Agregar Global Reach** para abrir la página de configuración *Agregar Global Reach*.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Habilitar Global Reach desde el emparejamiento privado":::

1. En la página de configuración *Agregar Global Reach*, asigne un nombre a esta configuración. Seleccione el *circuito ExpressRoute* al que quiere conectar este circuito y escriba en una dirección **/29 IPv4** para la *subred de Global Reach*. Las direcciones IP de esta subred se usan para establecer conectividad entre los dos circuitos ExpressRoute. No use las direcciones de esta subred en las redes virtuales de Azure ni en la red local. Seleccione **Agregar** para agregar el circuito a la configuración de emparejamiento privado.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration.png" alt-text="Captura de pantalla de la adición de Global Reach en el emparejamiento privado.":::

1. Seleccione **Guardar** para completar la configuración de Global Reach. Una vez concluida la operación, tendrá conectividad entre las dos redes locales a través de ambos circuitos ExpressRoute.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Captura de pantalla del almacenamiento de configuraciones de emparejamiento privado":::

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos ExpressRoute en distintas suscripciones de Azure

Si los dos circuitos no están en la misma suscripción de Azure, necesitará autorización. En la siguiente configuración, la autorización se genera a partir de la suscripción del circuito 2. A continuación, se pasa la clave de autorización al circuito 1.

1. Genere una clave de autorización.

   :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/create-authorization-expressroute-circuit.png" alt-text="Captura de pantalla de la generación de la clave de autorización."::: 

   Anote el identificador de recurso del circuito 2 y la clave de autorización.

1. Seleccione la configuración de emparejamiento **Azure privado**. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Captura de pantalla del emparejamiento privado en la página de información general.":::

1. Seleccione **Agregar Global Reach** para abrir la página de configuración *Agregar Global Reach*.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Captura de pantalla de la adición de Global Reach en el emparejamiento privado.":::

1. En la página de configuración *Agregar Global Reach*, asigne un nombre a esta configuración. Seleccione la casilla **Canjear autorización**. Escriba la **Clave de autorización** y el **Id. de circuito ExpressRoute** generados y obtenidos en el paso 1. A continuación, proporcione un **/29 IPv4** para la *subred de Global Reach*. Las direcciones IP de esta subred se usan para establecer conectividad entre los dos circuitos ExpressRoute. No use las direcciones de esta subred en las redes virtuales de Azure ni en la red local. Seleccione **Agregar** para agregar el circuito a la configuración de emparejamiento privado.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration-with-authorization.png" alt-text="Captura de pantalla de la adición de Global Reach con la clave de autorización.":::

1. Seleccione **Guardar** para completar la configuración de Global Reach. Una vez concluida la operación, tendrá conectividad entre las dos redes locales a través de ambos circuitos ExpressRoute.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Captura de pantalla del almacenamiento de la configuración del emparejamiento privado con Global Reach.":::

## <a name="verify-the-configuration"></a>Comprobación de la configuración

Compruebe la configuración de Global Reach al seleccionar el *emparejamiento privado* en la configuración del circuito ExpressRoute. Cuando se configura correctamente, las opciones deben ser similares a lo siguiente:

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/verify-global-reach-configuration.png" alt-text="Captura de pantalla de Global Reach configurado.":::

## <a name="disable-connectivity"></a>Deshabilitar la conectividad

Para deshabilitar la conectividad entre un circuito individual, seleccione el botón Eliminar situado junto al *nombre de Global Reach* para quitar la conectividad entre ellos. A continuación, seleccione **Guardar** para completar la operación.

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/disable-global-reach-configuration.png" alt-text="Captura de pantalla que muestra cómo deshabilitar Global Reach.":::

Tras finalizar la operación, dejará de tener conectividad dentro de la red local a través de los circuitos ExpressRoute.

## <a name="next-steps"></a>Pasos siguientes
- [Más información acerca de Global Reach de ExpressRoute](expressroute-global-reach.md)
- [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
- [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)
