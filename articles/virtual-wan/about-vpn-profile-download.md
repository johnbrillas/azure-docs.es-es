---
title: 'Azure Virtual WAN: Perfiles de cliente de VPN de usuario'
description: Esto le ayuda a trabajar con el archivo de perfil del cliente
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 02/08/2021
ms.author: cherylmc
ms.openlocfilehash: d83b6ed2ae83db569d3c61e3cf4cd887f875eb25
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99980922"
---
# <a name="working-with-user-vpn-client-profile-files"></a>Uso de archivos de perfil de cliente VPN de usuario

Los archivos de perfil contienen información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN de usuario.

## <a name="download-the-profile"></a>Descarga del perfil

Para descargar el archivo ZIP de perfiles de clientes siga los pasos del artículo [Descarga de perfiles](global-hub-profile.md).

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configuración de clientes de OpenVPN](../virtual-wan/howto-openvpn-clients.md#windows).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la VPN de usuario de Virtual WAN, consulte [Creación de una conexión VPN de usuario](virtual-wan-point-to-site-portal.md).
