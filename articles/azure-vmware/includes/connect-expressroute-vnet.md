---
title: Conexión de ExpressRoute a la puerta de enlace de red virtual
description: Pasos para conectar ExpressRoute a la puerta de enlace de red virtual.
ms.topic: include
ms.date: 12/08/2020
ms.openlocfilehash: 5f9a565a7662041dbd85e61388129496fa376962
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861529"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. Vaya a la nube privada que creó en el tutorial de [implementación del clúster de vSphere en Azure](../tutorial-create-private-cloud.md). Elija **Conectividad** en **Administrar** y, a continuación, seleccione la pestaña **ExpressRoute**.

1. Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione **+ Solicitar una clave de autorización**.

   :::image type="content" source="../media/expressroute-global-reach/start-request-auth-key.png" alt-text="Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione + Solicitar una clave de autorización." border="true" lightbox="../media/expressroute-global-reach/start-request-auth-key.png":::

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

   :::image type="content" source="../media/expressroute-global-reach/open-cloud-shell.png" alt-text="En la página Agregar conexión, proporcione valores para los campos y seleccione Aceptar." border="true" lightbox="../media/expressroute-global-reach/open-cloud-shell.png":::

Se crea la conexión entre el circuito ExpressRoute y la red virtual.