---
title: Métricas estándar de Azure Application Insights | Microsoft Docs
description: En este artículo se enumeran las métricas de Azure Application Insights con las agregaciones y dimensiones compatibles.
services: azure-monitor
ms.topic: reference
ms.date: 07/03/2019
ms.subservice: application-insights
ms.openlocfilehash: 0a18088fa434efa76007607c067feec107bdae57
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100572358"
---
# <a name="application-insights-standard-metrics"></a>Métricas estándar de Application Insights

Las métricas estándar se agregan previamente durante la recopilación, tienen un mejor rendimiento en el momento de la consulta. Esto las convierte en la mejor opción para los paneles y las alertas en tiempo real.

## <a name="availability-metrics"></a>Métricas de disponibilidad

Las métricas de la categoría Disponibilidad le permiten ver el estado de la aplicación web, tal y como se observa en los puntos de todo el mundo. [Configure las pruebas de disponibilidad](../app/monitor-web-app-availability.md) para empezar a usar las métricas de esta categoría.

### <a name="availability-availabilityresultsavailabilitypercentage"></a>Disponibilidad (availabilityResults/availabilityPercentage)
La métrica *Disponibilidad* muestra el porcentaje de las series de pruebas web que no detectaron ningún problema. El valor mínimo posible es 0, que indica que se han producido errores en todas las series de pruebas web. El valor de 100 significa que todas las series de pruebas web superaron los criterios de validación.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|---|---|---|
|Porcentaje|Average|`Run location`, `Test name`|


### <a name="availability-test-duration-availabilityresultsduration"></a>Duración de la prueba de disponibilidad (availabilityResults/duration)

La métrica *Duración de la prueba de disponibilidad* muestra cuánto tiempo tardó en ejecutarse la prueba web. En el caso de las [pruebas web de varios pasos](../app/availability-multistep.md), la métrica refleja el tiempo total de ejecución de todos los pasos.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|---|---|---|
|Milisegundos|Promedio, mín., máx.|`Run location`, `Test name`, `Test result`

### <a name="availability-tests-availabilityresultscount"></a>Pruebas de disponibilidad (availabilityResults/count)

La métrica de las *Pruebas de disponibilidad* refleja el recuento de las series de pruebas web de Azure Monitor.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|---|---|---|
|Count|Count|`Run location`, `Test name`, `Test result`|


## <a name="browser-metrics"></a>Métricas del explorador

El SDK de JavaScript de Application Insights recopila las métricas de explorador de los exploradores de los usuarios finales reales. Proporcionan una gran información sobre la experiencia de los usuarios con la aplicación web. Normalmente, no se muestrean las métricas del explorador, lo que significa que proporcionan una mayor precisión de los números de uso en comparación con las métricas del lado servidor, que podrían sesgarse con el muestreo.

> [!NOTE]
> Para recopilar las métricas del explorador, la aplicación debe estar instrumentada con el [SDK de JavaScript para Application Insights](../app/javascript.md).

### <a name="browser-page-load-time-browsertimingstotalduration"></a>Tiempo de carga de página del explorador (browserTimings/totalDuration)

|Unidad de medida|Agregaciones compatibles| Dimensiones compatibles|
|---|---|---|
|Milisegundos|Promedio, mín., máx.|None|

### <a name="client-processing-time-browsertimingprocessingduration"></a>Tiempo de procesamiento del cliente (browserTiming/processingDuration)

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
|Milisegundos|Promedio, mín., máx.|None|

### <a name="page-load-network-connect-time-browsertimingsnetworkduration"></a>Tiempo de conexión de red de carga de página (browserTimings/networkDuration)

|Unidad de medida|Agregaciones compatibles| Dimensiones compatibles|
|---|---|---|
|Milisegundos|Promedio, mín., máx.|None|

### <a name="receiving-response-time-browsertimingsreceiveduration"></a>Tiempo de respuesta de recepción (browserTimings/receiveDuration)

