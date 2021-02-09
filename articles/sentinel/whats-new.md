---
title: Novedades de Azure Sentinel
description: En este artículo se describen las nuevas características de Azure Sentinel de los últimos meses.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 8154e1adff8d8c2bdfe7fedc9309f95e5c5880bd
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99499460"
---
# <a name="whats-new-in-azure-sentinel"></a>Novedades de Azure Sentinel

En este artículo se enumeran las características que se han agregado recientemente a Azure Sentinel y las nuevas características de servicios relacionados que proporcionan una experiencia de usuario mejorada en Azure Sentinel.

Para más información sobre las características anteriores, consulte los [blogs de Tech Community](https://techcommunity.microsoft.com/t5/azure-sentinel/bg-p/AzureSentinelBlog/label-name/What's%20New).

Actualmente, las características indicadas están en VERSIÓN PRELIMINAR. En la página [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) se incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en versión beta, versión preliminar o que todavía no se han publicado para su disponibilidad general.


> [!TIP]
> Nuestros equipos de búsqueda de amenazas en Microsoft aportan consultas, cuadernos de estrategias, libros y cuadernos a la [comunidad de Azure Sentinel](https://github.com/Azure/Azure-Sentinel), lo que incluye [consultas de búsqueda](https://github.com/Azure/Azure-Sentinel) concretas que sus equipos pueden adaptar y usar. 
>
> Usted también puede contribuir. Únase a nosotros en la [comunidad de GitHub de cazadores de amenazas de Azure Sentinel](https://github.com/Azure/Azure-Sentinel/wiki).
> 

## <a name="january-2021"></a>Enero de 2021

- [Módulo de PowerShell Az.SecurityInsights (versión preliminar pública)](#azsecurityinsights-powershell-module-public-preview)
- [Conector de SQL Database](#sql-database-connector)
- [Mejora en los comentarios de incidentes](#improved-incident-comments)
- [Clústeres de Log Analytics dedicados](#dedicated-log-analytics-clusters)
- [Identidades administradas de Logic Apps](#logic-apps-managed-identities)
- [Mejora en el ajuste de reglas con los grafos en versión preliminar de las reglas de análisis](#improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview)


### <a name="azsecurityinsights-powershell-module-public-preview"></a>Módulo de PowerShell Az.SecurityInsights (versión preliminar pública)

Azure Sentinel ya admite el nuevo módulo de PowerShell [Az.SecurityInsights](https://www.powershellgallery.com/packages/Az.SecurityInsights/).

El módulo **Az.SecurityInsights** admite casos de uso comunes de Azure Sentinel, como la interacción con incidentes para cambiar los estados, la gravedad, el propietario, etc., la incorporación de comentarios y etiquetas a incidentes, y la creación de marcadores.

Aunque se recomienda usar las plantillas de [Azure Resource Manager](/azure/azure-resource-manager/templates/) para la canalización de CI/CD, el módulo **Az.SecurityInsights** es útil para las tareas posteriores a la implementación y está específicamente destinado a la automatización de SOC.  Por ejemplo, la automatización de SOC puede incluir los pasos necesarios para configurar conectores de datos, crear reglas de análisis o agregar acciones de automatización a las reglas de análisis.

Para más información, incluida una lista completa de los cmdlets disponibles y una descripción de los mismos, las descripciones y ejemplos de parámetros, consulte la [documentación de Az.SecurityInsights de PowerShell](/powershell/module/az.securityinsights/).

### <a name="sql-database-connector"></a>Conector de SQL Database

Azure Sentinel ahora incluye el conector de Azure SQL Database, que permite transmitir en secuencias los registros de auditoría y diagnóstico de bases de datos a Azure Sentinel, así como supervisar continuamente la actividad en todas las instancias.

Azure SQL es un motor de base de datos de plataforma como servicio (PaaS) totalmente administrado que se encarga de la mayoría de las funciones de administración de bases de datos, como actualizar, aplicar revisiones, crear copias de seguridad y supervisar sin la intervención del usuario.

Para más información, consulte [Conexión de los registros de auditoría y diagnóstico de bases de datos de Azure SQL](connect-azure-sql-logs.md).

### <a name="improved-incident-comments"></a>Mejora en los comentarios de incidentes

Los analistas usan estos comentarios para colaborar en los incidentes, así como documentar procesos y pasos manualmente o como parte de un cuaderno de estrategias. 

La mejora en nuestra experiencia en comentarios de incidentes permite no solo dar formato a los comentarios, sino también editar o eliminar los comentarios existentes.

Para más información, consulte [Creación automática de incidentes a partir de alertas de seguridad de Microsoft](create-incidents-from-alerts.md).
### <a name="dedicated-log-analytics-clusters"></a>Clústeres de Log Analytics dedicados

Azure Sentinel ahora admite clústeres de Log Analytics dedicados como opción de implementación. Se recomienda considerar la posibilidad de usar un clúster dedicado si:

- **Ingiere más de 1 TB al día** en el área de trabajo de Azure Sentinel.
- **Tiene varias áreas de trabajo de Azure Sentinel** en una inscripción de Azure.

Los clústeres dedicados le permiten usar características como claves administradas por el cliente, caja de seguridad, cifrado doble y consultas entre áreas de trabajo rápidas cuando varias áreas de trabajo están en el mismo clúster.

Para más información, consulte [Clústeres dedicados de registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters).

### <a name="logic-apps-managed-identities"></a>Identidades administradas de Logic Apps

Azure Sentinel ahora admite identidades administradas para el conector de Logic Apps de Azure Sentinel, lo que permite conceder permisos directamente a un cuaderno de estrategias específico para que funcione en Azure Sentinel, en lugar de crear identidades adicionales.

- **Sin una identidad administrada**, el conector de Logic Apps requiere una identidad independiente con un rol RBAC de Azure Sentinel para ejecutarse en Azure Sentinel. Esta identidad independiente puede ser un usuario de Azure AD o una entidad de servicio, como una aplicación registrada de Azure AD.

- **Al activar la compatibilidad con identidades administradas en una aplicación lógica**, la aplicación lógica se registra en Azure AD y se proporciona un identificador de objeto. Use el identificador de objeto en Azure Sentinel para asignar la aplicación lógica con un rol RBAC de Azure en el área de trabajo de Azure Sentinel. 

Para más información, consulte:

- [Autenticación con Identidad administrada en Azure Logic Apps](/azure/logic-apps/create-managed-service-identity)
- [Documentación del conector de Logic Apps de Azure Sentinel](/connectors/azuresentinel) 

### <a name="improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview"></a>Mejora en el ajuste de reglas con los grafos de versión preliminar de reglas de análisis (versión preliminar pública)

Azure Sentinel le permite ahora ajustar mejor las reglas de análisis, lo que le ayuda a aumentar su precisión y reducir el ruido.

Después de editar una regla de análisis en la pestaña **Establecer la lógica de la regla**, busque el área **Results simulation** (Simulación de resultados) de la derecha. 

Seleccione **Test with current data** (Probar con datos actuales) para que Azure Sentinel ejecute una simulación de las últimas 50 ejecuciones de una regla de análisis. Se genera un grafo para mostrar el número promedio de alertas que habría generado la regla, en función de los datos de eventos sin procesar que se han evaluado. 

Para más información, consulte [Definición de la lógica de consulta de regla y configuración de los valores](tutorial-detect-threats-custom.md#define-the-rule-query-logic-and-configure-settings).

## <a name="december-2020"></a>Diciembre de 2020

- [80 nuevas consultas de búsqueda integradas](#80-new-built-in-hunting-queries)
- [Mejoras en el agente de Log Analytics](#log-analytics-agent-improvements)

### <a name="80-new-built-in-hunting-queries"></a>80 nuevas consultas de búsqueda integradas
 
Las consultas de búsqueda integradas de Azure Sentinel permiten a los analistas de SOC reducir las lagunas en la cobertura de detección actual y lograr nuevos clientes potenciales de búsqueda.

Esta actualización de Azure Sentinel incluye nuevas consultas de búsqueda que proporcionan cobertura en la matriz de la plataforma MITRE ATT&CK:

- **Colección**
- **Comando y control**
- **Acceso de credenciales**
- **Detección**
- **Ejecución**
- **Exfiltración**
- **Impacto**
- **Acceso inicial**
- **Persistencia**
- **Elevación de privilegios**

Las consultas de búsqueda agregadas están diseñadas para ayudarle a encontrar actividades sospechosas en su entorno. Aunque pueden devolver actividades legítimas y actividades potencialmente malintencionadas, pueden resultar útiles como guía para la búsqueda. 

Si, después de ejecutar estas consultas, confía en los resultados, puede que desee convertirlos en reglas de análisis o agregar resultados de búsqueda a los incidentes nuevos o existentes.

Todas las consultas agregadas están disponibles a través de la página de búsqueda de Azure Sentinel. Para más información, consulte [Búsqueda de amenazas con Azure Sentinel](hunting.md).

### <a name="log-analytics-agent-improvements"></a>Mejoras en el agente de Log Analytics

Los usuarios de Azure Sentinel se benefician de las siguientes mejoras en el agente de Log Analytics:

- **Compatibilidad con más sistemas operativos**, entre los que se incluyen CentOS 8, RedHat 8 y SUSE Linux 15.
- **Compatibilidad con Python 3**, además de con Python 2

Azure Sentinel usa el agente de Log Analytics para enviar eventos al área de trabajo, lo que incluye eventos de seguridad de Windows, los eventos de Syslog, los registros de CEF, etc.

> [!NOTE]
> Al agente de Log Analytics, a veces se le denomina Agente de OMS o Microsoft Monitoring Agent (MMA). 
> 

Para más información, consulte la [documentación de Log Analytics](/azure/azure-monitor/platform/log-analytics-agent) y las [notas de la versión del agente de Log Analytics](https://github.com/microsoft/OMS-Agent-for-Linux/releases).
## <a name="november-2020"></a>Noviembre de 2020

- [Supervisión de los cuadernos de estrategias de Logic Apps en Azure Sentinel](#monitor-your-logic-apps-playbooks-in-azure-sentinel)
- [Conector de Microsoft 365 Defender (versión preliminar pública)](#microsoft-365-defender-connector-public-preview)
### <a name="monitor-your-logic-apps-playbooks-in-azure-sentinel"></a>Supervisión de los cuadernos de estrategias de Logic Apps en Azure Sentinel

Azure Sentinel ahora se integra en [Azure Log Apps](/azure/logic-apps/), un servicio en la nube que le ayuda a programar, automatizar y organizar tareas, procesos empresariales y flujos de trabajo.

Use una instancia de Azure Logic Apps en Azure Sentinel como cuaderno de estrategias, que se puede invocar automáticamente al crear un incidente o al evaluar y usar incidentes. 

Para proporcionar información sobre el estado, el rendimiento y el uso de los cuadernos de estrategias, incluidos los que se agregan con Azure Logic Apps, se ha agregado un [libro de Azure](/azure/azure-monitor/platform/workbooks-overview) denominado **Playbooks health monitoring** (Supervisión de estado de cuadernos de estrategias). 

El libro **Playbooks health monitoring** (Supervisión de estado de cuadernos de estrategias) se usa para supervisar el estado de los cuadernos de estrategias o buscar anomalías en la cantidad de ejecuciones correctas o con errores. 

El libro **Playbooks health monitoring** (Supervisión de estado de cuadernos de estrategias) ya está disponible en la galería de plantillas de Azure Sentinel:

:::image type="content" source="media/whats-new/playbook-monitoring-workbook.gif" alt-text="Libro Playbooks health monitoring (Supervisión de estado de cuadernos de estrategias) de ejemplo":::

Para más información, consulte:

- [Documentación de Logic Apps](/azure/logic-apps/monitor-logic-apps-log-analytics#set-up-azure-monitor-logs)

- [Documentación sobre Azure Monitor](/azure/azure-monitor/platform/activity-log#send-to-log-analytics-workspace)

### <a name="microsoft-365-defender-connector-public-preview"></a>Conector de Microsoft 365 Defender (versión preliminar pública)
 
El conector de Microsoft 365 Defender para Azure Sentinel permite transmitir en secuencias registros de búsqueda avanzada (un tipo de datos de eventos sin procesar) desde Microsoft 365 Defender hasta Azure Sentinel. 

Con la integración de [Microsoft Defender para punto de conexión (MDATP)](/windows/security/threat-protection/) en el paraguas de seguridad de [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection), ahora puede recopilar los eventos de búsqueda avanzada de Microsoft Defender para punto de conexión mediante el conector Microsoft 365 Defender y transmitirlos en secuencia directamente a nuevas tablas creadas específicamente en el área de trabajo de Azure Sentinel. 

Las tablas de Azure Sentinel se basan en el mismo esquema que se usa en el portal de Microsoft 365 Defender y proporcionan acceso pleno al conjunto completo de registros de búsqueda avanzada. 

Para más información, consulte [Conexión de datos de Microsoft 365 Defender con Azure Sentinel](connect-microsoft-365-defender.md).

> [!NOTE]
> Microsoft 365 Defender se conocía antes como Microsoft Threat Protection o MTP. Microsoft Defender para Endpoint se conocía anteriormente como Protección contra amenazas avanzada de Microsoft Defender o MDATP.
> 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Incorporación de Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Obtención de visibilidad sobre las alertas](quickstart-get-visibility.md)
