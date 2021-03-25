---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 03d83d96bb58ddaf9be4e46c4b62f10e2476e549
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101750399"
---
Hay algunos límites en el número de métricas y eventos por aplicación; es decir, por clave de instrumentación. Los límites dependen del [plan de precios](https://azure.microsoft.com/pricing/details/application-insights/) que elija.

| Resource | Límite | Nota:
| --- | --- | --- |
| Total de datos por día | 100 GB | Se pueden reducir los datos si se establece un límite. Si necesita más datos, puede aumentar el límite en el portal, hasta 1000 GB. Para capacidades mayores de 1000 GB, envíe un correo electrónico a AIDataCap@microsoft.com.
| Limitaciones | 32 000 eventos por segundo | El límite se mide por minuto.
| Retención de datos | [de 30 a 730 días](../articles/azure-monitor/app/pricing.md#change-the-data-retention-period) | Este recurso es para [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/logs/log-query-overview.md) y el [Explorador de métricas](../articles/azure-monitor/essentials/metrics-charts.md).
| Retención de resultados detallados para la [prueba de disponibilidad de varios pasos](../articles/azure-monitor/app/availability-multistep.md) | 90 días | Este recurso proporciona resultados detallados de cada paso.
| Tamaño máximo del evento | 64 000 bytes |
| Longitud de nombres de propiedades y métricas | 150 | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Longitud de cadena del valor de propiedad | 8192 | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Longitud del mensaje de seguimiento y excepción | 32 768  | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Número de [pruebas de disponibilidad](../articles/azure-monitor/app/monitor-web-app-availability.md) por aplicación | 100 |
| Retención de datos del [generador de perfiles](../articles/azure-monitor/app/profiler.md) | 5 días |
| Datos enviados por día del [generador de perfiles](../articles/azure-monitor/app/profiler.md) | 10 GB |

Para más información, consulte [Administración de precios y cuotas para Application Insights](../articles/azure-monitor/app/pricing.md).