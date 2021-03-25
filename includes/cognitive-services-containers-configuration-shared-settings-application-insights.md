---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: d74279c9c4d5d88e5837046e9484f5af7aad1442
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96001217"
---
La opción de configuración `ApplicationInsights` le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](/azure/application-insights) al contenedor. Application Insights proporciona una supervisión detallada del contenedor. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del contenedor. También puede identificar y diagnosticar errores en el contenedor rápidamente.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `ApplicationInsights`.

|Obligatorio| Nombre | Tipo de datos | Descripción |
|--|------|-----------|-------------|
|No| `InstrumentationKey` | String | Clave de instrumentación de la instancia de Application Insights para la que se envían los datos de telemetría del contenedor. Para más información, consulte [Application Insights para ASP.NET Core](../articles/azure-monitor/app/asp-net-core.md). <br><br>Ejemplo:<br>`InstrumentationKey=123456789`|