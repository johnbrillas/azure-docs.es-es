---
title: Azure Traffic Manager | Microsoft Docs
description: Este artículo ofrece una introducción sobre Azure Traffic Manager. Averigüe si es la elección correcta para equilibrar la carga de tráfico del usuario en la aplicación.
services: traffic-manager
author: duongau
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2021
ms.author: duau
ms.openlocfilehash: 09b82eed5ad6a9ad121ca56d197eb9c003d027f5
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98624916"
---
# <a name="what-is-traffic-manager"></a>¿Qué es el Administrador de tráfico?
Azure Traffic Manager es un equilibrador de carga de tráfico basado en DNS. Este servicio permite distribuir el tráfico a las aplicaciones orientadas al público en las regiones globales de Azure. Traffic Manager también proporciona puntos de conexión públicos con alta disponibilidad y rápida capacidad de respuesta.

Traffic Manager usa DNS para dirigir las solicitudes del cliente al punto de conexión de servicio adecuado en función de un método de enrutamiento del tráfico. Traffic Manager también proporciona supervisión de estado de todos los puntos de conexión. El punto de conexión de puede ser cualquier servicio accesible desde Internet hospedado dentro o fuera de Azure. Traffic Manager proporciona una serie de [métodos de enrutamiento del tráfico](traffic-manager-routing-methods.md) y [opciones de supervisión del punto de conexión](traffic-manager-monitoring.md) para satisfacer las distintas necesidades de las aplicaciones y los modelos de conmutación automática por error. Traffic Manager es resistente a errores, incluidos los que afecten a toda una región de Azure.

>[!NOTE]
> Azure ofrece un conjunto de soluciones de equilibrio de carga completamente administradas para sus escenarios. Si desea finalización con el protocolo de seguridad de la capa de transporte (TLS) ("descarga SSL") procesamiento de niveles de aplicación por solicitud HTTP/HTTPS, revise [Application Gateway](../application-gateway/overview.md). Si desea conseguir equilibrio de carga en la región, consulte [Load Balancer](../load-balancer/load-balancer-overview.md). Sus escenarios integrales pueden beneficiarse de la combinación de estas soluciones según sea necesario.
>
> Si desea ver una comparación de las distintas opciones de equilibrio de carga de Azure, consulte [Información general sobre las opciones de equilibrio de carga en Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).

Traffic Manager ofrece las siguientes características:

## <a name="increase-application-availability"></a>Aumento de la disponibilidad de la aplicación

Traffic Manager ofrece alta disponibilidad para las aplicaciones más importantes al supervisar los puntos de conexión y proporcionar la conmutación automática por error cuando un punto de conexión deja de funcionar.
    
## <a name="improve-application-performance"></a>Mejora del rendimiento de las aplicaciones

Azure permite ejecutar servicios y sitios web en la nube en los centros de datos ubicados en todo el mundo. Traffic Manager puede mejorar la capacidad de respuesta del sitio web, ya que dirige el tráfico al punto de conexión con la menor latencia posible.

## <a name="service-maintenance-without-downtime"></a>Mantenimiento del servicio sin inactividad

Se puede realizar un mantenimiento planeado de las aplicaciones sin que se produzca inactividad. Traffic Manager puede dirigir el tráfico a puntos de conexión alternativos mientras el mantenimiento está en curso.

## <a name="combine-hybrid-applications"></a>Combinación de aplicaciones híbridas

Traffic Manager admite puntos de conexión externos (que no forman parte de Azure) que pueden emplearse en implementaciones locales y de nube híbrida, entre ellos los escenarios de [implementación](https://azure.microsoft.com/overview/what-is-cloud-bursting/), migración y conmutación por error a la nube.

## <a name="distribute-traffic-for-complex-deployments"></a>Distribución del tráfico para implementaciones complejas

Mediante los [perfiles anidados de Traffic Manager](traffic-manager-nested-profiles.md), se pueden combinar varios métodos de enrutamiento del tráfico para crear reglas complejas y flexibles que satisfagan las necesidades de escalado de las implementaciones más grandes y complejas.

## <a name="pricing"></a>Precios

Para obtener información de precios, consulte la página de [precios de Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear un perfil de Traffic Manager](./quickstart-create-traffic-manager-profile.md).
- Consulte [Cómo funciona Traffic Manager](traffic-manager-how-it-works.md).
- Vea las [preguntas más frecuentes](traffic-manager-FAQs.md) sobre Traffic Manager.