---
title: Conexión de ExpressRoute a la puerta de enlace de red virtual
description: Pasos para conectar ExpressRoute a la puerta de enlace de red virtual.
ms.topic: include
ms.date: 12/08/2020
ms.openlocfilehash: 6e2e3748dbfd8d69b53dcc4c3a09809756ac48dc
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103494377"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. Vaya a la nube privada que creó en el tutorial de [implementación del clúster de vSphere en Azure](../tutorial-create-private-cloud.md). Elija **Conectividad** en **Administrar** y, a continuación, seleccione la pestaña **ExpressRoute**.

1. Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione **+ Solicitar una clave de autorización**.

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione + Solicitar una clave de autorización." border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Vaya a la puerta de enlace de red virtual que creó en el paso anterior y, en **Configuración**, seleccione **Conexiones**. En la página **Conexiones**, seleccione **+ Agregar**.

1. En la página **Agregar conexión**, proporcione valores para los campos y seleccione **Aceptar**. 

   | Campo | Valor |
   | --- | --- |
   | **Nombre**  | Escriba un nombre para la conexión.  |
   | **Tipo de conexión**  | seleccione **ExpressRoute**.  |
   | **Canjear autorización**  | Asegúrese de que este cuadro está seleccionado.  |
   | **Puerta de enlace de red virtual** | La puerta de enlace de red virtual que creó antes.  |
   | **Clave de autorización**  | Copie y pegue la clave de autorización de la pestaña de ExpressRoute para el grupo de recursos. |
   | **URI de circuito del mismo nivel**  | Copie y pegue el identificador de ExpressRoute de la pestaña de ExpressRoute para el grupo de recursos.  |

   :::image type="content" source="../media/expressroute-global-reach/expressroute-add-connection.png" alt-text="Captura de pantalla del panel Agregar conexión para conectar ExpressRoute a la puerta de enlace de red virtual.":::

Se crea la conexión entre el circuito ExpressRoute y la red virtual.

:::image type="content" source="../media/expressroute-global-reach/virtual-network-gateway-connections.png" alt-text="Captura de pantalla de las conexiones de puerta de enlace de red virtual.":::