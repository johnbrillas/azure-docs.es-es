---
title: Novedades en la documentación de Azure Monitor
description: Se realizan actualizaciones significativas de la documentación de Azure Monitor todos los meses.
ms.subservice: ''
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 02/10/2021
ms.openlocfilehash: b21e5ea8e25844e3e025915b0b9f15162c642f25
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713446"
---
# <a name="whats-new-in-azure-monitor-documentation"></a>Novedades en la documentación de Azure Monitor

En este artículo se enumeran los artículos de Azure Monitor que son nuevos o que se han actualizado de forma significativa. Se actualizará la primera semana de cada mes para que incluya las actualizaciones de artículos del mes anterior.

## <a name="january-2021"></a>Enero de 2021 

### <a name="general"></a>General 
- [Preguntas frecuentes sobre Azure Monitor](faq.md): se ha agregado una entrada sobre la información del dispositivo para Application Insights.
### <a name="agents"></a>Agentes  
- [Recopilación de eventos de Seguimiento de eventos para Windows (ETW) para el análisis de registros de Azure Monitor](./agents/data-sources-event-tracing-windows.md): artículo nuevo.
- [Reglas de recopilación de datos en Azure Monitor (versión preliminar)](./agents/data-collection-rule-overview.md): se han agregado vínculos a ejemplos de PowerShell y la CLI.

### <a name="alerts"></a>Alertas  
- [Configuración de Azure para conectar herramientas de ITSM mediante Exportación segura](./alerts/itsm-connector-secure-webhook-connections-azure-configuration.md): artículo nuevo.
- [Errores de estado del conector en el panel de ITSMC](./alerts/itsmc-dashboard-errors.md): artículo nuevo.
- [Investigación de errores mediante el panel de ITSMC](./alerts/itsmc-dashboard.md): artículo nuevo.
- [Solución de problemas de alertas de métricas de Azure](./alerts/alerts-troubleshoot-metric.md): se ha agregado secciones para los umbrales dinámicos.
- [Solución de problemas en Conector de Administración de servicios de TI](./alerts/itsmc-troubleshoot-overview.md): artículo nuevo.

### <a name="application-insights"></a>Application Insights
- [Correlación de los datos de telemetría de Azure Application Insights](app/correlation.md): se ha agregado una correlación de seguimiento cuando un módulo llama a otro en OpenCensus Python.
- [Application Insights para páginas web](app/javascript.md): artículo nuevo.
- [Complemento Click Analytics Auto-collection para el SDK de JavaScript para Application Insights](app/javascript-click-analytics-plugin.md): artículo nuevo.
- [Supervisión de las aplicaciones sin cambios de código: instrumentación automática para Azure Monitor Application Insights](app/codeless-overview.md): columna de Python agregada.
- [Complemento Native React para el SDK de JavaScript de Application Insights](app/javascript-react-plugin.md): artículo nuevo.
- [Procesadores de telemetría (versión preliminar): Azure Monitor Application Insights para Java](app/java-standalone-telemetry-processors.md): reescrito.
- [Análisis de uso con Azure Application Insights](app/usage-overview.md): artículo nuevo.
- [Uso de Application Change Analysis en Azure Monitor para detectar problemas de la aplicación web](app/change-analysis.md): se han agregado mensajes de error.


### <a name="insights"></a>Información detallada    
- [Azure Monitor para Azure Data Explorer (versión preliminar)](insights/data-explorer.md): artículo nuevo.

### <a name="logs"></a>Registros    
- [Clave administrada por el cliente de Azure Monitor](./logs/customer-managed-keys.md): se introduce la identidad administrada asignada por el usuario.
- [Clústeres dedicados de registros de Azure Monitor](./logs/logs-dedicated-clusters.md): código de respuesta actualizado.
- [Consulta entre servicios (Azure Monitor y Azure Data Explorer [versión preliminar])](/azure/azure-monitor/platform/azure-data-explorer-monitor-cross-service-query): nuevo artículo.

### <a name="metrics"></a>Métricas
- [Explicación de la visualización y agregación de métricas de Azure Monitor](./essentials/metrics-aggregation-explained.md): artículo nuevo.

### <a name="platform-logs"></a>Registros de plataforma
- [Categorías y servicios admitidos de los registros de recursos de Azure Monitor](./essentials/resource-logs-categories.md): artículo nuevo.

### <a name="visualizations"></a>Visualizaciones
- [Orígenes de datos de los libros de Azure Monitor](./visualize/workbooks-data-sources.md): se ha agregado la combinación y el análisis de cambios.


## <a name="december-2020"></a>Diciembre de 2020

### <a name="general"></a>General
- [Clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): los mensajes de error agregados.
- [Asociados que se integran con Azure Monitor](partners.md): sección agregada a la integración de Event Hubs.

### <a name="agents"></a>Agentes
- [Consulta entre recursos en Azure Data Explorer mediante Azure Monitor](logs/azure-monitor-data-explorer-proxy.md): nuevo artículo.
- [Introducción a los agentes de supervisión de Azure](agents/agents-overview.md): se ha agregado compatibilidad con Oracle 8.

### <a name="alerts"></a>Alertas
- [Solución de problemas de alertas de métricas de Azure](alerts/alerts-troubleshoot-metric.md): se ha agregado información de solución de problemas para umbrales dinámicos.
- [Conector de Administración de servicios de TI en Log Analytics](alerts/itsmc-definition.md): nuevo artículo.
- [Introducción al Conector de Administración de servicios de TI](alerts/itsmc-overview.md): información de solución de problemas reestructurada.
- [Conexión de Cherwell con el Conector de Administración de servicios de TI](alerts/itsmc-connections-cherwell.md): nuevo artículo.
- [Conexión de Provance con el Conector de Administración de servicios de TI](alerts/itsmc-connections-provance.md): nuevo artículo.
- [Conexión de SCSM con el Conector de Administración de servicios de TI](alerts/itsmc-connections-scsm.md): nuevo artículo.
- [Conexión de ServiceNow con el Conector de Administración de servicios de TI](alerts/itsmc-connections-servicenow.md): nuevo artículo.
- [Corrección manual de los problemas de sincronización de ServiceNow](alerts/itsmc-resync-servicenow.md): información de solución de problemas reestructurada.




### <a name="application-insights"></a>Application Insights
- [Azure Application Insights para aplicaciones web de JavaScript](app/javascript.md): se ha agregado la configuración de la cadena de conexión.
- [Métricas estándar de Azure Application Insights](app/standard-metrics.md): nuevo artículo.
- [Azure Monitor Application Insights en Java](app/java-in-process-agent.md): información adicional sobre el envío de telemetría personalizada desde la aplicación.
- [Exportación continua de telemetría desde Application Insights](app/export-telemetry.md): se ha agregado la exportación basada en la configuración de diagnóstico.
- [Habilitación de Snapshot Debugger para aplicaciones .NET y .NET Core en Azure Functions](app/snapshot-debugger-function-app.md): nuevo artículo.
- [Direcciones IP usadas por Application Insights y Log Analytics](app/ip-addresses.md): se han agregado direcciones IP para Azure Government.
- [Solución de problemas relacionados con Azure Application Insights Profiler](app/profiler-troubleshooting.md): se ha agregado información sobre la página de estado de la extensión de sitio de los servicios de diagnóstico.
- [Solución de problemas de las pruebas de disponibilidad de Azure Application Insights](app/troubleshoot-availability.md): actualizaciones para solucionar problemas de pruebas de ping.
- [Solución de problemas de Application Insights de Azure Monitor para Java](app/java-standalone-troubleshoot.md): nuevo artículo.

### <a name="containers"></a>Contenedores
- [Informes de Container Insights](insights/container-insights-reports.md): nuevo artículo.

### <a name="logs"></a>Registros
- [Clústeres dedicados de registros de Azure Monitor](logs/logs-dedicated-clusters.md): se han agregado comandos automatizados, métodos para desvincular y quitar e información de solución de problemas.
- [Consulta entre servicios entre Azure Monitor y Azure Data Explorer (versión preliminar)](logs/azure-data-explorer-monitor-cross-service-query.md): nuevo artículo.
- [Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)](logs/logs-data-export.md): se han agregado plantillas de Resource Manager.

### <a name="metrics"></a>Métricas
- [Características avanzadas del Explorador de métricas de Azure](essentials/metrics-charts.md): se ha agregado información sobre el selector de ámbitos de recursos.
- [Visualización de varios recursos en el Explorador de métricas](essentials/metrics-dynamic-scope.md): nuevo artículo.

### <a name="networks"></a>Redes
- [Solución Azure Networking Analytics en Azure Monitor](insights/azure-networking-analytics.md): se ha agregado información sobre el libro de información de red.

### <a name="virtual-machines"></a>Virtual Machines
- [Habilitación de Azure Monitor para un entorno híbrido](vm/vminsights-enable-hybrid.md): nueva versión del agente de dependencia.


### <a name="visualizations"></a>Visualizaciones
- [Visualizaciones de asignaciones de libros de Azure Monitor](visualize/workbooks-map-visualizations.md): artículo nuevo.
- [Los libros de Azure Monitor incorporan su propio almacenamiento](visualize/workbooks-bring-your-own-storage.md): nuevo artículo.


## <a name="november-2020"></a>Noviembre de 2020

### <a name="general"></a>General
- [Límites de servicio de Azure Monitor](service-limits.md):-se ha actualizado para la compatibilidad con Azure Arc.

