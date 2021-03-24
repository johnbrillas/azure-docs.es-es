---
title: Base de referencia de seguridad de Azure para Azure Functions
description: La base de referencia de seguridad de Azure Functions proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: azure-functions
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 4fd53067309f83b284da25040f9f6534936cead9
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101704673"
---
# <a name="azure-security-baseline-for-azure-functions"></a>Base de referencia de seguridad de Azure para Azure Functions

Esta línea de base de seguridad aplica las instrucciones de [Información general sobre Azure Security Benchmark (v1)](../security/benchmarks/overview-v1.md) a Azure Functions. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices aplicables a Azure Functions. Se han excluido los **controles** no aplicables a Azure Functions.

 
Para ver cómo Azure Functions se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Azure Functions completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: integre las aplicaciones de Azure Functions con una red virtual de Azure. Las aplicaciones de Functions que se ejecutan en el plan Premium tienen las mismas funcionalidades de hospedaje que las aplicaciones web de Azure App Service, entre las que se incluye la característica "Integración con red virtual".  Las redes virtuales de Azure le permiten colocar cualquier recurso de Azure, como Azure Functions, en una red enrutable que no sea Internet.

- [Integración de Functions con una red virtual de Azure](functions-create-vnet.md)

- [Descripción de la característica Integración con red virtual de Azure Functions y Azure App Service](../app-service/web-sites-integrate-with-vnet.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy (Microsoft.Web)** :

[!INCLUDE [Resource Policy for Microsoft.Web 1.1](../../includes/policy/standards/asb/rp-controls/microsoft.web-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: utilice Azure Security Center y siga las recomendaciones de protección de redes para ayudar a salvaguardar las configuraciones y recursos de red relacionados con las aplicaciones de Azure Functions.

Si usa grupos de seguridad de red con su implementación de Azure Functions, habilite los registros de flujo en estos grupos y envíe dichos registros a una cuenta de Azure Storage para auditar el tráfico. También puede enviar registros de flujo de los grupos de seguridad de red a un área de trabajo de Log Analytics y usar Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: Para proteger totalmente los puntos de conexión de Azure Functions en producción, considere la posibilidad de implementar una de las siguientes opciones de seguridad en el nivel de aplicación de funciones:

- Active la autenticación o autorización de App Service en la aplicación de funciones.

- Uso de Azure API Management (APIM) para autenticar solicitudes

- implemente la aplicación de Functions en una instancia de Azure App Service Environment.

Además, debe asegurarse de que la depuración remota esté deshabilitada en la instancia de producción de Azure Functions. Asimismo, el uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación de funciones en Azure. Permita la interacción con la aplicación de funciones solo de los dominios requeridos.

Considere la posibilidad de implementar el firewall de aplicaciones web (WAF) de Azure durante la configuración de las redes para realizar una inspección adicional del tráfico entrante. Habilite la configuración de diagnóstico para WAF e ingiera los registros en una cuenta de almacenamiento, un centro de eventos o un área de trabajo de análisis de registros. 

- [Protección de los puntos de conexión de Azure Functions en producción](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#secure-an-http-endpoint-in-production)

- [Implementación de WAF de Azure](../web-application-firewall/ag/create-waf-policy-ag.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy (Microsoft.Web)** :

[!INCLUDE [Resource Policy for Microsoft.Web 1.3](../../includes/policy/standards/asb/rp-controls/microsoft.web-1-3.md)]

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: habilite el estándar DDoS Protection en las redes virtuales asociadas a las aplicaciones de funciones para protegerse de los ataques de denegación de servicio distribuido (DDoS). Use las características de la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP públicas malintencionadas conocidas o no utilizadas.

Asimismo, configure una puerta de enlace front-end, como el firewall de aplicaciones web de Azure, para autenticar todas las solicitudes entrantes y filtrar el tráfico malintencionado. Azure Web Application Firewall puede ayudar a proteger la aplicación de funciones, ya que inspecciona el tráfico web entrante para bloquear las inyecciones de código SQL, los scripts de sitios, las cargas de malware y los ataques de denegación de servicio distribuido. Para incorporar un firewall de aplicaciones web requiere una instancia de App Service Environment o el uso de puntos de conexión privados. Puede usar puntos de conexión privados para las funciones hospedadas en los planes Prémium y de App Service.

- [Opciones de redes de Azure Functions](functions-networking-options.md)

- [Configuración de la protección contra DDoS](../ddos-protection/manage-ddos-protection.md)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](../security-center/azure-defender.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: si usa un grupo de seguridad de red con su implementación de Azure Functions, habilite los registros de flujo en estos grupos y envíe dichos registros a una cuenta de almacenamiento para auditar el tráfico. También puede enviar los registros de flujo a un área de trabajo de Log Analytics y usar Análisis de tráfico para obtener conclusiones sobre el flujo del tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: Configure una puerta de enlace front-end como firewall de aplicaciones web de Azure para autenticar todas las solicitudes entrantes y filtrar el tráfico malintencionado. Los firewalls de aplicaciones web de Azure ayudan a proteger las aplicaciones de Functions, ya que inspeccionan el tráfico web entrante para bloquear las inyecciones de código SQL, los scripts de sitios, las cargas de malware y los ataques de denegación de servicio distribuido. Para incorporar un firewall de aplicaciones web requiere una instancia de App Service Environment o el uso de puntos de conexión privados. Puede usar puntos de conexión privados para las funciones hospedadas en los planes Prémium y de App Service.

Como alternativa, hay varias opciones en marketplace, como el firewall de aplicaciones web de Barracuda para Azure, que están disponibles en Azure Marketplace que incluyen las características de prevención o detección de intrusiones.

- [Opciones de redes de Azure Functions](functions-networking-options.md)

- [Plan Premium de Azure Functions](functions-premium-plan.md)

- [Introducción a App Service Environment](../app-service/environment/intro.md)

- [Consideraciones de red para un entorno de App Service Environment](../app-service/environment/network-info.md) 

- [Descripción del firewall de aplicaciones web de Azure](../web-application-firewall/overview.md)

- [Uso de puntos de conexión privados con Azure Functions](../app-service/networking/private-endpoint.md)

- [Descripción del servicio en la nube Barracuda WAF](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-web-application-firewall#configuring-your-barracuda-waf-cloud-service)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Instrucciones**: Configure una puerta de enlace front-end para la red, como el firewall de aplicaciones web de Azure con cifrado TLS de un extremo a otro. Para incorporar un firewall de aplicaciones web requiere una instancia de App Service Environment o el uso de puntos de conexión privados. Puede usar puntos de conexión privados para las funciones hospedadas en los planes Prémium y de App Service.

- [Opciones de redes de Azure Functions](functions-networking-options.md)

- [Plan Premium de Azure Functions](functions-premium-plan.md)

- [Introducción a App Service Environment](../app-service/environment/intro.md)

- [Consideraciones de red para un entorno de App Service Environment](../app-service/environment/network-info.md) 

- [Descripción del firewall de aplicaciones web de Azure](../web-application-firewall/overview.md)

- [Configuración de TLS de un extremo a otro con Application Gateway mediante el portal](../application-gateway/end-to-end-ssl-portal.md)

- [Uso de puntos de conexión privados con Azure Functions](../app-service/networking/private-endpoint.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de red virtual para definir controles de acceso a la red en el grupo de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, AzureAppService) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico en el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Más información sobre el uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de red relacionados con Azure Functions. Utilice alias de Azure Policy en los espacios de nombres "Microsoft.Web" y "Microsoft.Network" para crear directivas personalizadas que permitan auditar o aplicar la configuración de red de Azure Functions. También puede usar definiciones de directiva integradas en Azure Functions, como:

- CORS no debe permitir que todos los recursos accedan a sus aplicaciones de funciones.

- A la aplicación de funciones solo se puede acceder a través de HTTPS.

- En la aplicación de funciones se debe usar la versión de TLS más reciente.

También puede utilizar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como las plantillas de Azure Resource Manager, el control de acceso basado en roles de Azure (Azure RBAC) y las directivas, en una única definición de un plano técnico. Aplique fácilmente el plano técnico a nuevas suscripciones y entornos, y ajuste el control y la administración mediante el control de versiones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: si usa un grupo de seguridad de red con su implementación de Azure Functions, use etiquetas para estos NSG y otros recursos relacionados con la seguridad de red y el flujo del tráfico. Para las reglas del grupo de seguridad de red, use el campo "Descripción" para especificar la duración o la necesidad del negocio y otra información de cualquier regla que permita el tráfico hacia o desde una red.

Use cualquiera de las definiciones de directiva de Azure integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor" para asegurarse de que todos los recursos se crean con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en las opciones y recursos de red relacionados con las implementaciones de Azure Functions. Cree alertas en Azure Monitor que se desencadenen cuando se produzcan cambios en la configuraciones o recursos de red críticos. 

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Para el registro de la auditoria del plano de control, habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para los recursos de Azure.

Azure Functions también puede integrarse directamente con Azure Application Insights para supervisar las funciones. Application Insights recopila información del registro, el rendimiento y los errores. Detecta automáticamente anomalías en el rendimiento e incluye herramientas de análisis eficaces que ayudan a diagnosticar problemas y comprender cómo se usan las funciones.

Si tiene un registro de auditoría o seguridad personalizado integrado en la aplicación de funciones, habilite la opción de diagnóstico "FunctionAppLogs" y envíe los registros a un área de trabajo de Log Analytics, a un centro de eventos de Azure o a una cuenta de almacenamiento de Azure para archivarlos. 

Como opción, puede habilitar e incorporar datos en Azure Sentinel o en una solución de administración de eventos e información de un sistema de terceros. 

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/activity-log)

- [Configuración de Azure Functions con Azure Application Insights](functions-monitoring.md)

- [Activación de la configuración de diagnóstico (registros generados por el usuario) en Azure Functions](functions-monitor-log-analytics.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Para el registro de la auditoria del plano de control, habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para los recursos de Azure.

Si tiene un registro de auditoría o seguridad personalizado integrado en la aplicación de funciones, habilite la opción de diagnóstico "FunctionAppLogs" y envíe los registros a un área de trabajo de Log Analytics, a un centro de eventos de Azure o a una cuenta de almacenamiento de Azure para archivarlos. 

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/activity-log)

- [Activación de la configuración de diagnóstico (registros generados por el usuario) en Azure Functions](functions-monitor-log-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy (Microsoft.Web)** :

[!INCLUDE [Resource Policy for Microsoft.Web 2.3](../../includes/policy/standards/asb/rp-controls/microsoft.web-2-3.md)]

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: no aplicable; esta guía está pensada para recursos de proceso de IaaS.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: En Azure Monitor, establezca el período de retención de registros en las áreas de trabajo de Log Analytics vinculadas a las aplicaciones de funciones atendiendo a las normativas de cumplimiento de su organización.

- [Establecimiento de parámetros de retención de registros](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure, así como la configuración de diagnóstico de la aplicación de funciones, y envíe los registros a un área de trabajo de Log Analytics. Realice consultas en Log Analytics para buscar términos, identificar tendencias, analizar patrones y proporcionar otra información detallada basada en los datos recopilados.

Habilite Application Insights para que las aplicaciones de funciones puedan recopilar datos de registro, rendimiento y error. Puede ver los datos de telemetría recopilados por Application Insights en Azure Portal.

Si tiene un registro de auditoría o seguridad personalizado integrado en la aplicación de funciones, habilite la opción de diagnóstico "FunctionAppLogs" y envíe los registros a un área de trabajo de Log Analytics, a un centro de eventos de Azure o a una cuenta de almacenamiento de Azure para archivarlos. 

Como opción, puede habilitar e incorporar datos en Azure Sentinel o en una solución de administración de eventos e información de un sistema de terceros.

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/activity-log)

- [Activación de la configuración de diagnóstico en Azure Functions](functions-monitor-log-analytics.md)

- [Configuración de Azure Functions con Azure Application Insights y visualización de los datos de telemetría](functions-monitoring.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure, así como la configuración de diagnóstico de la aplicación de funciones, y envíe los registros a un área de trabajo de Log Analytics. Realice consultas en Log Analytics para buscar términos, identificar tendencias, analizar patrones y proporcionar otra información detallada basada en los datos recopilados. Puede crear alertas basadas en las consultas del área de trabajo de Log Analytics.

Habilite Application Insights para que las aplicaciones de funciones puedan recopilar datos de registro, rendimiento y error. Puede ver los datos de telemetría recopilados por Application Insights y crear alertas en Azure Portal.

Como opción, puede habilitar e incorporar datos en Azure Sentinel o en una solución de administración de eventos e información de un sistema de terceros.

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/activity-log)

- [Activación de la configuración de diagnóstico en Azure Functions](functions-monitor-log-analytics.md)

- [Activación de Application Insights para Azure Functions](https://docs.microsoft.com/azure/azure-functions/configure-monitoring#enable-application-insights-integration)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Azure Active Directory (Azure AD) tiene roles integrados que se deben asignar explícitamente y son consultables. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc para detectar cuentas que son miembros de grupos administrativos.

- [Obtención de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: el acceso del plano de control a Azure Functions se controla mediante Azure Active Directory (Azure AD). Azure AD no tiene el concepto de contraseñas predeterminadas.

El acceso al plano de datos se puede controlar a través de diversos medios, como las claves de autorización, las restricciones de red y la validación de una identidad de Azure AD. Las claves de autorización las utilizan los clientes que se conectan a los puntos de conexión HTTP de Azure Functions y se pueden volver a generar en cualquier momento. Estas claves se generan de forma predeterminada para los nuevos puntos de conexión HTTP.

Hay diversos métodos de implementación disponibles para las aplicaciones de Functions, algunos de los cuales pueden hacer uso de un conjunto de credenciales generadas. Revise los métodos de implementación que se usarán con la aplicación.

- [Protección de un punto de conexión HTTP](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#secure-an-http-endpoint-in-production)

- [Obtención y regeneración de claves de autorización](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#obtaining-keys)

- [Tecnologías de implementación en Azure Functions](functions-deployment-technologies.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarle a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure Policy, por ejemplo:

- Debe haber más de un propietario asignado a su suscripción

- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción

- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

Información adicional disponible en los vínculos indicados.

- [Procedimiento para usar Azure Security Center para supervisar la identidad y el acceso](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: siempre que sea posible, use el SSO de Azure Active Directory (Azure AD) en lugar de configurar diferentes credenciales individuales en la aplicación de funciones para acceder a los datos. Use las recomendaciones de administración de identidades y acceso de Azure Security Center. Implemente el inicio de sesión único en las aplicaciones de funciones utilizando la característica de autenticación y autorización de App Service.

- [Descripción de la autenticación y la autorización en Azure Functions](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization#identity-providers)

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: utilice estaciones de trabajo de acceso con privilegios (PAW) que tengan configurada la autenticación multifactor para iniciar sesión en los recursos de Azure y configurarlos. 

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: use Privileged Identity Management (PIM) de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzca una actividad sospechosa o insegura en el entorno.

Además, use las detecciones de riesgo de Azure AD para ver alertas e informes sobre el comportamiento de los usuarios de riesgo.

- [Cómo implementar Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Información sobre las detecciones de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: El cliente debe usar ubicaciones con nombre de acceso condicional para permitir el acceso a Azure Portal solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización de las aplicaciones de funciones. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Configuración de la aplicación de funciones para usar el inicio de sesión de Azure AD](../app-service/configure-authentication-provider-aad.md)

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Active Directory (Azure AD) proporciona registros para ayudarle a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring/)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización de las aplicaciones de funciones. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

Tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión, auditoría y riesgo de Azure AD, que permiten la integración en Azure Sentinel o SIEM de terceros.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas de registro deseadas en Log Analytics.

- [Configuración de la aplicación de funciones para usar el inicio de sesión de Azure AD](../app-service/configure-authentication-provider-aad.md)

- [Integración de los registros de actividad de Azure en Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización de las aplicaciones de funciones. Para la desviación de comportamiento de inicio de sesión de cuenta en el plan de control (Azure Portal), use las características de detección de riesgos y de Azure AD Identity Protection para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Las aplicaciones de funciones deben estar separadas en cada red virtual o subred, y estar debidamente etiquetadas.

También se puede usar puntos de conexión privados para realizar el aislamiento de red. Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio (por ejemplo, el punto de conexión HTTP de una aplicación de funciones) con la tecnología de Azure Private Link. El punto de conexión privado usa una dirección IP privada de la red virtual, colocando el servicio de manera eficaz en su red virtual. Puede usar puntos de conexión privados para las funciones hospedadas en los planes Prémium y de App Service.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Opciones de redes de Azure Functions](functions-networking-options.md)

- [Plan Premium de Azure Functions](functions-premium-plan.md)

- [Descripción de puntos de conexión privados](../private-link/private-endpoint-overview.md)

- [Uso de puntos de conexión privados con Azure Functions](../app-service/networking/private-endpoint.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: En la instancia de Azure Portal de las aplicaciones de funciones, en "Características de la plataforma:  Redes: SSL ", habilite la opción"Solo HTTPS" y establezca la versión mínima de TLS en 1.2.

- [Requerir HTTPS en las aplicaciones de funciones](https://docs.microsoft.com/azure/azure-functions/security-concepts#require-https)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy (Microsoft.Web)** :

[!INCLUDE [Resource Policy for Microsoft.Web 4.4](../../includes/policy/standards/asb/rp-controls/microsoft.web-4-4.md)]

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Instrucciones**: no está disponible en estos momentos; las características de identificación, clasificación y prevención de pérdida de datos no están disponibles actualmente en Azure Functions. Etiquete las aplicaciones de Functions que podrían procesar información confidencial e implemente una solución de terceros, si es necesario por motivos de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: utilice el control de acceso basado en roles de Azure (Azure RBAC) para administrar el acceso al plano de control de la aplicación de funciones (Azure Portal).

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: no aplicable; esta recomendación está diseñada para recursos de proceso de IaaS.

Microsoft administra la infraestructura subyacente de Azure Functions y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Al crear una aplicación de funciones, debe crear o vincular una cuenta de Azure Storage de uso general compatible con Blob, Queue y Table Storage. El motivo es que Azure Functions usa Azure Storage con ciertas operaciones; por ejemplo, para administrar los desencadenadores y ejecutar las funciones de registro. Azure Storage cifra todos los datos de las cuentas de almacenamiento en reposo. De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente que pueden usarse para cifrar datos de archivos y blobs. Estas claves deben estar presentes en Azure Key Vault para que la aplicación de Azure Functions pueda acceder a la cuenta de almacenamiento.

- [Consideraciones de almacenamiento de Azure Functions](storage-considerations.md)

- [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas cuando se produzcan cambios en las aplicaciones de producción de las aplicaciones de funciones y en otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Adopte un enfoque DevSecOps (desarrollo, seguridad y operaciones) para garantizar que las aplicaciones de funciones son seguras y que se van a mantener lo más protegidas posible durante su ciclo de vida útil. El enfoque DevSecOps involucra al equipo de seguridad de la organización y sus funcionalidades en las prácticas de DevOps, lo que convierte la seguridad en una responsabilidad de todos los usuarios del equipo.

Siga también las recomendaciones de Azure Security Center para ayudar a proteger las aplicaciones de funciones.

- [Incorporación de un mecanismo continuo para validar la seguridad en la canalización de CI/CD](https://docs.microsoft.com/azure/devops/migrate/security-validation-cicd-pipeline?view=azure-devops&amp;preserve-view=true)

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía**: Microsoft se encarga de administrar las vulnerabilidades en los sistemas subyacentes que admiten Azure Functions. Sin embargo, puede utilizar la gravedad de las recomendaciones de Azure Security Center y la puntuación de seguridad para medir el riesgo de su entorno. La puntuación segura se basa en el número de recomendaciones de Security Center que ha mitigado. Para dar prioridad a las recomendaciones que deben resolverse en primer lugar, tenga en cuenta la gravedad de cada una.

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) dentro de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Además, use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Información adicional disponible en los vínculos indicados.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Defina los recursos de Azure y el software aprobados para los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones. 

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Información adicional disponible en los vínculos indicados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Configure el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación Microsoft Azure Management.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Defina e implemente configuraciones de seguridad estándar para su aplicación defunciones con Azure Policy. Utilice los alias de Azure Policy en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar o aplicar la configuración de las aplicaciones de funciones. También puede utilizar definiciones de directivas integradas como:

- Se debe usar una identidad administrada en la aplicación de funciones

- Se debe desactivar la depuración remota en las aplicaciones de funciones.

- A la aplicación de funciones solo se puede acceder a través de HTTPS.

Información adicional disponible en los vínculos indicados.

- [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Instrucciones**: Use la directiva de Azure [denegar] e [implementar si no existe] para aplicar la configuración segura en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Instrucciones**: almacene y administre plantillas de ARM y definiciones de directivas personalizadas de Azure de forma segura en el control de código fuente.

- [¿Qué es la infraestructura como código?](/azure/devops/learn/what-is-infrastructure-as-code)

- [Diseño de flujos de trabajo de directiva como código](../governance/policy/concepts/policy-as-code.md)

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Use la directiva de Azure [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**: Utilice identidades administradas con Azure Key Vault para simplificar y proteger la administración de secretos de las aplicaciones en la nube. Las identidades administradas permiten que la aplicación de funciones se autentique en cualquier servicio que admita la autenticación de Azure Active Directory (Azure AD), como Key Vault, sin necesidad de usar credenciales en el código.

- [Creación de un almacén de claves](../key-vault/secrets/quick-create-portal.md)

- [Cómo usar identidades administradas para App Service y Azure Functions](../app-service/overview-managed-identity.md)

- [Autenticación en Azure Key Vault](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

- [Uso de referencias de Key Vault para App Service y Azure Functions](../app-service/app-service-key-vault-references.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: use identidades administradas para proporcionar automáticamente a su aplicación de funciones una identidad administrada en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluyendo Key Vault, sin necesidad de credenciales en el código.

- [Cómo usar identidades administradas para App Service y Azure Functions](../app-service/overview-managed-identity.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy (Microsoft.Web)** :

[!INCLUDE [Resource Policy for Microsoft.Web 7.12](../../includes/policy/standards/asb/rp-controls/microsoft.web-7-12.md)]

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: utilice la característica Copias de seguridad y restauración para programar copias de seguridad periódicas de la aplicación. Las aplicaciones de Functions que se ejecutan en el plan Premium tienen las mismas funcionalidades de hospedaje que las aplicaciones web de Azure App Service, como la característica de copia de seguridad y restauración.

También puede usar una solución de control de código fuente como Azure Repos y Azure DevOps para almacenar y administrar el código de forma segura. Azure DevOps Services utiliza muchas de las características de almacenamiento de Azure para garantizar la disponibilidad de los datos en caso de un error de hardware, una interrupción del servicio o un desastre en la región. Además, el equipo de Azure DevOps sigue los procedimientos para proteger los datos contra la eliminación accidental o malintencionada.

- [Realizar una copia de seguridad de la aplicación en Azure](../app-service/manage-backup.md)

- [Descripción de la disponibilidad de los datos en Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability&amp;preserve-view=true)

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copias de seguridad de las claves administradas por el cliente

**Guía**: utilice la característica Copias de seguridad y restauración para programar copias de seguridad periódicas de la aplicación. Las aplicaciones de Functions que se ejecutan en el plan Premium tienen las mismas funcionalidades de hospedaje que las aplicaciones web de Azure App Service, como la característica de copia de seguridad y restauración. Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault.

También puede usar una solución de control de código fuente como Azure Repos y Azure DevOps para almacenar y administrar el código de forma segura. Azure DevOps Services utiliza muchas de las características de almacenamiento de Azure para garantizar la disponibilidad de los datos en caso de un error de hardware, una interrupción del servicio o un desastre en la región. Además, el equipo de Azure DevOps sigue los procedimientos para proteger los datos contra la eliminación accidental o malintencionada.

- [Realizar una copia de seguridad de la aplicación en Azure](../app-service/manage-backup.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Descripción de la disponibilidad de los datos en Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability&amp;preserve-view=true)

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: garantice la capacidad para restaurar los datos periódicamente desde la característica Copias de seguridad y restauración. Si usa otra ubicación sin conexión para realizar una copia de seguridad del código, no olvide realizar periódicamente restauraciones completas. Pruebe la restauración de la copia de seguridad de las claves administradas por el cliente.

- [Restauración de una aplicación de Azure desde una copia de seguridad](../app-service/web-sites-restore.md)

- [Restauración de una aplicación en Azure desde una instantánea](../app-service/app-service-web-restore-snapshots.md)

- [Restauración de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Las copias de seguridad de la característica Copias de seguridad y restauración usan una cuenta de Azure Storage de la suscripción. Azure Storage cifra todos los datos de las cuentas de almacenamiento en reposo. De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente para cifrar los datos de almacenamiento.

Si usa claves administradas por el cliente, asegúrese de que la eliminación temporal esté habilitada en Key Vault para evitar la eliminación accidental o malintencionada de las claves.

- [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md)

- [Habilitación de la eliminación temporal en Key Vault](../storage/blobs/soft-delete-blob-overview.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Guía del NIST para controlar incidentes de seguridad en equipos](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente.  Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: utilice la característica Automatización de flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas a las alertas y recomendaciones de seguridad con Logic Apps.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de pruebas de penetración de sitios activos y ataques simulados en la infraestructura en la nube, los servicios y las aplicaciones administrados por Microsoft.

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
