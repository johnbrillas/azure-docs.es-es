---
title: 'Tutorial: Emparejamiento de entornos locales con una nube privada'
description: Aprenda a crear el emparejamiento de Global Reach de ExpressRoute y una nube privada en Azure VMware Solution.
ms.topic: tutorial
ms.date: 01/27/2021
ms.openlocfilehash: 04be94849f1cd78ff915ca271845b283c8cfc64c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100558791"
---
# <a name="tutorial-peer-on-premises-environments-to-a-private-cloud"></a>Tutorial: Emparejamiento de entornos locales con una nube privada

Global Reach de ExpressRoute conecta su entorno local y la nubes privada de Azure VMware Solution. La conexión de Global Reach de ExpressRoute se establece entre el circuito ExpressRoute de la nube privada y una conexión existente de ExpressRoute con los entornos locales. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Utilizar Azure Portal para habilitar el emparejamiento de Global Reach de ExpressRoute del entorno local a la nube privada.


## <a name="before-you-begin"></a>Antes de empezar

Antes de habilitar la conectividad entre dos circuitos ExpressRoute mediante Global Reach, consulte la documentación sobre cómo [habilitar la conectividad en distintas suscripciones de Azure](../expressroute/expressroute-howto-set-global-reach-cli.md#enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions).  


## <a name="prerequisites"></a>Requisitos previos

- Un circuito de ExpressRoute independiente y funcional utilizado para conectar los entornos locales con Azure.
- Asegúrese de que todas las puertas de enlace, incluido el servicio del proveedor de ExpressRoute, admitan el número de sistema autónomo (ASN) de 4 bytes. Azure VMware Solution utiliza ASN públicos de 4 bytes para anunciar rutas.


## <a name="create-an-expressroute-authorization-key-in-the-on-premises-expressroute-circuit"></a>Creación de una clave de autorización de ExpressRoute en el circuito de ExpressRoute local

1. En la hoja **Circuitos de ExpressRoute**, en Configuración, seleccione **Autorizaciones**.

2. Escriba el nombre de la clave de autorización y seleccione **Guardar**.

    :::image type="content" source="media/expressroute-global-reach/start-request-auth-key-on-premises-expressroute.png" alt-text="Seleccione Autorizaciones y escriba el nombre de la clave de autorización.":::
  
     Una vez creada, la nueva clave aparece en la lista de claves de autorización del circuito.
 
 4. Tome nota de la clave de autorización y el identificador de ExpressRoute. Los usará en el paso siguiente para completar el emparejamiento.
 
 
 ## <a name="peer-private-cloud-to-on-premises"></a>Emparejamiento de una nube privada con el entorno local

1. En la sección de **información general** de la nube privada, en Administrar, seleccione **Conectividad > Global Reach de ExpressRoute > Agregar**.

    :::image type="content" source="./media/expressroute-global-reach/expressroute-global-reach-tab.png" alt-text="En el menú, seleccione Conectividad, la pestaña Global Reach de ExpressRoute y, a continuación, Agregar.":::

2. Escriba el identificador de ExpressRoute y la clave de autorización creada en la sección anterior.

    :::image type="content" source="./media/expressroute-global-reach/on-premises-cloud-connections.png" alt-text="Escriba el identificador de ExpressRoute, la clave de autorización y, a continuación, seleccione Crear.":::

3. Seleccione **Crear**. La nueva conexión se muestra en la lista de conexiones a la nube local.  

>[!TIP]
>Para eliminar o desconectar una conexión de la lista, seleccione **Más**.  
>
> :::image type="content" source="./media/expressroute-global-reach/on-premises-connection-disconnect.png" alt-text="Desconexión o eliminación de una conexión local":::


## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a habilitar el emparejamiento de Global Reach de ExpressRoute del entorno local a la nube privada. 

Continúe con el siguiente tutorial para aprender a implementar y configurar la solución VMware HCX para su nube privada de Azure VMware Solution.

> [!div class="nextstepaction"]
> [Implementación y configuración de VMware HCX](tutorial-deploy-vmware-hcx.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
