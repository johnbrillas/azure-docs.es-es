---
title: Configuración del proxy del Centro de distribución de claves de Kerberos en Windows Virtual Desktop - Azure
description: Cómo configurar un grupo de hosts de Windows Virtual Desktop para usar un servidor proxy del Centro de distribución de claves Kerberos.
author: Heidilohr
ms.topic: how-to
ms.date: 01/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: f123659941eaeb3b8ceeb6a999abf836eb9cf5ea
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798028"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy"></a>Configuración de un proxy del Centro de distribución de claves de Kerberos

En este artículo se muestra cómo configurar un proxy del Centro de distribución de claves de Kerberos (KDC) para el grupo de hosts. Este proxy permite que las organizaciones se autentiquen con Kerberos fuera de los límites de su empresa. Por ejemplo, puede usar el proxy de KDC para habilitar la autenticación con tarjeta inteligente para clientes externos.

## <a name="how-to-configure-the-kdc-proxy"></a>Configuración del proxy de KDC

Para configurar el proxy de KDC:

1. Inicie sesión en Azure Portal como administrador.

2. Vaya a la página de Windows Virtual Desktop.

3. Seleccione el grupo de hosts para el que desea habilitar el proxy de KDC y, a continuación, seleccione **Propiedades de RDP**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la página de Azure Portal en la que se muestra un usuario que selecciona grupos de host, el nombre del grupo host de ejemplo y, a continuación, las propiedades de RDP.](media/rdp-properties.png)

4. Seleccione la pestaña **Opciones avanzadas** y, a continuación, escriba un valor con el siguiente formato sin espacios:

    > kdcproxyname:s:\<fqdn\>

    > [!div class="mx-imgBorder"]
    > ![Una captura de pantalla que muestra la pestaña de opciones avanzadas seleccionada, con el valor especificado como se describe en el paso 4.](media/advanced-tab-selected.png)

5. Seleccione **Guardar**.

6. El grupo de hosts seleccionado ahora debe empezar a emitir archivos de conexión de RDP con el campo kdcproxyname especificado.

## <a name="next-steps"></a>Pasos siguientes

El rol RDGateway en Servicios de Escritorio remoto incluye un servicio de proxy de KDC. Consulte [Implementación del rol Puerta de enlace de Escritorio remoto en Windows Virtual Desktop](rd-gateway-role.md) para configurar uno como destino para Windows Virtual Desktop.