### <a name="agents"></a>Agentes
- [Información general sobre los agentes de Azure Monitor](agents/agents-overview.md): se ha-actualizado para la compatibilidad con Azure Arc.
- [Instalación del agente de Azure Monitor](agents/azure-monitor-agent-install.md): nuevo artículo.
- [Información general del agente de Azure Monitor](agents/azure-monitor-agent-overview.md): se ha actualizado para la compatibilidad con Azure Arc.
- [Ejemplos de plantillas de Azure Resource Manager para agentes](agents/resource-manager-agent.md): se ha actualizado para la compatibilidad con Azure Arc.

### <a name="alerts"></a>Alertas
- [Creación y administración de grupos de acciones en Azure Portal](alerts/action-groups.md): se han agregado direcciones IP de origen para webhooks.

### <a name="application-insights"></a>Application Insights
- [Supervisión de aplicaciones sin código de Java con Azure Monitor Application Insights](app/java-in-process-agent.md): se ha agregado un ejemplo de configuración.
- [Complemento React para el SDK de JavaScript de Application Insights](app/javascript-react-plugin.md): se ha agregado una sección sobre el uso de enlaces de React.
- [Actualización desde la versión 2.x del SDK de Java para Application Insights](app/java-standalone-upgrade-from-2x.md): nuevo artículo.
- [Notas de la versión para Microsoft.ApplicationInsights.SnapshotCollector](app/snapshot-collector-release-notes.md): nuevo artículo.

### <a name="autoscale"></a>Escalado automático
- [Introducción a la escalabilidad automática en Azure](autoscale/autoscale-get-started.md): se ha agregado una sección sobre cómo mover la escalabilidad automática a una región diferente.

### <a name="data-collection"></a>datos, recopilación
- [Configuración de la recopilación de datos para el agente de Azure Monitor (versión preliminar)](agents/data-collection-rule-azure-monitor-agent.md): se ha actualizado para la compatibilidad con Azure Arc.
- [Reglas de recopilación de datos en Azure Monitor (versión preliminar)](agents/data-collection-rule-overview.md): se ha actualizan para la compatibilidad con Azure Arc.
- [Ejemplos de plantillas de Resource Manager para reglas de recopilación de datos](agents/resource-manager-data-collection-rules.md): nuevo artículo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones
- [Conexión de Azure a las herramientas de ITSM mediante Exportación segura](alerts/it-service-management-connector-secure-webhook-connections.md): se ha agregado una sección sobre la conexión a ServiceNow.

### <a name="logs"></a>Registros
- [Integración de Log Analytics y Excel](logs/log-excel.md): nuevo artículo.
- [Seguridad de datos de Log Analytics](logs/data-security.md): se ha agregado una sección sobre características de seguridad adicionales.
- [Integración de Log Analytics con Power BI](logs/log-powerbi.md): nuevo artículo.
- [Columnas estándar en registros de Azure Monitor](logs/log-standard-columns.md): se ha agregado la columna SubscriptionId.

Artículos nuevos y actualizados a partir de la reestructuración del contenido de las consultas de registro.

- [Tutorial de Log Analytics](logs/log-analytics-tutorial.md)
- [Consultas de registro en Azure Monitor](logs/log-query-overview.md)
- [Introducción a Log Analytics en Azure Monitor](logs/log-analytics-overview.md)
- [Ejemplos de consultas para Azure Data Explorer y Azure Monitor](/azure/data-explorer/kusto/query/samples?pivots=azuremonitor)
- [Tutorial: Uso de consultas de Kusto en Azure Data Explorer y Azure Monitor](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor)



### <a name="virtual-machines"></a>Máquinas virtuales

- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md): se han agregado regiones admitidas.

Nuevos artículos sobre el estado de invitado de VM Insights (versión preliminar)

- [Estado de invitado de VM Insights (versión preliminar)](vm/vminsights-health-overview.md)
- [Alertas de estado de invitado de VM Insights (versión preliminar)](vm/vminsights-health-alerts.md)
- [Configuración de la supervisión en el estado de invitado de VM Insights (versión preliminar)](vm/vminsights-health-configure.md)
- [Configuración de la supervisión en el estado de invitado de VM Insights mediante reglas de recopilación de datos (versión preliminar)](vm/vminsights-health-configure-dcr.md)
- [Habilitación del estado de invitado de VM Insights (versión preliminar)](vm/vminsights-health-enable.md)
- [Solución de problemas de estado de invitado de VM Insights (versión preliminar)](vm/vminsights-health-troubleshoot.md)





## <a name="october-2020"></a>Octubre de 2020

### <a name="general"></a>General
- [Retirada de la API Azure Monitor](logs/operationalinsights-api-retirement.md): artículo nuevo.

### <a name="agents"></a>Agentes
- [Qué supervisa Azure Monitor](monitor-reference.md): se ha agregado una sección sobre agentes.

### <a name="alerts"></a>Alertas
- [Creación y administración de grupos de acciones en Azure Portal](alerts/action-groups.md): se ha agregado una sección sobre la etiqueta de servicio.
- [Ejemplos de plantillas de Resource Manager para alertas de métricas](alerts/resource-manager-alerts-metric.md): se ha agregado el parámetro de coincidencia y las ubicaciones de prueba.
- [Solución de problemas con las alertas de métricas de Azure](alerts/alerts-troubleshoot-metric.md): se ha agregado un procedimiento recomendado para la configuración de reglas.

### <a name="application-insights"></a>Application Insights
- [Complemento Angular para el SDK de JavaScript de Application Insights](app/javascript-angular-plugin.md): artículo nuevo.
- [Azure Application Insights para aplicaciones de ASP.NET Core](app/asp-net-core.md): se han agregado preguntas frecuentes sobre los registros de ILogger.
- [Configuración de la supervisión para ASP.NET con Azure Application Insights](app/asp-net.md): se ha reescrito el artículo.
- [Métricas agregadas previamente y basadas en registros en Application Insights](app/pre-aggregated-metrics-log-metrics.md): se han agregado tablas con métricas previamente agregadas.
- [Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sito web](app/monitor-web-app-availability.md): se ha agregado una sección sobre etiquetas para rellenar la ubicación.
- [Supervisión de aplicaciones Java en cualquier lugar: Application Insights de Azure Monitor](app/java-standalone-config.md): se ha agregado un ejemplo de configuración.
- [Supervisión de aplicaciones Java en cualquier lugar: Application Insights de Azure Monitor](app/java-standalone-telemetry-processors.md): artículo nuevo.
- [Uso de Application Change Analysis en Azure Monitor para encontrar problemas de las aplicaciones web](app/change-analysis.md): se han agregado secciones sobre máquinas virtuales y el registro de actividad.
  
### <a name="autoscale"></a>Escalado automático
- [Introducción al escalado automático en Azure](autoscale/autoscale-get-started.md): se ha agregado una sección sobre cómo mover el escalado automático a una región diferente.

### <a name="containers"></a>Contenedores
- [Configuración de la supervisión de VP con Container Insights](containers/container-insights-persistent-volumes.md): nuevo artículo.
- [Administración del agente de Container Insights](containers/container-insights-manage-agent.md): se ha agregado compatibilidad con el clúster de Kubernetes habilitado para Azure Arc.
- [Alertas de métricas de Container Insights](containers/container-insights-metric-alerts.md): se ha agregado compatibilidad con el clúster de Kubernetes habilitado para Azure Arc.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones
- [Conector de Administración de servicios de TI: Exportación segura en Azure Monitor](alerts/it-service-management-connector-secure-webhook-connections.md): se ha agregado una sección sobre ServiceNow.

### <a name="logs"></a>Registros
- [Archivado de datos de un área de trabajo de Log Analytics en Azure Storage mediante Logic Apps](logs/logs-export-logic-app.md): artículo nuevo.
- [Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)](logs/logs-data-export.md): se ha agregado un cuerpo de ejemplo para la solicitud de REST para el centro de eventos.
- [Administración del uso y los costos con los registros de Azure Monitor](logs/manage-cost-storage.md): se ha agregado información sobre la relación entre los registros de Azure Monitor y la facturación de Azure Security Center. Se ha agregado una consulta para los recuentos de nodos si se usa el plan de tarifa por nodo. 
- [Supervisión del estado del área de trabajo de Log Analytics en Azure Monitor](logs/monitor-workspace.md): artículo nuevo.
- [Consulta de datos en Azure Monitor mediante Azure Data Explorer (versión preliminar)](logs/azure-data-explorer-monitor-proxy.md): artículo nuevo.
- [Consulta de datos exportados desde Azure Monitor mediante Azure Data Explorer (versión preliminar)](logs/azure-data-explorer-query-storage.md): artículo nuevo.

### <a name="networks"></a>Redes
- [Azure Monitor para redes (versión preliminar)](insights/network-insights-overview.md): se ha agregado la sección de solución de problemas. Se ha agregado la sección sobre conectividad.

### <a name="platform-logs"></a>Registros de plataforma
- [Esquema de eventos del registro de actividad de Azure](essentials/activity-log-schema.md): se ha agregado la descripción de los niveles de gravedad.

### <a name="virtual-machines"></a>Máquinas virtuales
- [Análisis de cambios en Azure Monitor para VM](vm/vminsights-change-analysis.md): artículo nuevo.
- [Información general sobre la habilitación de Azure Monitor para VM](vm/vminsights-enable-overview.md): se han agregado las regiones admitidas.
- [Actualización de Container Insights para las métricas](containers/container-insights-update-metrics.md): se ha agregado compatibilidad con el clúster de Kubernetes habilitado para Azure Arc.



