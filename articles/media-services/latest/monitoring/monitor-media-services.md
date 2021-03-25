---
title: Supervisión de Media Services
description: Aquí es donde empezará a aprender a supervisar Media Services
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femilia
ms.topic: how-to
ms.service: media-services
ms.date: 03/12/2021
ms.openlocfilehash: 895c674fdbe5c413b68fe12862e7846f1eb7bd0b
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103464385"
---
# <a name="monitor-media-services"></a>Supervisión de Media Services

Si tiene aplicaciones y procesos empresariales críticos que dependen de recursos de Azure, querrá supervisar esos recursos para su disponibilidad, rendimiento y funcionamiento. En este artículo se describen los datos de supervisión generados por Media Services y cómo puede usar las características de Azure Monitor para analizar y alertar sobre estos datos.

## <a name="metrics-are-useful"></a>Las métricas son útiles.

Estos son algunos ejemplos de cómo la supervisión de las métricas de Media Services puede ayudarle a comprender el rendimiento de las aplicaciones. Algunas preguntas que se pueden solucionar con Media Services métricas son:

- ¿Cómo puedo supervisar el punto de conexión de streaming estándar para saber cuándo se superan los límites?
- ¿Cómo puedo saber si tengo suficientes unidades de escalado de puntos de conexión de streaming Premium?
- ¿Cómo puedo establecer una alerta para saber cuándo escalar verticalmente los puntos de conexión de streaming?
- ¿Cómo puedo establecer una alerta para saber cuándo se alcanzó el número máximo de salidas configuradas en la cuenta?
- ¿Cómo puedo ver el desglose de las solicitudes con errores y qué está causando el error?
- ¿Cómo puedo ver cuántas solicitudes HLS o DASH se extraen del empaquetador?
- ¿Cómo se establece una alerta para saber en qué momento se ha alcanzado el umbral de solicitudes con error?

<!--THIS DOESN'T BELONG HERE Concurrency becomes a concern for the number of Streaming Endpoints used in a single account over time. You need to keep in mind the relationship between the number of concurrent streams with complex publishing parameters like dynamic packaging to multiple protocols, multiple DRM encryptions etc. Each additional published live stream adds to the CPU and output bandwidth on the Streaming Endpoint. With that in mind, you should use Azure Monitor to closely watch the Streaming Endpoint's utilization (CPU and Egress capacity) to make certain that you are scaling it appropriately (or split traffic out between multiple Streaming Endpoints if you are getting into very high concurrency).-->

<!-- Optional diagram showing monitoring for your service. If you need help creating one, contact 
robb@microsoft.com -->

## <a name="what-is-azure-monitor"></a>¿Qué es Azure Monitor?

Media Services crea datos de supervisión mediante [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview), un servicio de supervisión de pila completo de Azure que proporciona un conjunto completo de características para supervisar los recursos de Azure, además de los recursos locales y en otras nubes.

Para comenzar, lea el artículo [Supervisión de recursos de Azure con Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource), en el que se describen los conceptos siguientes:

- ¿Qué es Azure Monitor?
- Costos asociados con la supervisión
- Datos de supervisión recopilados en Azure
- Configuración de la recolección de datos
- Herramientas estándar en Azure para analizar datos de supervisión y alertar sobre ellos

## <a name="monitoring-data"></a>Supervisión de datos

Media Services recopila los mismos tipos de datos de supervisión que otros recursos de Azure, que se describen en [Supervisión de datos de recursos de Azure](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data-from-Azure-resources).

Todos los datos recopilados por Azure Monitor pueden clasificarse en uno de los dos tipos fundamentales: métricas y registros. Con estos dos tipos puede:

- Usar el Explorador de métricas para visualizar y analizar datos de métricas.
- Supervisar registros de diagnóstico de Media Services y crear alertas y notificaciones para ellos.
- Con los registros puede:
  - Enviarlos a Azure Storage
  - Transmitirlos en secuencias a Azure Event Hubs
  - Exportarlos a Log Analytics
  - Usar servicios de terceros

