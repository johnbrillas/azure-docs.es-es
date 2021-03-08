---
title: Configuración de reglas NAT de VPN para una puerta de enlace
titleSuffix: Azure Virtual WAN
description: Aprenda a configurar las reglas NAT para una puerta de enlace de VPN VWAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 02/17/2021
ms.author: cherylmc
ms.openlocfilehash: a31b3718eb1baa32aef39474383924efe8cf93b6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743935"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Configuración de reglas NAT para la puerta de enlace de VPN de Virtual WAN (versión preliminar)

> [!IMPORTANT]
> Las reglas NAT se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede configurar una puerta de enlace de VPN de Virtual WAN con reglas NAT estáticas de uno a uno. Una regla NAT proporciona un mecanismo para configurar la traducción uno a uno de las direcciones IP. NAT se puede usar para interconectar dos redes IP que tienen direcciones IP incompatibles o superpuestas. Un escenario típico son las ramas con direcciones IP superpuestas que quieren acceder a los recursos de la red virtual de Azure.

Esta configuración usa una tabla de flujos para enrutar el tráfico de una dirección IP externa (host) a una dirección IP interna asociada a un punto de conexión dentro de una red virtual (máquina virtual, equipo, contenedor, etc.).

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Diagrama que muestra la arquitectura.":::

## <a name="configure-and-view-rules"></a><a name="view"></a>Configuración y visualización de reglas

Puede configurar y ver las reglas NAT en la configuración de la puerta de enlace de VPN en cualquier momento.

   :::image type="content" source="./media/nat-rules-vpn-gateway/edit-rules.png" alt-text="Captura de pantalla que muestra la edición de reglas.":::

1. Vaya al centro virtual.
1. Seleccione **VPN (de sitio a sitio)**
1. Seleccione **NAT rules (Edit)** (Reglas NAT [editar]).
1. En la página **Edit NAT Rule** (Editar regla NAT), puede **Agregar/Editar/Eliminar** una regla NAT con los siguientes valores:

   * **Nombre:** Nombre único para la regla NAT.
   * **Tipo:** Estático. Una regla NAT estática de uno a uno establece una relación de uno a uno entre una dirección interna y una dirección externa.
   * **Modo:** IngressSnat o EgressSnat.  
      * El modo IngressSnat (también conocido como NAT de origen de entrada) es aplicable al tráfico que entra en la puerta de enlace de VPN de sitio a sitio del centro de Azure.
      * El modo EgressSnat (también conocido como NAT de origen de salida) es aplicable al tráfico que sale de la puerta de enlace de VPN de sitio a sitio del centro de Azure.
   * **InternalMapping:** Intervalo de prefijos de dirección de las direcciones IP de origen en la red interna que se asignará a un conjunto de direcciones IP externas. En otras palabras, el intervalo de prefijos de dirección antes de NAT.
   * **ExternalMapping:** Intervalo de prefijos de dirección de las direcciones IP de destino en la red externa a la que se asignarán las direcciones IP de origen. En otras palabras, el intervalo de prefijos de dirección después de NAT.
   * **Link Connection** (Conexión de vínculo): Recurso de conexión que conecta virtualmente un sitio VPN a la puerta de enlace de VPN de sitio a sitio del centro de Azure.

### <a name="configuration-considerations"></a>Consideraciones de configuración

* El tamaño de la subred para la asignación interna y externa debe ser el mismo para la regla NAT estática de uno a uno.
* Asegúrese de editar el sitio VPN en Azure Portal para agregar prefijos de **ExternalMapping** en el campo "Espacio de direcciones privadas". Actualmente, los sitios que tienen BGP habilitado deben asegurarse de que el anunciante de BGP local (configuración de BGP del dispositivo) incluya una entrada para los prefijos de asignación externa.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las configuraciones de sitio a sitio, consulte [Configuración de una conexión de sitio a sitio de Virtual WAN](virtual-wan-site-to-site-portal.md).