## <a name="september-2020"></a>Septiembre de 2020

### <a name="general"></a>General
- [Preguntas frecuentes sobre Azure Monitor](faq.md): sección agregada sobre OpenTelemetry.

### <a name="agents"></a>Agentes
- [Información general del agente de Azure Monitor](agents/azure-monitor-agent-overview.md): se han agregado factores de decisión para cambiar al nuevo agente.
- [Información general sobre los agentes de Azure Monitor](agents/agents-overview.md): se ha agregado compatibilidad con Windows 10.

### <a name="alerts"></a>Alertas
- [Creación de una alerta de registro con una plantilla de Azure Resource Manager](alerts/alerts-log-create-templates.md): artículo nuevo.
- [Solución de problemas en las alertas de métricas de Azure Monitor](alerts/alerts-troubleshoot-metric.md): se ha agregado una sección sobre la exportación de plantillas de Resource Manager para una regla de alertas de métricas.

### <a name="application-insights"></a>Application Insights
- [Creación de recursos basados en áreas de trabajo de Application Insights para Azure Monitor](app/create-workspace-resource.md): se ha eliminado la designación de versión preliminar.
- [Retención y almacenamiento de datos en Azure Application Insights](app/data-retention-privacy.md): se han agregado detalles sobre la nueva compatibilidad con la protección contra pérdida de datos en Mac y Linux.
- [Contadores de eventos en Application Insights](app/eventcounters.md): se han agregado notas sobre los contadores recopilados de forma predeterminada.
- [Métricas agregadas previamente y basadas en registros en Application Insights](app/pre-aggregated-metrics-log-metrics.md): se ha eliminado la designación de versión preliminar.
- [Migración de recursos clásicos de Application Insights para Azure Monitor a recursos basados en el área de trabajo](app/convert-classic-resource.md): artículo nuevo.
- [Supervisión de aplicaciones Java en cualquier entorno: Application Insights para Azure Monitor](app/java-in-process-agent.md): se ha actualizado para la nueva versión preliminar del agente.
- [Configuración del análisis de aplicaciones web para ASP.NET con Azure Application Insights](app/asp-net.md): artículo escrito de nuevo.
- [Canales de telemetría en Azure Application Insights](app/telemetry-channels.md): se han agregado detalles de la nueva compatibilidad con la protección contra la pérdida de datos en Mac y Linux.
- [Solución de problemas de Snapshot Debugger de Azure Application Insights](app/snapshot-debugger-troubleshoot.md): se ha agregado la sección de SSL a la solución de problemas de Snapshot Debugger.
- [Uso de Application Change Analysis en Azure Monitor para encontrar problemas de las aplicaciones web](app/change-analysis.md): se han agregado Virtual Machine y el registro de actividad.


### <a name="containers"></a>Containers
- [Configuración del clúster de Kubernetes habilitado para Azure Arc con Container Insights](containers/container-insights-enable-arc-enabled-clusters.md): se han agregado instrucciones para habilitar la supervisión mediante la entidad de servicio.
- [Implementación y métricas de HPA con Container Insights](containers/container-insights-deployment-hpa-metrics.md): nuevo artículo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones
- [Azure Monitor para Azure Cache for Redis](insights/redis-cache-insights-overview.md): se ha eliminado la designación de versión preliminar.
- [Azure Monitor para redes (versión preliminar)](insights/network-insights-overview.md): se han agregado las secciones Conectividad y Tráfico.
- [Conector de Administración de servicios de TI: Exportación segura en Azure Monitor](alerts/it-service-management-connector-secure-webhook-connections.md): artículo nuevo.
- [Conector de Administración de servicios de TI en Azure Monitor](alerts/itsmc-connections.md): nota sobre las integraciones de ITSM de Cherwell y Provance.
- [Supervisión de Key Vault con Azure Monitor para Key Vault](insights/key-vault-insights-overview.md): se ha eliminado la designación de versión preliminar.

### <a name="logs"></a>Registros
- [Auditoría de las consultas en las consultas de registro de Azure Monitor](logs/query-audit.md): artículo nuevo.
- [Clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): se ha agregado la Caja de seguridad del cliente.
- [Clústeres dedicados de registros de Azure Monitor](logs/logs-dedicated-clusters.md): artículo nuevo.
- [Diseño de la implementación de registros de Azure Monitor](logs/design-logs-deployment.md): se ha actualizado la sección de límite de frecuencia del volumen de escala e ingesta.
- [Ámbito de consulta de registro en Log Analytics para Azure Monitor](logs/scope.md): actualizaciones para incluir aplicaciones basadas en el área de trabajo.
- [Registros de Azure Monitor](logs/data-platform-logs.md): actualizaciones para incluir aplicaciones basadas en el área de trabajo.
- [Columnas estándar en registros de Azure Monitor](logs/log-standard-columns.md): actualizaciones para incluir aplicaciones basadas en el área de trabajo.
- [Límites de servicio de Azure Monitor](service-limits.md): se han actualizado los límites de las consultas de usuario.
- [Uso de cuentas de almacenamiento administradas por el cliente en Log Analytics para Azure Monitor](logs/private-storage.md): artículo escrito nuevamente.
- [Visualización y análisis de datos en Azure Log Analytics](./logs/data-platform-logs.md): actualizaciones para incluir aplicaciones basadas en el área de trabajo.


### <a name="platform-logs"></a>Registros de plataforma
- [Esquema de eventos del registro de actividad de Azure: Azure Monitor](essentials/activity-log-schema.md): se han agregado niveles de gravedad.
- [Ejemplos de plantillas de Resource Manager para la configuración de diagnóstico](essentials/resource-manager-diagnostic-settings.md): se ha agregado un ejemplo para las cuentas de almacenamiento de Azure.

### <a name="visualizations"></a>Visualizaciones
- [Visualizaciones de gráficos de libro de Azure Monitor](visualize/workbooks-chart-visualizations.md): artículo nuevo.
- [Representador de barra compuesta de libro de Azure Monitor](visualize/workbooks-composite-bar.md): artículo nuevo.
- [Visualizaciones de grafos de libro de Azure Monitor](visualize/workbooks-graph-visualizations.md): artículo nuevo.
- [Visualizaciones de cuadrículas de libro de Azure Monitor](visualize/workbooks-grid-visualizations.md): artículo nuevo.
- [Visualizaciones de panales de libro de Azure Monitor](visualize/workbooks-honey-comb.md): artículo nuevo.
- [Visualizaciones de texto de libro de Azure Monitor](visualize/workbooks-text-visualizations.md): artículo nuevo.
- [Visualizaciones de iconos de libro de Azure Monitor](visualize/workbooks-tile-visualizations.md): artículo nuevo.
- [Visualizaciones de árbol de libro de Azure Monitor](visualize/workbooks-tree-visualizations.md): artículo nuevo.




## <a name="august-2020"></a>Agosto de 2020

### <a name="general"></a>General

- [Qué supervisa Azure Monitor](monitor-reference.md): se actualizó para incluir el agente de Azure Monitor.


### <a name="agents"></a>Agentes
- [Información general del agente de Azure Monitor](agents/azure-monitor-agent-overview.md): artículo nuevo.
- [Habilitación de Azure Monitor para un entorno híbrido](vm/vminsights-enable-hybrid.md): se actualizó la versión del agente de dependencia.
- [Información general de los agentes de supervisión de Azure](agents/agents-overview.md): se agregó el agente de Azure Monitor y se consolidó la tabla de compatibilidad de sistemas operativos.


#### <a name="new-and-updated-articles-from-restructure-of-agent-content"></a>Artículos nuevos y actualizados a partir de la reestructuración del contenido del agente
- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md)
- [Instalación del agente de Log Analytics en equipos Linux](agents/agent-linux.md)
- [Instalación del agente de Log Analytics en equipos Windows](agents/agent-windows.md)
- [Introducción al agente de Log Analytics](agents/log-analytics-agent.md)

### <a name="application-insights"></a>Application Insights
- [Azure Application Insights para aplicaciones web de JavaScript](app/javascript.md): se agregó la sección de clarificación de la correlación y configuración del servidor cliente para la correlación con CORS.
- [Creación de un nuevo recurso basado en el área de trabajo de Azure Monitor Application Insights](app/create-workspace-resource.md): se agregaron funcionalidades proporcionadas por aplicaciones basadas en el área de trabajo.
- [Direcciones IP usadas por Application Insights y Log Analytics](app/ip-addresses.md): se actualizaron las direcciones IP para Live Metrics Stream.
- [Supervisión de aplicaciones Java en cualquier entorno (Azure Monitor Application Insights)](app/java-in-process-agent.md): se agregó una tabla de la telemetría personalizada compatible.
- [Complemento Native React para el SDK de JavaScript de Application Insights](app/javascript-react-native-plugin.md): artículo nuevo.
- [Complemento Native React para el SDK de JavaScript de Application Insights](app/javascript-react-plugin.md): artículo nuevo.
- [Ejemplo de plantilla de Resource Manager para crear aplicaciones de Azure Functions con la supervisión de Application Insights](app/resource-manager-function-app.md): artículo nuevo.
- [Ejemplos de plantillas de Resource Manager para crear aplicaciones web de Azure App Services con la supervisión de Application Insights](app/resource-manager-web-app.md): artículo nuevo.
- [Análisis de uso con Azure Application Insights](app/usage-overview.md): se agregó un vídeo.

