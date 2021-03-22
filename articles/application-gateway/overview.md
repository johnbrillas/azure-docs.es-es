---
title: ¿Qué es Azure Application Gateway?
description: Obtenga información acerca de cómo puede utilizar una puerta de enlace de aplicaciones de Azure para administrar el tráfico web a la aplicación.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: 4344cd38d9a58eec27c6202e81b8ef678a510681
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102176015"
---
# <a name="what-is-azure-application-gateway"></a>¿Qué es Azure Application Gateway?

Azure Application Gateway es un equilibrador de carga de tráfico web que permite administrar el tráfico a las aplicaciones web. Los equilibradores de carga tradicionales operan en la capa de transporte (OSI capa 4: TCP y UDP) y enrutan el tráfico en función de la dirección IP y puerto de origen a una dirección IP y puerto de destino.

Application Gateway puede tomar decisiones de enrutamiento basadas en atributos adicionales de una solicitud HTTP, por ejemplo los encabezados de host o la ruta de acceso del URI. Por ejemplo, puede enrutar el tráfico en función de la dirección URL entrante. Por lo que si `/images` se encuentra en la dirección URL entrante, puede redirigir el tráfico a un conjunto específico de servidores (conocido como un grupo) configurados para imágenes. Si `/video` se encuentra en la dirección URL, el tráfico se enruta a otro grupo optimizado para vídeos.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Este tipo de enrutamiento se conoce como equilibrio de carga de capa de aplicación (OSI capa 7). Azure Application Gateway puede hacer enrutamiento basado en dirección URL y mucho más.

>[!NOTE]
> Azure ofrece un conjunto de soluciones de equilibrio de carga completamente administradas para sus escenarios. 
> * Si busca un enrutamiento global basado en DNS y **no** tiene requisitos de finalización con el protocolo de seguridad de la capa de transporte (TLS) ("descarga SSL"), de procesamiento de niveles de aplicación o por solicitud HTTP/HTTPS, revise [Traffic Manager](../traffic-manager/traffic-manager-overview.md). 
> * Si necesita optimizar el enrutamiento global del tráfico web y optimizar el rendimiento y la confiabilidad de los usuarios finales de nivel superior mediante una conmutación por error global rápida, consulte [Front Door](../frontdoor/front-door-overview.md).
> * Para realizar el equilibrio de carga de capa de red, revise [Load Balancer](../load-balancer/load-balancer-overview.md). 
> 
> Sus escenarios pueden beneficiarse de extremo a extremo de la combinación de estas soluciones según sea necesario.
> Si desea ver una comparación de las distintas opciones de equilibrio de carga de Azure, consulte [Información general sobre las opciones de equilibrio de carga en Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).


## <a name="features"></a>Características

Para obtener información sobre las características de Application Gateway, consulte [Características de Azure Application Gateway](features.md).

## <a name="pricing-and-sla"></a>Precios y contrato de nivel de servicio

Para obtener información sobre los precios, de Application Gateway consulte [Precios de Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/).

Para obtener información sobre el Acuerdo de Nivel de Servicio de Application Gateway, consulte [SLA para Application Gateway](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/).

## <a name="whats-new"></a>Novedades

Para obtener información sobre las novedades de Azure Application Gateway, consulte [Actualizaciones de Azure](https://azure.microsoft.com/updates/?category=networking&query=Application%20Gateway).

## <a name="next-steps"></a>Pasos siguientes

Según los requisitos y el entorno, puede crear una instancia de Application Gateway de prueba mediante Azure Portal, Azure PowerShell o la CLI de Azure.

- [Inicio rápido: Dirección del tráfico web con Azure Application Gateway: Azure Portal](quick-create-portal.md)
- [Guía de inicio rápido: dirigir el tráfico web con Azure Application Gateway: Azure PowerShell](quick-create-powershell.md)
- [Inicio rápido: Dirección del tráfico web con Azure Application Gateway: CLI de Azure](quick-create-cli.md)