|Unidad de medida|Agregaciones compatibles| Dimensiones compatibles|
|---|---|---|
|Milisegundos|Promedio, mín., máx.|None|

### <a name="send-request-time-browsertimingssendduration"></a>Hora de envío de la solicitud (browserTimings/sendDuration)

|Unidad de medida|Agregaciones compatibles| Dimensiones compatibles|
|---|---|---|
|Milisegundos|Promedio, mín., máx.|None|

## <a name="failure-metrics"></a>Métricas de errores

Las métricas de **Errores** muestran problemas con el procesamiento de solicitudes, las llamadas de dependencia y las excepciones producidas.

### <a name="browser-exceptions-exceptionsbrowser"></a>Excepciones de explorador (exceptions/browser)

Esta métrica refleja el número de excepciones producidas a partir del código de aplicación que se ejecuta en el explorador. Solo se incluyen en la métrica las excepciones de las que se realiza un seguimiento con una llamada API de Application Insights ```trackException()```.

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles|
|---|---|---|---|
| Count | Count | `Cloud role name` |

### <a name="dependency-call-failures-dependenciesfailed"></a>Errores de llamada de dependencia (dependencies/failed)

Cantidad de llamadas de dependencia con errores.

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
|Count|Count| `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Rarget of dependency call`.


### <a name="exceptions-exceptionscount"></a>Excepciones (exceptions/count)