### <a name="autoscale"></a>Escalado automático
- [Introducción al escalado automático en Azure](autoscale/autoscale-get-started.md): se agregó una sección sobre el enrutamiento a instancias correctas de App Service.

### <a name="data-collection"></a>Recopilación de datos
- [Configuración de la recopilación de datos para el agente de Azure Monitor (versión preliminar)](agents/data-collection-rule-azure-monitor-agent.md): artículo nuevo.
- [Reglas de recopilación de datos en Azure Monitor (versión preliminar)](agents/data-collection-rule-overview.md): artículo nuevo.


### <a name="containers"></a>Contenedores
- [Implementación y métricas de HPA con Container Insights](containers/container-insights-deployment-hpa-metrics.md): nuevo artículo.

### <a name="insights"></a>Información detallada
- [Soluciones de supervisión en Azure Monitor](insights/solutions.md): se actualizaron para la nueva interfaz de usuario.
- [Solución Network Performance Monitor en Azure](insights/network-performance-monitor.md): se agregaron regiones de área de trabajo compatibles.


### <a name="logs"></a>Registros
- [Preguntas más frecuentes de Azure Monitor](faq.md): se agregó una entrada de eliminación de datos de un área de trabajo. Se agregó una entrada en las respuestas 502 y 503.
  - [Diseño de la implementación de registros de Azure Monitor](logs/design-logs-deployment.md): actualizaciones en la sección de límite de frecuencia del volumen de ingesta.
- [Administración del uso y los costos de los registros de Azure Monitor](logs/manage-cost-storage.md): se actualizaron las consultas de uso a un formato de consulta más eficaz.
- [Optimización de las consultas de registros en Azure Monitor](logs/query-optimization.md): se agregaron valores específicos para los indicadores de rendimiento.
- [Ejemplos de plantillas de Resource Manager](essentials/resource-manager-diagnostic-settings.md): se agregó un ejemplo para los registros de auditoría de las consultas de registro.


### <a name="platform-logs"></a>Registros de plataforma
- [Creación de una configuración de diagnóstico para enviar registros de plataforma y métricas a diferentes destinos](essentials/diagnostic-settings.md): se agregó la configuración regional de requisitos de diagnóstico.

### <a name="visualizations"></a>Visualizaciones
- [Introducción a los libros de Azure Monitor](visualize/workbooks-overview.md): se agregó un vídeo.
- [Traslado de una plantilla de libro de Azure a otra región](visualize/workbook-templates-move-region.md): artículo nuevo.
- [Traslado de un libro de Azure a otra región](visualize/workbooks-move-region.md): artículo nuevo.



## <a name="july-2020"></a>Julio de 2020

### <a name="general"></a>General
- [Implementación de Azure Monitor](deploy-scale.md): reestructuración del contenido de la incorporación de VM Insights.
- [Uso de Azure Private Link para conectar redes a Azure Monitor de forma segura](logs/private-link-security.md): se agrega la sección en los límites.

### <a name="alerts"></a>Alertas
- [Reglas de acción para alertas de Azure Monitor](alerts/alerts-action-rules.md): se han agregado procesos de la CLI.
- [Creación y administración de grupos de acciones en Azure Portal](alerts/action-groups.md): se ha actualizado para reflejar los cambios en la interfaz de usuario.
- [Consultas de ejemplo en Log Analytics de Azure Monitor](logs/example-queries.md): artículo nuevo.
- [Solución de problemas de alertas de registro en Azure Monitor](alerts/alerts-troubleshoot-log.md): se ha agregado la sección de la cuota de reglas de alerta.
- [Solución de problemas en las alertas de métricas de Azure Monitor](alerts/alerts-troubleshoot-metric.md): se ha agregado la sección sobre la regla de alerta de una métrica personalizada que todavía no se ha emitido.
- [Descripción del funcionamiento de las alertas de métricas en Azure Monitor.](alerts/alerts-metric-overview.md) : se ha agregado una recomendación para seleccionar la granularidad de la agregación.

### <a name="application-insights"></a>Application Insights
- [Notas de la versión para la extensión de Azure Web Apps - Application Insights](app/web-app-extension-release-notes.md): nuevo artículo.
- [Ejemplos de plantilla de Resource Manager para la creación de recursos de Application Insights](app/resource-manager-app-resource.md): nuevo artículo.
- [Solución de problemas con Azure Application Insights Profiler](app/profiler-troubleshooting.md): se ha agregado una nota sobre el error al ejecutar Profiler para aplicaciones de ASP.NET Core en Azure App Service. 

### <a name="containers"></a>Contenedores
- [Alertas de registro de Container Insights](containers/container-insights-log-alerts.md): nuevo artículo.
- [Alertas de métricas de Container Insights](containers/container-insights-metric-alerts.md): nuevo artículo.

### <a name="logs"></a>Registros
- [Clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): se agrega el mensaje de error y la sección Configuración de CMK para las consultas.
- [HTTP Data Collector API de Azure Monitor](logs/data-collector-api.md): se ha agregado el ejemplo de Python 3.
- [Optimización de las consultas de registro en Azure Monitor](logs/query-optimization.md): se ha agregado la sección para evitar varios análisis de datos al usar subconsultas.
- [Tutorial: Introducción a las consultas de Log Analytics](./logs/log-analytics-tutorial.md): se ha agregado un vídeo.

### <a name="platform-logs"></a>Registros de plataforma
- [Creación de una configuración de diagnóstico para enviar los registros y las métricas de la plataforma a diferentes destinos](essentials/diagnostic-settings.md): se ha agregado un vídeo.
- [Ejemplos de plantillas de Resource Manager para Azure Monitor](/resource-manager-samples.md): se ha agregado un ejemplo de ARM con el tipo de destino Logs. 

### <a name="solutions"></a>Soluciones
- [Soluciones de supervisión en Azure Monitor](insights/solutions.md): se han agregado procesos de la CLI.
- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): se ha cambiado la fecha de retirada.

### <a name="virtual-machines"></a>Máquinas virtuales

Artículos nuevos y actualizados a partir de la reestructuración del contenido de VM Insights

- [¿Qué es VM Insights?](vm/vminsights-overview.md)
- [Configuración del área de trabajo de Log Analytics para VM Insights](vm/vminsights-configure-workspace.md)
- [Conexión de equipos Linux a Azure Monitor](agents/agent-linux.md)
- [Habilitar Azure Monitor para un entorno híbrido](vm/vminsights-enable-hybrid.md)
- [Habilitar Azure Monitor para VM para una sola máquina virtual o un conjunto de escalado de máquinas virtuales mediante Azure Portal](vm/vminsights-enable-portal.md)
- [Habilitación de VM Insights mediante Azure Policy](./vm/vminsights-enable-policy.md)
- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md)
- [Habilitación de VM Insights mediante PowerShell](vm/vminsights-enable-powershell.md)
- [Habilitación de VM Insights con plantillas de Resource Manager](vm/vminsights-enable-resource-manager.md)
- [Habilitación de VM Insights con PowerShell o plantillas](./vm/vminsights-enable-powershell.md)


### <a name="visualizations"></a>Visualizaciones
- [Actualización de las visualizaciones del panel de Log Analytics](logs/dashboard-upgrade.md): se ha actualizado la frecuencia de actualización.
- [Visualización de datos de Azure Monitor](visualizations.md): se ha agregado un vídeo.


## <a name="june-2020"></a>Junio de 2020

### <a name="general"></a>General
- [Implementación de Azure Monitor](deploy-scale.md). Nuevo artículo.
- [Clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md). Propiedad billingtype actualizada. Comandos PowerShell incorporados.

### <a name="agents"></a>Agentes
- [Introducción al agente de Log Analytics](agents/log-analytics-agent.md). Requisito de Python 2 incorporado.

### <a name="alerts"></a>Alertas
- [Actualización de reglas de alertas o reglas de acción cuando su recurso de destino se mueve a otra región de Azure](alerts/alerts-resource-move.md). Nuevo artículo.
- [Solución de problemas en las alertas de métricas de Azure Monitor](alerts/alerts-troubleshoot-metric.md). Nuevo artículo.
- [Solución de problemas de alertas de registro en Azure Monitor](alerts/alerts-troubleshoot-metric.md). Nuevo artículo.
  
### <a name="application-insights"></a>Application Insights
- [Azure Application Insights para aplicaciones web de JavaScript](app/javascript.md). Actualización de la sección del SDK de JavaScript. Fragmento de código actualizado para informar sobre errores de carga.
- [Configuración de BYOS (Traiga su propio almacenamiento) para Profiler y Snapshot Debugger](app/profiler-bring-your-own-storage.md). Nuevo artículo.
- [Seguimiento de las solicitudes entrantes en Azure Application Insights con OpenCensus para Python](app/opencensus-python-request.md). Registro y configuración actualizadas para OpenCensus.
- [Supervisión de una aplicación web de ASP.NET en vivo con Azure Application Insights](app/monitor-performance-live-website-now.md): se ha actualizado la fecha de entrada en desuso de Status Monitor v1.
- [Supervisión de servicios de node. js con Azure Application Insights](app/nodejs.md). Varias actualizaciones, incluida la migración desde versiones anteriores y la configuración del SDK
- [Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar)](app/opencensus-python.md). Incorporación de una sección sobre la configuración de los exportadores de Azure Monitor.
- [Supervisión de las aplicaciones sin cambios de código: instrumentación automática para Azure Monitor Application Insights](app/codeless-overview.md). Nuevo artículo.
- [Solución de problemas de errores de carga del SDK para aplicaciones web de JavaScript](app/javascript-sdk-load-failure.md). Nuevo artículo.

