---
title: Comparación entre las SKU de Azure Front Door Estándar/Prémium
description: En este artículo se proporciona información general sobre las SKU de Azure Front Door Estándar/Prémium y se detallan las diferencias entre sus características.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: amsriva
ms.openlocfilehash: 1753f2bb649e73d7a5fe6c1cc32361a418ea7f63
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102181812"
---
# <a name="overview-of-azure-front-door-standardpremium-sku-preview"></a>Información general de las SKU de Azure Front Door Estándar/Prémium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

Azure Front Door se ofrece para 3 SKU diferentes, [Azure Front Door](../front-door-overview.md), Azure Front Door Estándar (versión preliminar) y Azure Front Door Prémium (versión preliminar). Las SKU de Azure Front Door Estándar/Prémium combinan funcionalidades de Azure Front Door, Azure CDN Estándar de Microsoft y Azure WAF en una única plataforma CDN de nube segura con protección inteligente contra amenazas.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [**Condiciones de uso complementarias de las versiones preliminares de Microsoft Azure**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

* La **SKU de Azure Front Door Estándar** ofrece:

    * Entrega de contenido optimizada
    * Aceleración de contenido estático y dinámico
    * Equilibrio de carga global
    * Descarga de SSL
    * Administración de dominios y certificados
    * Análisis de tráfico mejorado 
    * Funcionalidades de seguridad básicas

* Además de las funcionalidades de la SKU de la versión estándar, la **SKU de Azure Front Door Prémium** ofrece también:

    * Amplias funcionalidades de seguridad en WAF
    * Protección contra bots
    * Compatibilidad con Private Link
    * Integración con el análisis de seguridad y la inteligencia sobre amenazas de Microsoft. 

![Diagrama que muestra una comparación entre las SKU de Front Door.](../media/tier-comparison/tier-comparison.png)

## <a name="feature-comparison"></a>Comparación de características

| Característica |      Estándar      |  Premium |
|----------|:-------------:|------:|
| Dominios personalizados | Sí | Sí |
| Descarga de SSL | Sí | Sí |
| Almacenamiento en memoria caché |  Sí  | Sí |
| Compresión | Sí | Sí   |
| Equilibrio de carga global | Sí  | Sí |
| Enrutamiento de capa 7 | Sí | Sí |
| Reescritura de direcciones URL | Sí | Sí |
| Motor de reglas | Sí | Sí |
| Origen privado (Private Link) | No | Sí |
| WAF | Solo reglas personalizadas | Sí |
| Protección contra bots | No | Sí |
| Métricas y diagnósticos mejorados | Sí | Sí |
| Informe de tráfico | Sí | Sí |
| Security Report | No | Sí | 

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [crear una instancia de Front Door](create-front-door-portal.md).