Consulte el artículo [Supervisión de la referencia de datos de Media Services](monitor-media-services-data-reference.md) para obtener información detallada sobre las métricas y las de métricas de registros que crea Media Services.

## <a name="collection-and-routing"></a>Recopilación y enrutamiento

Tanto las *métricas de la plataforma* como el *registro de actividad* se recopilan y almacenan de forma automática, pero se pueden enrutar a otras ubicaciones mediante una configuración de diagnóstico.  

Los *registros de recursos* **no** se recopilan ni almacenan hasta que se crea una configuración de diagnóstico y se enrutan a una o varias ubicaciones.

Consulte el artículo [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) para conocer con todo lujo de detalles el proceso de creación de una configuración de diagnóstico mediante Azure Portal, la CLI o PowerShell.

Cuando se crea una configuración de diagnóstico, se especifican las categorías de registros que se van a recopilar. Las categorías de Media Services se enumeran en [Referencia de datos de supervisión de Media Services](monitor-media-services-data-reference.md).

## <a name="analyzing-metrics"></a>Análisis de métricas

Las métricas de Media Services se pueden analizar con métricas de otros servicios de Azure mediante el Explorador de métricas. Para ello, hay que abrir **Métricas** en el menú de **Azure Monitor**. Consulte [Introducción al explorador de métricas de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) para más información sobre esta herramienta.

Para obtener una lista de las métricas que se recopilan para Media Services, consulte [Supervisión de la referencia de datos de Media Services](monitor-media-services-data-reference.md).

## <a name="analyzing-logs"></a>Análisis de datos

Los datos de los registros de Azure Monitor se almacenan en tablas, cada una con un conjunto propio de propiedades únicas.  

Todos los registros de recursos de Azure Monitor tienen los mismos campos seguidos de campos específicos del servicio. El esquema común se describe en [Esquema de registros de recursos de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-schema#top-level-resource-logs-schema).

El esquema de los registros de recursos de Media Services se encuentra en [Supervisión de la referencia de datos de Media Services](monitor-media-services-data-reference.md).

El [registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log) es un registro de plataforma de Azure que proporciona conclusiones sobre los eventos del nivel de suscripción. Puede verlo de forma independiente o enrutarlo a registros de Azure Monitor, donde puede realizar consultas mucho más complejas mediante Log Analytics.

Para obtener una lista de los tipos de registros de recursos que se recopilan para Media Services, consulte [Supervisión de la referencia de datos de Media Services](monitor-media-services-data-reference.md).

### <a name="why-would-i-want-to-use-diagnostic-logs"></a>¿Por qué es conveniente usar los registros de diagnóstico?

Estos son algunos elementos que se pueden examinar con los registros de diagnóstico son:

- El número de licencias que entrega el tipo DRM.
- El número de licencias que entrega la directiva.
- Los errores por DRM o tipo de directiva.
- El número de solicitudes de licencia sin autorización de los clientes.

## <a name="alerts"></a>Alertas

Las alertas de Azure Monitor le informan de forma proactiva cuando se detectan condiciones importantes en los datos que se supervisan. Permiten identificar y solucionar las incidencias en el sistema antes de que los clientes puedan verlos. Puede establecer alertas en [métricas](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview), [registros](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log) y el [registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-alerts).

Las métricas de Media Services se recopilan a intervalos regulares, independientemente de que el valor cambie, o no. Son útiles para las alertas porque se pueden muestrear con frecuencia. Una alerta puede activarse rápidamente con una lógica relativamente simple.

<!--
The following table lists common and recommended alert rules for Media Services.

<!-- Fill in the table with metric and log alerts that would be valuable for your service. Change the format as necessary to make it more readable
**PLACEHOLDER** table

| Alert type | Condition | Description  |
|:---|:---|:---|
| | | |
| | | |
-->

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]