### <a name="containers"></a>Contenedores
- [Cómo detener la supervisión del clúster híbrido de Kubernetes](containers/container-insights-optout-hybrid.md). Sección agregada para Kubernetes habilitado para Arc.
- [Configuración de un clúster de Kubernetes habilitado para Azure Arc con Container Insights](containers/container-insights-enable-arc-enabled-clusters.md): nuevo artículo.
- [Configuración de Red Hat OpenShift en Azure v4.x con Container insights](containers/container-insights-azure-redhat4-setup.md): se han actualizado los requisitos previos.
- [Configuración de datos en directo de Container Insights (versión preliminar)](containers/container-insights-livedata-setup.md): se ha eliminado la nota sobre la característica que no está disponible en Azure US Government.

### <a name="insights"></a>Información detallada
- [Preguntas más frecuentes: Solución Network Performance Monitor en Azure](insights/network-performance-monitor-faq.md). Incorporación de preguntas más frecuentes sobre Supervisión de ExpressRoute.

### <a name="logs"></a>Registros
- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](logs/delete-workspace.md). Incorporación de un comando de PowerShell. Actualización de Solución de problemas.
- [Administración de áreas de trabajo de Log Analytics en Azure Monitor](logs/manage-access.md). Incorporación de ejemplo para tablas no permitidas en la sección de RBAC de Azure.
- [Administración del uso y los costos con los registros de Azure Monitor](logs/manage-cost-storage.md). Detalles adicionales sobre el cálculo del tamaño de los datos. Se actualizó la configuración de alertas de volumen de datos. Detalles sobre los datos de seguridad recopilados por Azure Sentinel. Aclaración sobre el límite de datos.
- [Uso de Azure Monitor Logs con Azure Logic Apps y Power Automate](logs/logicapp-flow-connector.md): se han agregado límites del conector.

### <a name="metrics"></a>Métricas
- [Métricas compatibles de Azure Monitor por tipo de recurso](essentials/metrics-supported.md). Actualización de métricas de SQL Server.


### <a name="platform-logs"></a>Registros de plataforma

- [Ejemplos de plantillas de Resource Manager para la configuración de diagnóstico](essentials/resource-manager-diagnostic-settings.md). Corrección de la configuración de diagnóstico del registro de actividad.
- [Envío del registro de actividad de Azure al área de trabajo de Log Analytics mediante Azure Portal](essentials/quick-collect-activity-log-portal.md). Nuevo artículo.
- [Envío del registro de actividad de Azure al área de trabajo de Log Analytics mediante una plantilla de Azure Resource Manager](essentials/quick-collect-activity-log-arm.md). Nuevo artículo.

Artículos nuevos y actualizados sobre reestructuración y consolidación del contenido de registros de plataforma

