---
title: ¿Qué es Azure Route Server (versión preliminar)?
description: Obtenga información sobre cómo puede simplificar Azure Route Server el enrutamiento entre la aplicación virtual de red (NVA) y la red virtual.
services: route-server
author: duongau
ms.service: route-server
ms.topic: overview
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 98527ea5227fcdc2c35dbe3579657368e30fcdaa
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680585"
---
# <a name="what-is-azure-route-server-preview"></a>¿Qué es Azure Route Server (versión preliminar)? 

Azure Route Server simplifica el enrutamiento dinámico entre la aplicación virtual de red (NVA) y la red virtual. Permite intercambiar información de enrutamiento directamente mediante el protocolo de enrutamiento BGP (Protocolo de puerta de enlace de borde) entre cualquier NVA que admita el protocolo de enrutamiento BGP y la red definida por software (SDN) de Azure en la red virtual de Azure (VNET) sin necesidad de configurar ni mantener manualmente las tablas de rutas. Azure Route Server es un servicio totalmente administrado y está configurado con alta disponibilidad.

> [!IMPORTANT]
> Azure Route Server (versión preliminar) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-does-it-work"></a>¿Cómo funciona?

En el siguiente diagrama se ilustra cómo funciona Azure Route Server con una NVA SDWAN y una NVA de seguridad en una red virtual. Una vez que haya establecido el emparejamiento de BGP, Azure Route Server recibirá una ruta local (10.250.0.0/16) del dispositivo SDWAN y una ruta predeterminada (0.0.0.0/0) del firewall. A continuación, estas rutas se configuran automáticamente en las máquinas virtuales de la red virtual. Como resultado, todo el tráfico destinado a la red local se enviará al dispositivo SDWAN. Por otro lado, todo el tráfico relacionado con Internet se enviará al firewall. En la dirección opuesta, Azure Route Server enviará la dirección de la red virtual (10.1.0.0/16) a ambas NVA. El dispositivo SDWAN puede propagarlo a la red local.

![Diagrama que muestra Azure Route Server configurado en una red virtual.](./media/overview/route-server-overview.png)

## <a name="key-benefits"></a>Ventajas principales 

Azure Route Server simplifica la configuración, la administración y la implementación de la NVA en la red virtual.  

* Ya no es necesario actualizar manualmente la tabla de enrutamiento en la NVA cada vez que se actualicen las direcciones de la red virtual. 

* Ya no es necesario actualizar manualmente las [rutas definidas por el usuario](../virtual-network/virtual-networks-udr-overview.md) cada vez que la NVA anuncie nuevas rutas o retire las anteriores. 

* Ya no es necesario configurar un equilibrador de carga delante de la NVA con fines de resistencia o rendimiento. Al emparejar varias instancias de la NVA con Azure Route Server, puede configurar los atributos de BGP en la NVA. Estos atributos de BGP permitirán saber a Azure Route Server qué instancia de la NVA debe estar activa o pasiva. 

* La interfaz entre la NVA y Azure Route Server se basa en un protocolo estándar común. Siempre que la NVA admita BGP, puede emparejarla con Azure Route Server. Para más información, consulte [Protocolos de enrutamiento admitidos por Route Server](route-server-faq.md#protocol).

* Puede implementar Azure Route Server en cualquiera de las redes virtuales nuevas o existentes. 

## <a name="faq"></a>Preguntas más frecuentes

Para ver las preguntas más frecuentes sobre Azure Route Server, consulte [Preguntas frecuentes sobre Azure Route Server](route-server-faq.md).

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Configuración de una instancia de Azure Route Server](quickstart-configure-route-server-portal.md).
- [Compatibilidad de Azure Route Server (versión preliminar) con ExpressRoute y VPN de Azure](expressroute-vpn-support.md)