Cada vez que registra una excepción en Application Insights, se produce una llamada al [método trackException()](../app/api-custom-events-metrics.md#trackexception) del SDK. La métrica de excepciones muestra la cantidad de excepciones registradas.

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
|Count|Count|`Cloud role instance`, `Cloud role name`, `Device type`|

### <a name="failed-requests-requestsfailed"></a>Solicitudes con error (requests/failed)

Recuento de solicitudes de servidor de las que se ha realizado un seguimiento y que se marcaron como *errores*. De forma predeterminada, el SDK de Application Insights marca automáticamente cada solicitud de servidor que devolvió el código de respuesta HTTP 5xx o 4xx como una solicitud con error. Para personalizar esta lógica, puede modificar la propiedad *success* del elemento de telemetría de la solicitud en un [inicializador de telemetría personalizado](../app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer).


|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
|Count|Count|`Cloud role instance`, `Cloud role name`, `Is synthetic traffic`, `Request performance`, `Result code`|


### <a name="server-exceptions-exceptionsserver"></a>Excepciones del servidor (exceptions/server)

Esta métrica muestra el número de excepciones de servidor.

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
|Count|Count|`Cloud role instance`, `Cloud role name`|

## <a name="performance-counters"></a>Contadores de rendimiento

Use las métricas de la categoría de **Contadores de rendimiento** para acceder a los [contadores de rendimiento del sistema que recopila Application Insights](../app/performance-counters.md).

### <a name="available-memory-performancecountersavailablememory"></a>Memoria disponible (performanceCounters/availableMemory)

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
|Dependiente de datos: Megabytes, gigabytes|Promedio, máx., mín.|`Cloud role instance`|


### <a name="exception-rate-performancecountersexceptionrate"></a>Tasa de excepciones (performanceCounters/exceptionRate)

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
| Count | Promedio, máx., mín. | `Cloud role instance` |


### <a name="http-request-execution-time-performancecountersrequestexecutiontime"></a>Tiempo de ejecución de la solicitud HTTP (performanceCounters/requestExecutionTime)

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
| Milisegundos | Promedio, máx., mín. | `Cloud role instance` |

### <a name="http-request-rate-performancecountersrequestspersecond"></a>Frecuencia de solicitudes HTTP (performanceCounters/requestsPerSecond)

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
| Solicitudes por segundo | Promedio, máx., mín. | `Cloud role instance` |

### <a name="http-requests-in-application-queue-performancecountersrequestsinqueue"></a>Solicitudes HTTP en la cola de la aplicación (performanceCounters/requestsInQueue)

|Unidad de medida|Agregaciones compatibles | Dimensiones compatibles |
|---|---|---|---|
| Count | Promedio, máx., mín. | `Cloud role instance` |


### <a name="process-cpu-performancecountersprocesscpupercentage"></a>CPU de proceso (performanceCounters/processCpuPercentage)

La métrica muestra la cantidad de la capacidad total del procesador que usa el proceso que hospeda la aplicación supervisada.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
|Porcentaje|Promedio, máx., mín.| `Cloud role instance` |


### <a name="process-io-rate-performancecountersprocessiobytespersecond"></a>Tasa de E/S del proceso (performanceCounters/processIOBytesPerSecond)

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
|Bytes por segundo|Promedio, mín., máx.|`Cloud role instance` |



### <a name="process-private-bytes-performancecountersprocessprivatebytes"></a>Bytes privados del proceso (performanceCounters/processPrivateBytes)

Cantidad de memoria no compartida que el proceso supervisado asignó para sus datos.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
|Bytes|Promedio, mín., máx.|`Cloud role instance` |

### <a name="processor-time-performancecountersprocessorcpupercentage"></a>Tiempo de procesador (performanceCounters/processorCpuPercentage)

Consumo de CPU de *todos* los procesos que se ejecutan en la instancia del servidor supervisado.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
|Porcentaje|Promedio, mín., máx.|`Cloud role instance` |

>[!NOTE]
> La métrica de tiempo del procesador no está disponible para las aplicaciones hospedadas en Azure App Services. Use la métrica de la [CPU de procesos](#process-cpu-performancecountersprocesscpupercentage) para realizar un seguimiento del uso de la CPU de las aplicaciones web hospedadas en App Services.

## <a name="server-metrics"></a>Métricas del servidor

### <a name="dependency-calls-dependenciescount"></a>Llamadas de dependencia (dependencies/count)

Esta métrica está relacionada con el número de llamadas de dependencia.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Count | Count | `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Successful call`, `Target of a dependency call` |

### <a name="dependency-duration-dependenciesduration"></a>Duración de la dependencia (dependencies/duration)

Esta métrica hace referencia a la duración de las llamadas de dependencia.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Hora | Promedio, mín., máx. | `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Successful call`, `Target of a dependency call` |


### <a name="server-request-rate-requestscount"></a>Tasa de solicitudes del servidor (solicitudes/recuento)

Esta métrica refleja el número de solicitudes de servidor entrantes que recibió la aplicación web.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Count | Average | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

### <a name="server-requests-requestscount"></a>Solicitudes de servidor (requests/count)

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Count | Count | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

### <a name="server-response-time-requestsduration"></a>Tiempo de respuesta del servidor (requests/duration)

Esta métrica refleja el tiempo que tardaron los servidores en procesar las solicitudes entrantes.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Hora | Promedio, mín., máx. | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

## <a name="usage-metrics"></a>Métricas de uso

### <a name="page-view-load-time-pageviewsduration"></a>Tiempo de carga de la vista de página (pageViews/duration)

Esta métrica hace referencia a la cantidad de tiempo que tardaron en cargarse los eventos de PageView.

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Hora | Promedio, mín., máx. | `Cloud role name`, `Is traffic synthetic` |

### <a name="page-views-pageviewscount"></a>Vistas de página (pageViews/count)

Recuento de eventos de PageView registrados con la API de Application Insights TrackPageView().

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Count | Count | `Cloud role name`, `Is traffic synthetic` |

### <a name="traces-tracescount"></a>Seguimientos (traces/count)

Recuento de instrucciones de seguimiento registradas con la llamada API de Application Insights TrackTrace().

|Unidad de medida|Agregaciones compatibles|Dimensiones compatibles|
|---|---|---|
| Count | Count | `Cloud role instance`, `Cloud role name`,  `Is traffic synthetic`, `Severity level` |


## <a name="next-steps"></a>Pasos siguientes

* Aprenda sobre las [métricas basadas en registros y agregadas previamente](./pre-aggregated-metrics-log-metrics.md).
* [Definiciones y consultas de métricas basadas en registros](../essentials/app-insights-metrics.md).