- [Archivado de registros de recursos de Azure en la cuenta de almacenamiento](./essentials/resource-logs.md#send-to-azure-storage)
- [Esquema de eventos del registro de actividad de Azure](essentials/activity-log-schema.md)
- [Registro de actividad de Azure](essentials/activity-log.md)
- [Ejemplos de la CLI de Azure Monitor](/cli-samples.md)
- [Ejemplos de PowerShell de Azure Monitor](/powershell-samples.md)
- [Tutorial sobre la API de REST de supervisión de Azure](essentials/rest-api-walkthrough.md)
- [Servicios y esquemas admitidos de los registros de recursos de Azure](./essentials/resource-logs-schema.md)
- [Registros de recursos de Azure](essentials/resource-logs.md)
- [Recopilación y análisis del registro de actividad de Azure en Azure Monitor](./essentials/activity-log.md)
- [Recopilación de registros de recursos de Azure en el área de trabajo de Log Analytics](./essentials/resource-logs.md#send-to-log-analytics-workspace)
- [Creación de una configuración de diagnóstico para enviar registros de plataforma y métricas a diferentes destinos](essentials/diagnostic-settings.md)
- [Exportación del registro de actividad de Azure](./essentials/activity-log.md#legacy-collection-methods)
- [Introducción a los registros de plataforma Azure](essentials/platform-logs-overview.md)
- [Transmisión de registros de plataforma de Azure a un centro de eventos](./essentials/resource-logs.md#send-to-azure-event-hubs)
- [Visualización de eventos del registro de actividad de Azure en Azure Monitor](./essentials/activity-log.md#view-the-activity-log)

### <a name="virtual-machines"></a>Máquinas virtuales
- [Habilitación de VM Insights en Azure Portal](./vm/vminsights-enable-portal.md): se ha actualizado para incluir Azure Arc.
- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md): se ha actualizado para incluir Azure Arc.
- [¿Qué es VM Insights?](vm/vminsights-overview.md) Actualización para incluir Azure Arc.


### <a name="visualizations"></a>Visualizaciones
- [Orígenes de datos de libros de Azure Monitor](visualize/workbooks-data-sources.md). Incorporación de las secciones de Alertas y Punto de conexión personalizado.
- [Solución de problemas de conclusiones basadas en libros de Azure Monitor](insights/troubleshoot-workbooks.md). Nuevo artículo.
- [Actualización de las visualizaciones del panel de Log Analytics](logs/dashboard-upgrade.md). Nuevo artículo.



## <a name="may-2020"></a>Mayo de 2020

### <a name="general"></a>General

- [Preguntas más frecuentes de Azure Monitor](faq.md): se ha agregado una sección de métricas.
- [Clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): varios cambios en la preparación para la disponibilidad general.
- [Definiciones de directivas integradas para Azure Monitor](./policy-reference.md): nuevo artículo.
- [Cuentas de almacenamiento de propiedad del cliente para la ingesta de registros](logs/private-storage.md): nuevo artículo.
- [Administrar el uso y los costos con los registros de Azure Monitor](logs/manage-cost-storage.md): se ha agregado la facturación proporcional de los clústeres.
- [Uso de Azure Private Link para conectar redes a Azure Monitor de forma segura](logs/private-link-security.md): nuevo artículo.


#### <a name="new-resource-manager-template-samples"></a>Nuevos ejemplos de plantillas de Resource Manager 
- [Ejemplos de plantillas de Resource Manager para Azure Monitor](/resource-manager-samples.md)
- [Ejemplos de plantillas de Resource Manager para grupos de acciones](alerts/resource-manager-action-groups.md)
- [Ejemplos de plantillas de Resource Manager para agentes](agents/resource-manager-agent.md)
- [Ejemplos de plantilla de Resource Manager para Container Insights](containers/resource-manager-container-insights.md)
- [Ejemplos de plantilla de Resource Manager para VM Insights](vm/resource-manager-vminsights.md)
- [Ejemplos de plantillas de Resource Manager para la configuración de diagnóstico](essentials/resource-manager-diagnostic-settings.md)
- [Ejemplos de plantillas de Resource Manager para áreas de trabajo de Log Analytics](logs/resource-manager-workspace.md)
- [Ejemplos de plantillas de Resource Manager para consultas de registros](logs/resource-manager-log-queries.md)
- [Ejemplos de plantillas de Resource Manager para reglas de alertas de consultas de registros](alerts/resource-manager-alerts-log.md)
- [Ejemplos de plantillas de Resource Manager para reglas de alertas de métricas](alerts/resource-manager-alerts-metric.md)
- [Ejemplos de plantillas de Resource Manager para libros](visualize/resource-manager-workbooks.md)

### <a name="agents"></a>Agentes
- [Instalación y configuración de la extensión de Diagnósticos de Azure para Windows (WAD)](agents/diagnostics-extension-windows-install.md): detalle agregado sobre la configuración de diagnóstico.
- [Información general del agente de Log Analytics](agents/log-analytics-agent.md): se han agregado versiones de Linux compatibles.

### <a name="application-insights"></a>Application Insights

- [Supervisión de aplicaciones que se ejecutan en Azure Functions con Application Insights: Azure Monitor](app/monitor-functions.md): nuevo artículo.
- [Supervisión de los servicios de Node.js con Azure Application Insights](app/nodejs.md): actualizaciones generales, incluida la nueva sección sobre la migración desde versiones anteriores.
- [Direcciones IP usadas por Application Insights y Log Analytics](app/ip-addresses.md): se han agregado direcciones IP para webhooks y US Government.
- [Supervisión de aplicaciones en Azure Kubernetes Service (AKS) con Application Insights: Azure Monitor](app/kubernetes-codeless.md): nuevo artículo.
- [Solución de problemas cuando no hay datos: Application Insights para .NET](app/asp-net-troubleshoot-no-data.md): se ha agregado una sección sobre la recopilación de registros con dotnet-trace.
- [Uso de Application Change Analysis en Azure Monitor para detectar problemas de la aplicación web](app/change-analysis.md): varias actualizaciones en la característica Change Analysis.

#### <a name="new-and-updated-articles-for-preview-of-workspace-based-applications"></a>Artículos nuevos y actualizados para la versión preliminar de aplicaciones basadas en el área de trabajo
- [Esquema de recursos basado en el área de trabajo de Application Insights de Azure Monitor](app/apm-tables.md)
- [Creación de nuevos recursos basados en área de trabajo de Application Insights de Azure Monitor](app/create-workspace-resource.md)
- [Expresión app() en consultas de registro de Azure Monitor](logs/app-expression.md)
- [Ámbito de la consulta de registro en Log Analytics de Azure Monitor](logs/scope.md)
- [Consulta entre recursos con Azure Monitor](logs/cross-workspace-query.md)
- [Propiedades estándar de los registros de Azure Monitor](./logs/log-standard-columns.md)
- [Estructura de registros de Azure Monitor](./logs/data-platform-logs.md)





### <a name="containers"></a>Contenedores
- [Cómo habilitar Container Insights](containers/container-insights-onboard.md): se ha actualizado la tabla de configuración del firewall.
- [Actualización de Container Insights para las métricas](containers/container-insights-update-metrics.md): actualización para usar identidades administradas para recopilar métricas.
- [Supervisión del costo de Container Insights](containers/container-insights-cost.md): nuevo artículo.
- [Configuración de datos en directo de Container Insights (versión preliminar)](containers/container-insights-livedata-setup.md): soporte técnico para el nuevo enlace de rol de clúster.

### <a name="insights"></a>Información detallada
- [Azure Monitor para Azure Cache for Redis (versión preliminar)](insights/redis-cache-insights-overview.md): nuevo artículo.
- [Supervisión de Key Vault con Azure Monitor para Key Vault (versión preliminar)](./insights/key-vault-insights-overview.md): nuevo artículo.

### <a name="logs"></a>Registros
- [Creación y configuración de Log Analytics con PowerShell](logs/powershell-workspace-configuration.md): se ha agregado una sección de solución de problemas.
- [Creación de un área de trabajo de Log Analytics en Azure Portal](logs/quick-create-workspace.md): se ha agregado una sección de solución de problemas.
- [Creación de un área de trabajo de Log Analytics mediante la CLI de Azure](logs/quick-create-workspace-cli.md): se ha agregado una sección de solución de problemas.
- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](logs/delete-workspace.md): se ha actualizado la información sobre la recuperación de un área de trabajo eliminada.
- [Funciones de las consultas de registros de Azure Monitor](logs/functions.md): se ha quitado la nota sobre las funciones que no contienen otras funciones.
- [Estructura de registros de Azure Monitor](./logs/data-platform-logs.md): se han aclarado las descripciones de propiedades de la tabla de Application Insights.
- [Uso de Azure Monitor Logs con Azure Logic Apps y Power Automate](logs/logicapp-flow-connector.md): se ha agregado una sección de límites.
- [Uso de PowerShell para crear y configurar un área de trabajo de Log Analytics](logs/powershell-workspace-configuration.md): se ha agregado una sección de solución de problemas.


### <a name="metrics"></a>Métricas
- [Métricas compatibles de Azure Monitor por tipo de recurso](essentials/metrics-supported.md): se han aclarado las métricas de invitado y el enrutamiento de métricas. 

### <a name="solutions"></a>Soluciones
- [Optimización del entorno de Active Directory con Azure Monitor](insights/ad-assessment.md): se ha agregado Windows Server 2019 a las versiones compatibles.
- [Optimización del entorno de SQL Server con Azure Monitor](insights/sql-assessment.md): se ha agregado a las versiones compatibles de SQL Server.


### <a name="virtual-machines"></a>Máquinas virtuales
- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md): se ha agregado a las versiones compatibles de Ubuntu Server. Se han agregado regiones admitidas para el área de trabajo de Log Analytics.
- [Cómo representar el rendimiento en gráficos con VM Insights](vm/vminsights-performance.md): se ha agregado una sección de limitaciones para las métricas no disponibles.

### <a name="visualizations"></a>Visualizaciones
- [Libros de Azure Monitor y plantillas de Azure Resource Manager](visualize/workbooks-automate.md): se ha agregado una actualización de Resource Manager para implementar una plantilla de libro.
- [Grupos en los libros de Azure Monitor](./visualize/workbooks-groups.md): nuevo artículo.
- [Libros de Azure Monitor: transformación de datos JSON con JSONPath](visualize/workbooks-jsonpath.md): nuevo artículo.


## <a name="april-2020"></a>Abril de 2020

### <a name="general"></a>General

- [Configuración de la clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): se ha agregado una sección sobre operaciones asincrónicas.
- [Administración del acceso a los datos de registro y las áreas de trabajo en Azure Monitor](logs/manage-access.md): se ha actualizado la sección de registros personalizados.

### <a name="alerts"></a>Alertas

- [Reglas de acción (versión preliminar)](alerts/alerts-action-rules.md): se ha agregado un vídeo.
- [Información general sobre las alertas en Microsoft Azure](alerts/alerts-overview.md): se ha agregado un vídeo.

### <a name="application-insights"></a>Application Insights

- [Mapa de aplicación: Evaluación de prioridades de las aplicaciones distribuidas](app/app-map.md): se ha agregado la configuración de nombres de roles de nube para el agente de Java.
- [Referencia de la API del agente de .NET de Azure Application Insights](app/status-monitor-v2-api-reference.md): se ha consolidado la referencia de la API.
- [Direcciones IP que emplean Application Insights y Log Analytics](app/ip-addresses.md): se han actualizado las direcciones IP para App Insights y las API de Log Analytics, webhooks de grupo de acciones, Azure US Government.
- [Supervisión de aplicaciones de Java en cualquier sitio](app/java-standalone-config.md): nuevo artículo.
- [Supervisión de aplicaciones de Java en cualquier entorno](app/java-in-process-agent.md): nuevo artículo.
- [Supervisión de aplicaciones de Java que se ejecutan en cualquier entorno](app/java-standalone-arguments.md): nuevo artículo.
- [Supervisión de aplicaciones de Java que se ejecutan en el entorno local](app/java-on-premises.md): nuevo artículo.
- [Procedimiento para quitar Application Insights en Visual Studio](app/remove-application-insights.md): nuevo artículo.
- [Muestreo en Application Insights](app/sampling.md): se ha corregido el ejemplo de frecuencia fija en Python.

### <a name="containers"></a>Contenedores

- [Configuración de Red Hat OpenShift en Azure v4.x con Container insights](containers/container-insights-azure-redhat4-setup.md): nuevo artículo.
- [Cómo corregir manualmente los problemas de sincronización de ServiceNow](alerts/itsmc-resync-servicenow.md): nuevo artículo.
- [Cómo detener la supervisión del clúster de Azure y Red Hat OpenShift v4](containers/container-insights-optout-openshift-v4.md): nuevo artículo.
- [Cómo detener la supervisión del clúster de Red Hat OpenShift en Azure v3](containers/container-insights-optout-openshift-v3.md): nuevo artículo.
- [Cómo detener la supervisión del clúster de Kubernetes híbrido](containers/container-insights-optout-hybrid.md): nuevo artículo.

### <a name="insights"></a>Información detallada

- [Supervisión de Azure Key Vault con Azure Monitor para Key Vault (versión preliminar)](insights/key-vault-insights-overview.md): nuevo artículo.

### <a name="logs"></a>Registros

- [Límites de servicio de Azure Monitor](service-limits.md): se ha agregado la limitación de consultas de usuario.
- [Administrar el uso y los costos con los registros de Azure Monitor](logs/manage-cost-storage.md): se ha agregado la facturación de los clústeres de registros. Se ha agregado una consulta de Kusto para que los clientes con el plan de tarifa por nodo heredado puedan determinar si deben cambiar a un nivel de reserva de capacidad o por GB.

### <a name="metrics"></a>Métricas

- [Características avanzadas del Explorador de métricas de Azure](essentials/metrics-charts.md): se ha agregado la sección de agregación.

### <a name="workbooks"></a>Workbooks

- [Libros de Azure Monitor y plantillas de Azure Resource Manager](visualize/workbooks-automate.md): se ha agregado una plantilla de Resource Manager para implementar una plantilla de libro.

## <a name="march-2020"></a>Marzo de 2020

### <a name="general"></a>General

- [Introducción a Azure Monitor](overview.md): se ha agregado un vídeo de introducción a Azure Monitor.
- [Configuración de la clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): actualizaciones generales.
- [Referencia de datos de Azure Monitor](/azure/azure-monitor/reference/): nuevo sitio.

### <a name="alerts"></a>Alertas

- [Crear, ver y administrar las alertas del registro de actividad mediante Azure Monitor](alerts/alerts-activity-log.md): explicación adicional de la plantilla de Resource Manager.
- [Descripción del funcionamiento de las alertas de métricas en Azure Monitor.](alerts/alerts-metric-overview.md) : se ha actualizado para la compatibilidad con la Administración Pública.
- [Solución de problemas de alertas y notificaciones de Azure Monitor](alerts/alerts-troubleshoot.md): nuevo artículo.

### <a name="application-insights"></a>Application Insights

- [Automatización de Azure Application Insights con PowerShell](app/powershell.md): se han agregado ejemplos de ARMClient.
- [Exportación continua de telemetría desde Application Insights](app/export-telemetry.md):se ha agregado una tabla con detalles de la estructura de exportación.
- [Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service](app/snapshot-debugger-appservice.md): se ha agregado una plantilla de Resource Manager de ejemplo.
- [Administración del uso y los costos de Application Insights](app/pricing.md): se ha agregado información sobre la alerta de límite de datos.
- [Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar)](app/opencensus-python.md): se han agregado métricas estándar.
- [Compatibilidad del mapa de origen con las aplicaciones de JavaScript: Application Insights para Azure Monitor](app/source-map-support.md): nuevo artículo.

### <a name="containers"></a>Contenedores

- [Preguntas más frecuentes de Azure Monitor](faq.md): actualización para Container Insights.
- [Configuración de la supervisión de GPU con Container insights](containers/container-insights-gpu-monitoring.md): nuevo artículo.

### <a name="insights"></a>Información detallada

- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): fecha de desuso actualizada.

