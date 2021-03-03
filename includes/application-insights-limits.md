---
title: archivo de inclusión
description: archivo de inclusión
services: application-insights
author: lgayhardt
ms.service: application-insights
ms.topic: include
ms.date: 08/06/2019
ms.author: lagayhar
ms.custom: include file
ms.openlocfilehash: 1c4f6b876a4aa80c7e51f2bb3ca88234203d0daa
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726190"
---
Hay algunos límites en el número de métricas y eventos por aplicación; es decir, por clave de instrumentación. Los límites dependen del [plan de precios](https://azure.microsoft.com/pricing/details/application-insights/) que elija.

| Resource | Límite predeterminado | Nota:
| --- | --- | --- |
| Total de datos por día | 100 GB | Se pueden reducir los datos si se establece un límite. Si necesita más datos, puede aumentar el límite en el portal, hasta 1000 GB. Para capacidades mayores de 1000 GB, envíe un correo electrónico a AIDataCap@microsoft.com.
| Limitaciones | 32 000 eventos por segundo | El límite se mide por minuto.
| Registros de retención de datos | [Entre 30 y 730 días](../articles/azure-monitor/app/pricing.md#change-the-data-retention-period)  | Este recurso es para [Registros](../articles/azure-monitor/logs/log-query-overview.md).
| Métricas de retención de datos | 90 días| Este recurso es para el [Explorador de métricas](../articles/azure-monitor/essentials/metrics-charts.md).
| Retención de resultados detallados para la [prueba de disponibilidad de varios pasos](../articles/azure-monitor/app/availability-multistep.md) | 90 días | Este recurso proporciona resultados detallados de cada paso.
| Tamaño máximo de elementos de telemetría | 64 KB |
| Número máximo de elementos de telemetría por lote | 64 K |
| Longitud de nombres de propiedades y métricas | 150 | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Longitud de cadena del valor de propiedad | 8192  | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Longitud del mensaje de seguimiento y excepción | 32 768  | Consulte [esquemas de tipos](https://github.com/MohanGsk/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Número de [pruebas de disponibilidad](../articles/azure-monitor/app/monitor-web-app-availability.md) por aplicación | 100 |
| Retención de datos del [generador de perfiles](../articles/azure-monitor/app/profiler.md) | 5 días |
| Datos enviados por día del [generador de perfiles](../articles/azure-monitor/app/profiler.md) | 10 GB |

Para más información, consulte [Administración de precios y cuotas para Application Insights](../articles/azure-monitor/app/pricing.md).