---
title: Configuración de reglas NAT de VPN para una puerta de enlace
titleSuffix: Azure Virtual WAN
description: Aprenda a configurar las reglas NAT para una puerta de enlace de VPN VWAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/05/2021
ms.author: cherylmc
ms.openlocfilehash: 6fbee31f015953bd7e65648ea273e3ca84686115
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431223"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Configuración de reglas NAT para la puerta de enlace de VPN de Virtual WAN (versión preliminar)

> [!IMPORTANT]
> Las reglas NAT se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin ningún acuerdo de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede configurar una puerta de enlace de VPN de Virtual WAN con reglas NAT estáticas de uno a uno. Una regla NAT proporciona un mecanismo para configurar la traducción uno a uno de las direcciones IP. NAT se puede usar para interconectar dos redes IP que tienen direcciones IP incompatibles o superpuestas. Un escenario típico son las ramas con direcciones IP superpuestas que quieren acceder a los recursos de la red virtual de Azure.

Esta configuración usa una tabla de flujos para enrutar el tráfico de una dirección IP externa (host) a una dirección IP interna asociada a un punto de conexión dentro de una red virtual (máquina virtual, equipo, contenedor, etc.).

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Diagrama que muestra la arquitectura.":::

## <a name="configure-nat-rules"></a><a name="rules"></a>Configuración de reglas NAT

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

### <a name="configuration-considerations"></a><a name="considerations"></a>Consideraciones de configuración

* El tamaño de la subred para la asignación interna y externa debe ser el mismo para la regla NAT estática de uno a uno.
* Asegúrese de editar el sitio VPN en Azure Portal para agregar prefijos de **ExternalMapping** en el campo "Espacio de direcciones privadas". Actualmente, los sitios que tienen BGP habilitado deben asegurarse de que el anunciante de BGP local (configuración de BGP del dispositivo) incluya una entrada para los prefijos de asignación externa.

## <a name="examples-and-verification"></a><a name="examples"></a>Ejemplos y comprobación

### <a name="ingress-mode-nat"></a>NAT de modo de entrada

Las reglas NAT de modo de entrada se aplican a los paquetes que entran en Azure a través de la instancia de VPN Gateway de sitio a sitio de Virtual WAN. En este escenario, puede conectar dos ramas de VPN de sitio a sitio a Azure. El sitio VPN 1 se conecta a través del vínculo Link 1 y el sitio VPN 2 se conecta a través del vínculo Link 2. Cada sitio tiene el espacio de direcciones 192.169.1.0/24.

En el diagrama siguiente se muestra el resultado final previsto:

:::image type="content" source="./media/nat-rules-vpn-gateway/ingress.png" alt-text="Diagrama que muestra las reglas NAT de modo de entrada.":::

1. Especifique una regla NAT.

   Especifique una regla NAT para asegurarse de que la instancia de VPN Gateway de sitio a sitio puede distinguir entre las dos ramas con espacios de direcciones superpuestos (por ejemplo, 192.168.1.0/24). En este ejemplo, nos centramos en el vínculo Link 1 para el sitio VPN 1.

   La siguiente regla NAT se puede configurar y asociar al vínculo Link 1 de una de las ramas. Dado que se trata de una regla NAT estática, los espacios de direcciones de InternalMapping y ExternalMapping contienen el mismo número de direcciones IP.

   * **Nombre:** IngressRule01
   * **Tipo:** Estático.
   * **Modo:** IngressSnat.
   * **InternalMapping:** 192.168.1.0/24
   * **ExternalMapping:** 10.1.1.0/24
   * **Conexión de vínculo:** Link 1

1. Anuncie el espacio de direcciones ExternalMapping correcto.

   En este paso, asegúrese de que la instancia de VPN Gateway de sitio a sitio anuncia el espacio de direcciones ExternalMapping correcto al resto de los recursos de Azure. Hay distintas instrucciones, según si BGP está habilitado o no.

   **Ejemplo 1: BGP habilitado**

   * Asegúrese de que el altavoz BGP local ubicado en el sitio VPN 1 está configurado para anunciar el espacio de direcciones 10.1.1.0/24.
   * Durante la versión preliminar, los sitios que tienen BGP habilitado deben asegurarse de que el anunciante de BGP local (configuración de BGP del dispositivo) incluye una entrada para los prefijos de asignación externa.

   **Ejemplo 2: BGP no habilitado**

   * Navegue hasta el recurso de centro virtual que contiene la instancia de VPN Gateway de sitio a sitio. En la página del centro virtual, en **Conectividad**, seleccione **VPN (sitio a sitio)** .
   * Seleccione el sitio VPN que está conectado al centro de Virtual WAN a través del vínculo Link 1. Seleccione **Editar sitio** e introduzca 10.1.1.0/24 como el espacio de direcciones privado para el sitio VPN.

     :::image type="content" source="./media/nat-rules-vpn-gateway/edit-site.png" alt-text="Captura de pantalla que muestra la página Editar sitio VPN.":::

### <a name="packet-flow"></a>Flujo de paquetes

En este ejemplo, un dispositivo local quiere acceder a una red virtual de radios. El flujo de paquetes es el siguiente, con las traducciones NAT en negrita.

1. Se inicia el tráfico desde el entorno local.
   * Dirección IP de origen: **192.168.1.1**
   * Dirección IP de destino: 30.0.0.1
1. El tráfico entra en la puerta de enlace de sitio a sitio, se traduce mediante la regla NAT y se envía a los radios.
   * Dirección IP de origen: **10.1.1.1**
   * Dirección IP de destino: 30.0.0.1
1. Se inicia la respuesta desde los radios.
   * Dirección IP de origen: 30.0.0.1
   * Dirección IP de destino: **10.1.1.1**
1. El tráfico entra en VPN Gateway de sitio a sitio, y la traducción se invierte y se envía al entorno local.
   * Dirección IP de origen: 30.0.0.1
   * Dirección IP de destino: **192.168.1.1**

### <a name="verification-checks"></a>Comprobaciones

En esta sección se muestran las comprobaciones para determinar que la configuración está configurada correctamente.

#### <a name="validate-defaultroutetable-rules-and-routes"></a>Validar DefaultRouteTable, reglas y rutas

Las ramas de Virtual WAN se asocian al recurso **DefaultRouteTable**, lo que implica que todas las conexiones de rama aprenden las rutas que se rellenan en DefaultRouteTable. Verá la regla NAT con el prefijo de asignación externa en las rutas efectivas del recurso DefaultRouteTable.

Ejemplo:

* **Prefijo:** 10.1.1.0/24  
* **Tipo de próximo salto:** VPN_S2S_Gateway
* **Próximo salto:** recurso VPN_S2S_Gateway

#### <a name="validate-address-prefixes"></a>Validar prefijos de direcciones

Este ejemplo se aplica a los recursos de redes virtuales que están asociados al recurso DefaultRouteTable.

Las **rutas efectivas** en las tarjetas de interfaz de red (NIC) de cualquier máquina virtual que se encuentre en una red virtual de radios conectada al centro de Virtual WAN también deben contener los prefijos de dirección del espacio de direcciones **ExternalMapping** de las reglas NAT.

#### <a name="validate-bgp-advertisements"></a>Validar anuncios BGP

Si tiene BGP configurado en la conexión del sitio VPN, compruebe el altavoz BGP local para asegurarse de que está anunciando una entrada para los prefijos de asignación externa.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las configuraciones de sitio a sitio, consulte [Configuración de una conexión de sitio a sitio de Virtual WAN](virtual-wan-site-to-site-portal.md).