### <a name="logs"></a>Registros

- [Optimización de las consultas de registro en Azure Monitor](logs/query-optimization.md): se ha agregado la condición de CPU para el análisis de XML y JSON.
- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](logs/delete-workspace.md): se ha agregado información de solución de problemas.
- [Uso de registros de Azure Monitor con Azure Logic Apps y Power Automate](logs/logicapp-flow-connector.md): se ha actualizado con un nuevo conector de Azure Monitor.

### <a name="metrics"></a>Métricas

- [Desuso de métricas de disco en Azure Portal](essentials/portal-disk-metrics-deprecation.md): nuevo artículo.
- [Tutorial: Creación de un gráfico de métricas en Azure Monitor](essentials/tutorial-metrics-explorer.md): se ha agregado un vídeo.

### <a name="platform-logs"></a>Registros de plataforma

- [Recopilación y análisis del registro de actividad de Azure en Azure Monitor](./essentials/activity-log.md): se ha redactado de nuevo para explicar mejor la recopilación del registro de actividad con configuración de diagnóstico.

### <a name="virtual-machines"></a>Máquinas virtuales

- [Supervisión de máquinas virtuales de Azure con Azure Monitor](vm/monitor-vm-azure.md): nuevo artículo.
- [Inicio rápido: Supervisión de Azure Virtual Machines con Azure Monitor](vm/quick-monitor-azure-vm.md): se ha actualizado para agregar VM Insights.
- [Alertas de VM Insights](vm/vminsights-alerts.md): nuevo artículo.
- [Información general sobre la habilitación de VM Insights](vm/vminsights-enable-overview.md): se han actualizado los vínculos de descarga de agentes.

Actualizaciones generales para la disponibilidad general de VM Insights

- [¿Qué es VM Insights?](vm/vminsights-overview.md)
- [Preguntas más frecuentes de VM Insights (GA)](vm/vminsights-ga-release-faq.md) 
- [Habilitación de VM Insights mediante Azure Policy](./vm/vminsights-enable-policy.md) 
- [Representación gráfica del rendimiento con VM Insights](vm/vminsights-performance.md)
- [Procedimiento para consultar registros desde VM Insights](vm/vminsights-log-search.md)
- [Visualización de las dependencias de la aplicación con VM Insights](vm/vminsights-maps.md) 

### <a name="visualizations"></a>Visualizaciones

- [Visualización de datos de Azure Monitor](visualizations.md): se ha actualizado para indicar el desuso planeado del Diseñador de vistas.

## <a name="february-2020"></a>Febrero de 2020

### <a name="agents"></a>Agentes

Actualizaciones múltiples como parte de la reescritura del contenido de la extensión de diagnóstico.

- [Introducción a los agentes de supervisión de Azure](agents/agents-overview.md): reestructuración de las tablas para aclarar mejor las características únicas de cada agente.
- [Introducción a las extensiones de Azure Diagnostics](agents/diagnostics-extension-overview.md): se ha reescrito en su totalidad.
- [Uso de Blob Storage para IIS y Table Storage para eventos en Azure Monitor](essentials/diagnostics-extension-logs.md): reescritura general con fines de actualización y claridad.
- [Instalación y configuración de Windows Azure Diagnostics Extension (WAD)](agents/diagnostics-extension-windows-install.md): artículo nuevo. 
- [Esquema de Windows Diagnostics Extension](agents/diagnostics-extension-schema-windows.md): se ha reorganizado.
- [Envío de datos desde la extensión de Diagnósticos de Windows Azure a Azure Event Hubs](agents/diagnostics-extension-stream-event-hubs.md): se ha reescrito y actualizado por completo.
- [Almacenamiento y visualización de datos de diagnóstico en Azure Storage](../cloud-services/diagnostics-extension-to-storage.md): se ha reescrito y actualizado por completo.
- [Extensión de máquina virtual de Log Analytics para Windows](../virtual-machines/extensions/oms-windows.md): aclara la relación con el agente de Log Analytics.
- [Extensión de máquina virtual de Azure Monitor para Linux](../virtual-machines/extensions/oms-linux.md): aclara la relación con el agente de Log Analytics.

### <a name="application-insights"></a>Application Insights

- [Cadenas de conexión en Azure Application Insights](app/sdk-connection-string.md): artículo nuevo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones

#### <a name="container-insights"></a>Container Insights

- [Integración de Azure Active Directory con Azure Kubernetes Service](../aks/azure-ad-integration-cli.md): se ha agregado una nota para crear una aplicación cliente que admita clústeres habilitados para RBAC de Kubernetes con el fin de admitir Container Insights.

#### <a name="vm-insights"></a>VM Insights

- [Preguntas más frecuentes sobre VM Insights (GA)](vm/vminsights-ga-release-faq.md): cambio en el modo en que se almacenan los datos de rendimiento.

#### <a name="office-365"></a>Office 365

- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): fecha de desuso actualizada.


### <a name="logs"></a>Registros

- [Optimización de las consultas de registro en Azure Monitor](logs/query-optimization.md): artículo nuevo.
- [Administración del uso y los costos de los registros de Azure Monitor](logs/manage-cost-storage.md): mejora de las consultas de ejemplo para ayudarle a comprender el uso.

### <a name="metrics"></a>Métricas

- [Métricas de plataforma de Azure Monitor exportables a través de la configuración del diagnóstico](essentials/metrics-supported-export-diagnostic-settings.md): se ha agregado una sección sobre el cambio en el comportamiento de los valores NULL y cero.

### <a name="visualizations"></a>Visualizaciones

Varios artículos nuevos para la guía de conversión del diseñador de vistas a libros.

- [Guía de transición del diseñador de vistas a los libros de Azure Monitor](visualize/view-designer-conversion-overview.md): artículo nuevo.
- [Opciones de conversión del diseñador de vistas a los libros de Azure Monitor](visualize/view-designer-conversion-options.md): artículo nuevo.
- [Conversión de elementos del diseñador de vistas a los libros de Azure Monitor](visualize/view-designer-conversion-tiles.md): artículo nuevo.
- [Resumen y acceso a la conversión del diseñador de vistas a libros de Azure Monitor](visualize/view-designer-conversion-access.md): artículo nuevo.
- [Tareas comunes de la conversión del diseñador de vistas a libros de Azure Monitor](visualize/view-designer-conversion-tasks.md): artículo nuevo.
- [Ejemplos de conversión del diseñador de vistas a libros de Azure Monitor](visualize/view-designer-conversion-examples.md): artículo nuevo.

## <a name="january-2020"></a>Enero de 2020

### <a name="general"></a>General

- [¿Qué supervisa Azure Monitor?](monitor-reference.md) - Nuevo artículo.

### <a name="agents"></a>Agentes

- [Recopilación de datos de registro con el agente de Azure Log Analytics](agents/log-analytics-agent.md): se ha actualizado la tabla de requisitos del firewall de red.

### <a name="alerts"></a>Alertas

- [Creación y administración de grupos de acciones en Azure Portal](alerts/action-groups.md): se ha eliminado la configuración de las funciones v2 que ya no es necesaria.
- [Creación de una alerta de métrica con una plantilla de Resource Manager](alerts/alerts-metric-create-templates.md): se ha agregado un ejemplo para el parámetro *ignoreDataBefore*.  Se han agregado restricciones sobre las reglas de varios criterios.
- [Uso de la API REST de alertas de Log Analytics](alerts/api-alerts.md): se ha corregido el ejemplo de JSON.

### <a name="application-insights"></a>Application Insights

- [Direcciones IP que emplean Application Insights y Log Analytics](app/ip-addresses.md): se ha actualizado la sección de prueba de disponibilidad con la incorporación de una regla de puerto de entrada que permite el tráfico mediante grupos de seguridad de red de Azure.
- [Solución de problemas relacionados con Azure Application Insights Profiler](app/profiler-troubleshooting.md): se ha actualizado la solución de problemas general.
- [Muestreo de telemetría en Azure Application Insights](app/sampling.md): se ha actualizado y reestructurado para mejorar la legibilidad en función de los comentarios de los clientes.

### <a name="data-security"></a>Seguridad de los datos

- [Configuración de la clave administrada por el cliente de Azure Monitor](logs/customer-managed-keys.md): nuevo artículo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones

#### <a name="container-insights"></a>Container Insights

