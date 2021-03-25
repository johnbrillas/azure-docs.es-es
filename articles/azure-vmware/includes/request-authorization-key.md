---
title: Solicitud de la clave de autorización para ExpressRoute
description: Pasos para solicitar una clave de autorización para ExpressRoute.
ms.topic: include
ms.date: 03/15/2021
ms.openlocfilehash: 54a610c8b0f3f3fe9d3ebe39291bba7767007839
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491861"
---
<!-- used in expressroute-global-reach-private-cloud.md and create-ipsec-tunnel.md -->

1. En Azure Portal, en la sección **Conectividad**, en la pestaña **ExpressRoute**, seleccione **+ Solicitar una clave de autorización**. 

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Captura de pantalla que muestra cómo solicitar una clave de autorización de ExpressRoute." border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Proporciónele un nombre y luego seleccione **Crear**. 
      
   La clave puede tardar unos 30 segundos en crearse. Una vez creada, la nueva clave aparece en la lista de claves de autorización para la nube privada.

   :::image type="content" source="../media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Captura de pantalla que muestra la clave de autorización de ExpressRoute Global Reach.":::
  
1. Tome nota de la clave de autorización y el identificador de ExpressRoute. Los usará para completar el emparejamiento.  

   > [!NOTE]
   > La clave de autorización desaparece tras un tiempo, por lo que debe copiarla en cuanto aparezca.