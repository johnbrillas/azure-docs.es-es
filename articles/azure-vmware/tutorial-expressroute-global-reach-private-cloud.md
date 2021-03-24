---
title: 'Tutorial: Emparejamiento de entornos locales con una nube privada'
description: Aprenda a crear el emparejamiento de Global Reach de ExpressRoute y una nube privada en Azure VMware Solution.
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: ae92bf89a08c5fade8757e3ee596c4ed4a5e6389
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103494189"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Emparejamiento de entornos locales con una nube privada

Global Reach de ExpressRoute conecta su entorno local y la nubes privada de Azure VMware Solution. La conexión de Global Reach de ExpressRoute se establece entre el circuito ExpressRoute de la nube privada y una conexión existente de ExpressRoute con los entornos locales. 

El circuito ExpressRoute que se usa al [configurar redes para la nube privada de VMware en Azure](tutorial-configure-networking.md) requiere la creación y el uso de claves de autorización.  Ya habrá usado una clave de autorización del circuito ExpressRoute y, en este tutorial, va a crear una segunda clave de autorización para el emparejamiento con el circuito ExpressRoute local.

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Crear una segunda clave de autorización para _circuit 2_, el circuito ExpressRoute de nube privada.
> * Usar la CLI de Azure o Azure Portal en un método de Cloud Shell, en la suscripción de _circuit 1_ para habilitar el emparejamiento de Global Reach de ExpressRoute entre el entorno local y la nube privada.


## <a name="before-you-begin"></a>Antes de empezar

Antes de habilitar la conectividad entre dos circuitos ExpressRoute mediante Global Reach, consulte la documentación sobre cómo [habilitar la conectividad en distintas suscripciones de Azure](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions).  

## <a name="prerequisites"></a>Requisitos previos

- Establecimiento de conectividad a y desde una nube privada de Azure VMware Solution con su circuito ExpressRoute emparejado con una puerta de enlace de ExpressRoute en una red virtual (VNet) de Azure, que es circuit 2 en los procedimientos de emparejamiento.
- Un circuito ExpressRoute independiente y funcional utilizado para conectar entornos locales con Azure, que es circuit 1 desde la perspectiva de los procedimientos de emparejamiento.
- Un [bloque de direcciones de la red](../expressroute/expressroute-routing.md#ip-addresses-used-for-peerings) /29 sin superposición para el emparejamiento de Global Reach de ExpressRoute.
- Asegúrese de que todas las puertas de enlace, incluido el servicio del proveedor de ExpressRoute, admitan el número de sistema autónomo (ASN) de 4 bytes. Azure VMware Solution utiliza ASN públicos de 4 bytes para anunciar rutas.

>[!IMPORTANT]
>En el contexto de estos requisitos previos, su circuito ExpressRoute local es _circuit 1_, y su circuito ExpressRoute de nube privada está en otra suscripción y se denomina _circuit 2_.

## <a name="create-an-expressroute-authorization-key-in-the-on-premises-circuit"></a>Creación de una clave de autorización de ExpressRoute en el circuito local

[!INCLUDE [request-authorization-key](includes/request-authorization-key.md)]
 
## <a name="peer-private-cloud-to-on-premises-with-authorization-key"></a>Emparejamiento entre el entorno local y la nube privada con clave de autorización
Ahora que ha creado una clave de autorización para el circuito ExpressRoute de la nube privada, puede emparejarlo con el circuito ExpressRoute local. El emparejamiento se realiza desde la perspectiva del circuito ExpressRoute local en **Azure Portal** o mediante la **CLI de Azure en un método de Cloud Shell**. Con ambos métodos, se usará el identificador de recurso y la clave de autorización del circuito ExpressRoute de la nube privada para finalizar el emparejamiento.

### <a name="portal"></a>[Portal](#tab/azure-portal)
 
1. Inicie sesión en [Azure Portal](https://portal.azure.com) mediante la misma suscripción que el circuito ExpressRoute local.

1. En la sección de **información general** de la nube privada, en Administrar, seleccione **Conectividad** > **Global Reach de ExpressRoute** > **Agregar**.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="Captura de pantalla que muestra la pestaña Global Reach de ExpressRoute en la nube privada de Azure VMware Solution.":::

1. Cree una conexión entre el entorno local y la nube. Realice una de las acciones siguientes y seleccione **Crear**:

   - Seleccione el **circuito ExpressRoute** de la lista.
   - O, si tiene el identificador del circuito, péguelo en el campo y proporcione la clave de autorización.

   :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Escriba el identificador de ExpressRoute, la clave de autorización y, a continuación, seleccione Crear.":::   
   
   La nueva conexión se muestra en la lista de conexiones a la nube local.

>[!TIP]
>Para eliminar o desconectar una conexión de la lista, seleccione **Más**.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Desconexión o eliminación de una conexión local":::

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Hemos ampliado los [comandos de la CLI](../expressroute/expressroute-howto-set-global-reach-cli.md) para incluir detalles y ejemplos específicos que le ayudarán a configurar el emparejamiento de Global Reach de ExpressRoute entre entornos locales y una nube privada de Azure VMware Solution.

>[!TIP]
>Para una salida más breve del comando de la CLI de Azure, estas instrucciones pueden usar un argumento [`–query`](https://docs.microsoft.com/cli/azure/query-azure-cli) para ejecutar una consulta JMESPath y que se muestren solo los resultados necesarios.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) mediante la misma suscripción que el circuito ExpressRoute local. 

1. Abra una instancia de Cloud Shell y deje el shell como bash.

   :::image type="content" source="media/expressroute-global-reach/azure-portal-cloud-shell.png" alt-text="Captura de pantalla que muestra la instancia de Cloud Shell de Azure Portal.":::

1. Cree el emparejamiento con el circuito 1, pasando el identificador de recurso y la clave de autorización del circuito 2. 

   ```azurecli-interactive
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

   :::image type="content" source="media/expressroute-global-reach/azure-command-with-results.png" alt-text="Captura de pantalla que muestra el comando y los resultados de un emparejamiento correcto entre el circuito 1 y el circuito 2.":::

Puede conectarse desde los entornos locales hasta la nube privada mediante el emparejamiento de Global Reach de ExpressRoute.

>[!TIP]
>Para eliminar el emparejamiento, siga las instrucciones de [Deshabilitación de la conectividad entre las redes locales](../expressroute/expressroute-howto-set-global-reach-cli.md#disable-connectivity-between-your-on-premises-networks).


---

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a habilitar el emparejamiento de Global Reach de ExpressRoute del entorno local a la nube privada. 

Continúe con el siguiente tutorial para aprender a implementar y configurar la solución VMware HCX para su nube privada de Azure VMware Solution.

> [!div class="nextstepaction"]
> [Implementación y configuración de VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