- [Configuración de la recopilación de datos del agente de Container Insights](containers/container-insights-agent-config.md): se han agregado detalles para la actualización del agente en Red Hat OpenShift en Azure e información adicional para distinguir los métodos de actualización del agente.
- [Creación de alertas de rendimiento para Container Insights](./containers/container-insights-log-alerts.md): se ha revisado la información y se han actualizado los pasos para crear una alerta sobre los datos de rendimiento almacenados en el área de trabajo mediante alertas de contexto del área de trabajo.
- [Supervisión de Kubernetes con Container Insights](containers/container-insights-analyze.md): se ha actualizado el artículo introductorio y el de análisis relacionado con la compatibilidad de los clústeres de Windows Kubernetes.
- [Configuración de clústeres de Red Hat OpenShift en Azure con Container Insights](containers/container-insights-azure-redhat-setup.md): se han agregado detalles para la actualización del agente en Red Hat OpenShift en Azure e información adicional para distinguir los métodos de actualización del agente.
- [Configuración de los clústeres híbridos de Kubernetes con Container Insights](containers/container-insights-hybrid-setup.md): se ha actualizado para reflejar la compatibilidad agregada para el puerto seguro 10250 con Kubelet's cAdvisor.
- [Administración del agente de Container Insights](containers/container-insights-manage-agent.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la extracción de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.
- [Configuración de la integración de Prometheus con Container Insights](containers/container-insights-prometheus-integration.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la extracción de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.
- [Actualización de Container Insights para las métricas](containers/container-insights-update-metrics.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la extracción de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.

#### <a name="vm-insights"></a>VM Insights

- [Preguntas más frecuentes sobre VM Insights (GA)](vm/vminsights-ga-release-faq.md): se ha agregado información sobre la actualización del área de trabajo y los agentes a la nueva versión.

#### <a name="office-365"></a>Office 365

- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): se han agregado detalles y preguntas frecuentes sobre la migración a una solución de Office 365 en Azure Sentinel. Se ha eliminado la sección de incorporación.

### <a name="logs"></a>Registros

- [Administración de áreas de trabajo de Log Analytics en Azure Monitor](logs/manage-access.md): se ha actualizado la sección de "No acciones".
- [Administración del uso y los costos de los registros de Azure Monitor](logs/manage-cost-storage.md): se ha agregado claridad al cálculo del volumen de datos en la sección Modelo de precios.
- [Uso de las plantillas de Azure Resource Manager para crear y configurar un área de trabajo de Log Analytics](./logs/resource-manager-workspace.md): se ha actualizado la plantilla con nuevos planes de tarifa.

### <a name="platform-logs"></a>Registros de plataforma

- [Recopilación del registro de actividad de Azure con configuración de diagnóstico: Azure Monitor](./essentials/activity-log.md): se ha agregado información adicional sobre las propiedades modificadas.
- [Exportación del registro de actividad de Azure](./essentials/activity-log.md#legacy-collection-methods): se ha actualizado con los cambios de la interfaz de usuario. 

## <a name="december-2019"></a>Diciembre de 2019

### <a name="agents"></a>Agentes

- [Conexión de equipos Linux a Azure Monitor](agents/agent-linux.md): nuevo artículo.

### <a name="alerts"></a>Alertas

- [Creación de una alerta de métrica con una plantilla de Resource Manager](alerts/alerts-metric-create-templates.md): se ha agregado un ejemplo para métricas personalizadas.
- [Creación de alertas con umbrales dinámicos en Azure Monitor](alerts/alerts-dynamic-thresholds.md): se ha agregado una sección sobre la interpretación de gráficos de umbrales dinámicos.
- [Información general de alertas y supervisión de notificaciones en Azure](alerts/alerts-overview.md): se ha actualizado la consulta de Resource Graph.
- [Recursos compatibles para las alertas de métricas de Azure Monitor](alerts/alerts-metric-near-real-time.md): actualización sobre las métricas y dimensiones admitidas.
- [Cambio de la API heredada de alertas de Log Analytics a la nueva API de Alertas de Azure](alerts/alerts-log-api-switch.md): se ha agregado una nota sobre el nombre de alerta modificado.
- [Descripción del funcionamiento de las alertas de métricas en Azure Monitor.](alerts/alerts-metric-overview.md) se han agregado los tipos de recursos admitidos para la supervisión a escala.

### <a name="application-insights"></a>Application Insights

- [Application Insights para las aplicaciones de servicio de trabajo (aplicaciones sin HTTP)](app/worker-service.md): se ha agregado un nivel de registro predeterminado al código de C#. Se ha actualizado la versión de referencia del paquete.
- [Referencia de ApplicationInsights.config: Azure](app/configuration-with-applicationinsights-config.md): se ha actualizado el código de ejemplo.
- [Automatización de Azure Application Insights con PowerShell](app/powershell.md): se ha actualizado la plantilla de Resource Manager.
- [Creación de un recurso de Azure Application Insights](app/create-new-resource.md): se ha agregado una nota al nombre único global.
- [Diagnóstico con Live Metrics Stream: Azure Application Insights](app/live-stream.md): se ha actualizado el requisito de versión del SDK de ASP.NET Core.
- [Contadores de eventos en Application Insights](app/eventcounters.md): se ha actualizado la categoría y la tabla de customMetrics.
- [Exploración de registros de seguimiento de Java en Azure Application Insights](app/java-trace-logs.md): se ha agregado la configuración del umbral de registro del agente de Java.
- [Direcciones IP que emplean Application Insights y Log Analytics](app/ip-addresses.md): se han actualizado las direcciones IP para Live Metrics Stream.
- [Supervisar el rendimiento de Azure App Services](app/azure-web-apps.md): se ha agregado compatibilidad con ASP.NET Core 3.0. 
- [Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar)](app/opencensus-python.md): se ha agregado una aclaración para la asignación de esquemas de Python para OpenCensus en Azure. Esquema de Monitor
- [Notas de la versión de Azure Application Insights](app/release-notes.md): se han agregado notas para las versiones más antiguas.
- [Canales de telemetría en Azure Application Insights](app/telemetry-channels.md): se ha actualizado la duración de los datos descartados durante el período ampliado de la conexión perdida.
- [Muestreo de telemetría en Azure Application Insights](app/sampling.md): se ha corregido un fragmento de código del elemento TelemetryInitializer.
- [Solución de problemas de Application Insights en un proyecto web de Java](app/java-troubleshoot.md): se ha eliminado una instrucción acerca de la no admisión de la recopilación de dependencias en JDK 9.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones

- [Preguntas más frecuentes sobre Container Insights](./faq.md): se ha agregado una pregunta sobre los campos Imagen y Nombre.
- [Solución Azure SQL Analytics en Azure Monitor](insights/azure-sql.md): se han actualizado los tiempos de espera de la base de datos para la compatibilidad con Instancia administrada.
- [Configuración de la recopilación de datos del agente de Container Insights](containers/container-insights-agent-config.md): se ha agregado la configuración de enrich_container_logs.
- [Configuración de los clústeres híbridos de Kubernetes con Container Insights](containers/container-insights-hybrid-setup.md): se ha agregado la sección de solución de problemas.
- [Supervisión de Active Directory Replication Status con Azure Monitor](insights/ad-replication-status.md): se ha actualizado el requisito previo de .NET Framework.
- [Solución Network Performance Monitor en Azure](insights/network-performance-monitor.md): se han agregado regiones compatibles.
- [Optimización del entorno de Active Directory con Azure Monitor](insights/ad-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Optimización del entorno de SQL Server con Azure Monitor](insights/sql-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Optimización del entorno de System Center Operations Manager con Azure Log Analytics](insights/scom-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Conexiones compatibles con IT Service Management Connector en Azure Log Analytics](alerts/itsmc-connections.md): se ha agregado "New York" al requisito previo de id. de cliente y secreto de cliente.

### <a name="logs"></a>Registros

- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](logs/delete-workspace.md): se ha agregado un método con PowerShell.
- [Diseño de la implementación de registros de Azure Monitor](logs/design-logs-deployment.md): se ha aumentado la velocidad de ingesta de un área de trabajo.

### <a name="metrics"></a>Métricas

- [Métricas de plataforma de Azure Monitor que pueden exportarse con la configuración de diagnóstico](essentials/metrics-supported-export-diagnostic-settings.md): nuevo artículo.

### <a name="platform-logs"></a>Registros de plataforma

Se han actualizado varios artículos como parte de la reestructuración de contenido de los registros de la plataforma en función de la nueva característica para configurar el registro de actividad mediante la configuración de diagnóstico.

- [Archivado de registros de recursos de Azure en la cuenta de almacenamiento](./essentials/resource-logs.md#send-to-azure-storage)
- [Esquema de eventos del registro de actividad de Azure](essentials/activity-log-schema.md)
- [Límites de servicio de Azure Monitor](service-limits.md)
- [Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics](./essentials/activity-log.md)
- [Recopilación del registro de actividad de Azure con configuración de diagnóstico (versión preliminar): Azure Monitor](./essentials/activity-log.md)
- [Recopilación de registros de actividad de Azure en un área de trabajo de Log Analytics entre inquilinos de Azure](./essentials/activity-log.md)
- [Recopilación de registros de recursos de Azure en el área de trabajo de Log Analytics](./essentials/resource-logs.md#send-to-log-analytics-workspace)
- [Creación de la configuración de diagnóstico en Azure con una plantilla de Resource Manager](./essentials/resource-manager-diagnostic-settings.md)
- [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](essentials/diagnostic-settings.md)
- [Exportación del registro de actividad de Azure](./essentials/activity-log.md#legacy-collection-methods)
- [Introducción a los registros de plataforma Azure](essentials/platform-logs-overview.md)
- [Transmisión de datos de supervisión de Azure a un centro de eventos](essentials/stream-monitoring-data-event-hubs.md)
- [Transmisión de registros de plataforma de Azure a un centro de eventos](./essentials/resource-logs.md#send-to-azure-event-hubs)

### <a name="quickstarts-and-tutorials"></a>Guías de inicio rápido y tutoriales

- [Creación de un gráfico de métricas en Azure Monitor](essentials/tutorial-metrics-explorer.md): nuevo artículo.
- [Recopilación de registros de recurso de un recurso de Azure y análisis con Azure Monitor](essentials/tutorial-resource-logs.md): nuevo artículo.
- [Supervisión de recursos de Azure con Azure Monitor](essentials/quick-monitor-azure-resource.md): nuevo artículo.
   
## <a name="next-steps"></a>Pasos siguientes

- Para colaborar en la documentación de Azure Monitor, consulte la [Guía para colaboradores de Microsoft Docs](/contribute/).