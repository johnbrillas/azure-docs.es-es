---
title: Planes de servicio y cuotas para Azure Spring Cloud
description: Más información sobre las cuotas y los planes de servicio de Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: b02ccb3acb4546e08e7d58159ab9d85bca2d0eed
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877701"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Cuotas y planes de servicio de Azure Spring Cloud

**Este artículo se aplica a:** ✔️ Java ✔️ C#

Todos los servicios de Azure establecen límites y cuotas predeterminados para recursos y características.   Azure Spring Cloud ofrece dos planes de tarifa: Básico y Estándar. En este artículo se detallan los límites de ambos niveles.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Límites y niveles de servicio de Azure Spring Cloud

| Recurso | Ámbito | Básico | Estándar
------- | ------- | -------
vCPU | por instancia de aplicación | 1 | 4
Memoria | por instancia de aplicación | 2 GB | 8 GB
Instancias del servicio Azure Spring Cloud | por región y suscripción | 10 | 10
Instancias de aplicación totales | por instancia del servicio Azure Spring Cloud | 25 | 500
Dominios personalizados | por instancia del servicio Azure Spring Cloud | 0 | 25 
Volúmenes persistentes | por instancia del servicio Azure Spring Cloud | 1 GB/aplicación x 10 aplicaciones | 50 GB/aplicación x 10 aplicaciones

> [!TIP]
> Los límites indicados para las instancias de aplicación totales por instancia de servicio se aplican a las aplicaciones y las implementaciones en cualquier estado, incluido el estado Detenido. Elimine las aplicaciones o implementaciones que no estén en uso.

## <a name="next-steps"></a>Pasos siguientes

Algunos límites predeterminados se pueden aumentar. Si su configuración requiere un aumento, [cree una solicitud de soporte técnico](../azure-portal/supportability/how-to-create-azure-support-request.md).
