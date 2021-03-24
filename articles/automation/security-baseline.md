---
title: Base de referencia de seguridad de Azure para Automation
description: La línea base de seguridad de Automation proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: automation
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 55440c3bec940e0cd5fd4c4d644801e7012b5e95
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101701490"
---
# <a name="azure-security-baseline-for-automation"></a>Base de referencia de seguridad de Azure para Automation

Esta línea base de seguridad aplica las instrucciones de [la versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md) a Azure Automation. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** que define Azure Security Benchmark y las directrices aplicables a Azure Automation relacionadas. Se han excluido los **controles** no aplicables a Azure Automation.

 
Para ver cómo Azure Automation se adapta por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea base de seguridad de Azure Automation](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: La cuenta de Azure Automation todavía no admite Azure Private Link para restringir el acceso al servicio a través de puntos de conexión privados. Los runbooks que se autentican y ejecutan en los recursos de Azure se ejecutan en un espacio aislado de Azure y aprovechan los recursos de back-end compartidos, de los cuales Microsoft es responsable de aislar entre sí; su red no está restringida y pueden acceder a recursos públicos. En la actualidad, Azure Automation no cuenta con la integración de red virtual para redes privadas más allá de la compatibilidad con Hybrid Runbook Worker. Este control no es aplicable si usa el servicio listo para usar sin Hybrid Runbook Worker.

Para obtener más aislamiento para los runbooks, puede usar instancias de Hybrid Runbook Worker en ejecución en Azure Virtual Machines. Cuando se crea una máquina virtual (VM) de Azure, es preciso crear una red virtual (VNet) o usar una red virtual existente y configurar las máquinas virtuales con una subred. Asegúrese de que todas las subredes implementadas tienen un grupo de seguridad de red aplicado con controles de acceso a la red específicos para los puertos y orígenes de confianza de sus aplicaciones. Para conocer los requisitos específicos de un servicio, consulte la recomendación de seguridad para ese servicio concreto. Como alternativa, si tiene un requisito específico, también puede usarse Azure Firewall para cumplirlo.

- [Redes virtuales y máquinas virtuales en Azure](../virtual-machines/network-overview.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación y configuración de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Entorno de ejecución de runbooks](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-execution-environment)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Instrucciones**: En la actualidad, Azure Automation no cuenta con la integración de red virtual para redes privadas más allá de la compatibilidad con Hybrid Runbook Worker. Este control no es aplicable si usa el servicio listo para usar sin Hybrid Runbook Worker.

Si usa instancias de Hybrid Runbook Worker respaldadas por Azure Virtual Machines, asegúrese de que la subred que contiene esos trabajadores tenga habilitado un grupo de seguridad de red (NSG) y configure los registros de flujo para desviar los registros a una cuenta de almacenamiento para la auditoría del tráfico. También puede desviar los registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

Aunque las reglas de grupo de seguridad de red y las rutas definidas por el usuario no se apliquen al punto de conexión privado, los registros de flujo de grupo de seguridad de red y la información de supervisión de las conexiones salientes siguen siendo compatibles y se pueden usar.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: En la actualidad, Azure Automation no cuenta con la integración de red virtual para redes privadas más allá de la compatibilidad con Hybrid Runbook Worker. Este control no es aplicable si usa el servicio listo para usar sin Hybrid Runbook Worker.

Si usa instancias de Hybrid Runbook Worker respaldadas por Azure Virtual Machines, habilite la protección estándar de denegación de servicio distribuido (DDoS) en las redes virtuales que hospedan las instancias de Hybrid Runbook Worker para protegerse frente a ataques DDoS. Use la inteligencia sobre amenazas integrada en Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas.  Configure Azure Firewall en cada uno de los segmentos de la red virtual, con la inteligencia sobre amenazas habilitada y configurada para **Alertar y denegar** el tráfico de red malintencionado.

Puede usar el acceso de red Just-in-Time de Azure Security Center para limitar la exposición de las máquinas virtuales de Windows a las direcciones IP aprobadas durante un período limitado.  Use también las recomendaciones de la Protección de red adaptable de Azure Security Center para las configuraciones de grupo de seguridad de red, a fin de limitar los puertos y las direcciones IP basadas en el tráfico real y la inteligencia sobre amenazas.

- [Configuración de la protección contra DDoS](../ddos-protection/manage-ddos-protection.md)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](../security-center/azure-defender.md)

- [Descripción de la protección de red adaptable de Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)

- [Descripción del control de acceso de red Just-in-Time de Azure Security Center](../security-center/security-center-just-in-time.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Azure Automation no cuenta actualmente con la integración de red virtual para redes privadas más allá de la compatibilidad con Hybrid Runbook Worker, este control no es aplicable si usa el servicio listo para usar sin instancias de Hybrid Worker.

Si usa instancias de Hybrid Runbook Worker respaldadas por Azure Virtual Machines, puede grabar los registros de flujo de grupo de seguridad de red en una cuenta de almacenamiento para generar registros de flujo de las instancias de Azure Virtual Machines que actúan como Runbook Worker. Al investigar actividades anómalas, puede habilitar la captura de paquetes de Network Watcher para que el tráfico de red pueda revisarse en busca de actividad inusual e inesperada.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: En la actualidad, Azure Automation no cuenta con la integración de red virtual para redes privadas más allá de la compatibilidad con Hybrid Runbook Worker. Este control no es aplicable si usa el servicio listo para usar sin Hybrid Runbook Worker.

Si usa instancias de Hybrid Runbook Worker hospedadas en Azure Virtual Machines, puede combinar las capturas de paquetes proporcionadas por Network Watcher y abrir las herramientas IDS de código abierto para realizar la detección de intrusiones de red de una amplia gama de amenazas en esas máquinas de trabajador. Además, puede implementar Azure Firewall en segmentos de la red virtual, según corresponda, con la inteligencia sobre amenazas habilitada y configurada para "Alertar y denegar" el tráfico de red malintencionado.

- [Realización de detección de intrusiones en la red con Network Watcher y herramientas de código abierto](../network-watcher/network-watcher-intrusion-detection-open-source-tools.md)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Instrucciones**: Use etiquetas de servicio de red virtual con el fin de definir controles de acceso a la red en grupos de seguridad de red o en Azure Firewall configurado en Azure que requiera acceso a los recursos de Automation. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, GuestAndHybridManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Instrucciones**: Defina e implemente configuraciones de seguridad estándar para los recursos de red usados por Azure Automation con Azure Policy.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de Azure RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ejemplos de Azure Policy para redes](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: Use etiquetas para NSG y otros recursos relacionados con la seguridad de red y el flujo de tráfico. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Instrucciones**: Use el registro de actividad de Azure para supervisar las configuraciones de recursos y detectar cambios en los recursos de red. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Desvíe los datos de registro a los registros de Azure Monitor para agregar los datos de seguridad generados por los recursos de Azure Automation. En Azure Monitor, use las consultas de registro para buscar y realizar análisis, y utilice cuentas de Azure Storage para el almacenamiento de archivos a largo plazo. Azure Automation puede enviar el estado de trabajo del runbook, los flujos de trabajo, los datos de configuración del estado de Automation, la administración de actualizaciones y el seguimiento de cambios o los registros de inventario al área de trabajo de Log Analytics. Esta información está visible desde la API de registros de Azure Portal, Azure PowerShell y Azure Monitor, lo que permite realizar investigaciones sencillas.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros. 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings)

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Reenvío de datos de un trabajo de Azure Automation a registros de Azure Monitor](automation-manage-send-joblogs-log-analytics.md)

- [Integración de DSC con registros de Azure Monitor](automation-dsc-diagnostics.md)

- [Regiones admitidas para el área de trabajo de Log Analytics vinculada](how-to/region-mappings.md)

- [Consulta de registros de Update Management](update-management/query-logs.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Habilite Azure Monitor para el acceso a los registros de auditoría y actividad que incluyen el origen del evento, la fecha, el usuario, la marca de tiempo, las direcciones de origen y de destino, y otros elementos útiles. 

- [Recopilación de registros y métricas de plataforma con Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings) 

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Instrucciones**: En Azure Monitor, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

- [Cambio del período de retención de datos en Log Analytics](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Detalles de retención de datos para cuentas de Automation](https://docs.microsoft.com/azure/automation/automation-managing-data#data-retention)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Analice y supervise los registros en busca de comportamientos anómalos y revise los resultados con regularidad. Use las consultas de registro de Azure Monitor para revisar los registros y realizar consultas en los datos de registro.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros. 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Descripción de las consultas de registro en Azure Monitor](/azure/azure-monitor/log-query/log-analytics-tutorial)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Use Azure Security Center con Azure Monitor para la supervisión y alerta de actividades anómalas que se encuentren en los registros y eventos de seguridad.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alerta sobre datos de registro de Azure Monitor](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: Implemente una solución de terceros de Azure Marketplace para la solución de registro DNS según las necesidades de su organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Use los roles de administrador integrados de Azure Active Directory (Azure AD) que se pueden asignar explícitamente y se pueden consultar. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc para detectar cuentas que son miembros de grupos administrativos. Siempre que use cuentas de ejecución de la cuenta de Automation para los runbooks, asegúrese de que estas entidades de servicio también se siguen en el inventario, ya que a menudo tienen permisos elevados. Elimine las cuentas de ejecución no utilizadas para minimizar la superficie expuesta a ataques.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

- [Eliminación de una cuenta de ejecución o de ejecución clásica](delete-run-as-account.md)

- [Administración de una cuenta de ejecución de Azure Automation](manage-runas-account.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: La cuenta de Azure Automation no tiene el concepto de contraseñas predeterminadas.  Los clientes son responsables de las aplicaciones de terceros y de los servicios de marketplace que pueden usar contraseñas predeterminadas que se ejecutan sobre el servicio o sus Hybrid Runbook Worker.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas. Siempre que use cuentas de ejecución de la cuenta de Automation para los runbooks, asegúrese de que estas entidades de servicio también se siguen en el inventario, ya que a menudo tienen permisos elevados. Examine las identidades con permisos con privilegios mínimos que necesiten para que los runbooks realicen correctamente su proceso automatizado. Elimine las cuentas de ejecución no utilizadas para minimizar la superficie expuesta a ataques.

También puede habilitar el acceso Just-in-Time/Just-Enough usando roles con privilegios de Privileged Identity Management de Azure Active Directory (Azure AD) para los servicios de Microsoft y Azure Resource Manager.

- [Más información acerca de Privileged Identity Management](/azure/active-directory/privileged-identity-management/)

- [Eliminación de una cuenta de ejecución o de ejecución clásica](delete-run-as-account.md)

- [Administración de una cuenta de ejecución de Azure Automation](manage-runas-account.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Siempre que sea posible, use el SSO con Azure Active Directory (Azure AD) en lugar de configurar credenciales independientes individuales por servicio. Use las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Inicio de sesión único para aplicaciones de Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

- [Uso de Azure AD para autenticarse en Azure](automation-use-azure-ad.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: Use PAW con la autenticación multifactor configurada para iniciar sesión y configurar los recursos de la cuenta de Azure Automation en entornos de producción. 

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas 

**Guía**: Use la característica de detecciones de riesgo de Azure Active Directory (Azure AD) para ver alertas e informes sobre el comportamiento de los usuarios de riesgo. De manera opcional, puede desviar las alertas de detección de riesgo de Azure Security Center a Azure Monitor y configurar alertas o notificaciones personalizadas con grupos de acciones.

- [Descripción de las detecciones de riesgo de Azure Security Center (actividad sospechosa)](../active-directory/identity-protection/overview-identity-protection.md)

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Configuración de grupos de acciones para alertas y notificaciones personalizadas](/azure/azure-monitor/platform/action-groups)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas 

**Guía**: Se recomienda usar ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones. 

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios. Si usa Hybrid Runbook Worker, puede aprovechar las identidades administradas en lugar de las cuentas de ejecución para habilitar permisos seguros más eficientes.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory-domain-services/tutorial-create-instance.md)

- [Uso de la autenticación de runbooks con identidades administradas](https://docs.microsoft.com/azure/automation/automation-hrw-run-runbooks#runbook-auth-managed-identities)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones**: Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. Siempre que use cuentas de ejecución de la cuenta de Automation para los runbooks, asegúrese de que estas entidades de servicio también se siguen en el inventario, ya que a menudo tienen permisos elevados. Elimine las cuentas de ejecución no utilizadas para minimizar la superficie expuesta a ataques.

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring/)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

- [Eliminación de una cuenta de ejecución o de ejecución clásica](delete-run-as-account.md)

- [Administración de una cuenta de ejecución de Azure Automation](manage-runas-account.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: Tiene acceso a los orígenes de registro de eventos de riesgo, auditoría y actividad de inicio de sesión de Azure Active Directory (Azure AD), que permiten la integración en cualquier herramienta SIEM o supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use las características de protección de riesgos e identidad de Azure Active Directory (Azure AD) para configurar las respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios para el recurso de red. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía**: Para las cuentas de Azure Automation, el soporte técnico de Microsoft puede acceder a los metadatos de recursos de plataforma durante un caso de soporte abierto sin usar otra herramienta.

Sin embargo, cuando se usan instancias de Hybrid Runbook Worker respaldadas por Azure Virtual Machines y un tercero necesita acceder a los datos del cliente (por ejemplo, durante una solicitud de soporte técnico), use la Caja de seguridad del cliente (versión preliminar) para Azure Virtual Machines a fin de revisar y aprobar o rechazar las solicitudes de acceso a los datos del cliente.

- [Descripción de la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: Use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure Automation que almacenan o procesan información confidencial. 

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Aísle los entornos mediante el uso de recursos separados de la cuenta de Automation. Los recursos como Hybrid Runbook Worker deben separarse por red virtual o subred, etiquetarse adecuadamente y protegerse en un grupo de seguridad de red (NSG) o Azure Firewall. Para las máquinas virtuales que almacenan o procesan datos confidenciales, implemente la directiva y los procedimientos necesarios para desactivarlos cuando no estén en uso.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas o alertas y denegación con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: Cunado use la característica Hybrid Runbook Worker, use una solución de terceros de Azure Marketplace en los perímetros de red que supervise la transferencia no autorizada de información confidencial y bloquee dichas transferencias al tiempo que alerta a los profesionales de seguridad de la información.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y protege a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Instrucciones**: Cifre toda la información confidencial en tránsito. Asegúrese de que los clientes que se conectan a los recursos de Azure en redes virtuales de Azure pueden negociar TLS 1.2 o superior. Azure Automation es totalmente compatible y aplica la capa de transporte (TLS) 1.2 y todas las llamadas de cliente o versiones posteriores para todos los puntos de conexión HTPS externos (a través de webhooks, nodos de DSC, Hybrid Runbook Worker).

Siga las recomendaciones de Azure Security Center para el cifrado en reposo y el cifrado en tránsito, si procede.

- [Descripción del cifrado en tránsito con Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

- [Aplicación de TLS 1.2 para Azure Automation](../active-directory/hybrid/reference-connect-tls-enforcement.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Instrucciones**: Use una herramienta de detección activa de terceros para identificar toda la información confidencial almacenada, procesada o transmitida por los sistemas tecnológicos de la organización, incluidos los del ubicación local o los que se encuentran en un proveedor de servicios remoto, y actualizar el inventario de información confidencial de la organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Use el control de acceso basado en rol de Azure (Azure RBAC) para controlar el acceso a los recursos de Azure Automation con las definiciones de roles integrados y asignar el acceso a los usuarios que acceden a los recursos de Automation siguiendo un modelo de acceso con privilegios mínimos o "suficientes". Al usar instancias de Hybrid Runbook Worker, aproveche las identidades administradas de esas máquinas virtuales para evitar el uso de entidades de servicio y, al usar multiinquilino o Hybrid Runbook Worker, asegúrese de aplicar los permisos de Azure RBAC con el ámbito correcto en la identidad de los trabajos de runbook.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Permisos del runbook para una instancia de Hybrid Runbook Worker](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker#runbook-permissions-for-a-hybrid-runbook-worker)

- [Administración de seguridad y permisos de roles](automation-role-based-access-control.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: En la actualidad, Azure Automation no expone las máquinas virtuales de Runbook Worker multiinquilino subyacentes, lo que se controla por la plataforma. Este control no es aplicable si usa el servicio listo para usar sin Hybrid Runbook Worker.

Si usa instancias de Hybrid Runbook Worker respaldadas por Azure Virtual Machines, debe usar una solución de prevención de pérdida de datos basada en host de terceros para aplicar controles de acceso a las máquinas virtuales hospedadas en Hybrid Runbook Worker.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Use claves administradas por el cliente con Azure Automation. Azure Automation admite el uso de claves administradas por el cliente para cifrar todos los "recursos seguros" utilizados como credenciales, certificados, conexiones y variables cifradas. Aproveche las variables cifradas con sus runbooks para todas las necesidades de búsqueda de variables persistentes a fin de evitar una exposición imprevista.

Al usar instancias de Hybrid Runbook Worker, los discos virtuales de las máquinas virtuales se cifran en reposo mediante el cifrado del lado servidor o el cifrado de discos de Azure (ADE). Azure Disk Encryption aprovecha la característica BitLocker de Windows para cifrar los discos administrados con claves administradas por el cliente dentro de la máquina virtual invitada. El cifrado del lado servidor con claves administradas por el cliente mejora en ADE al permitir el uso de cualquier tipo de sistema operativo y de imágenes para las máquinas virtuales mediante el cifrado de datos en el servicio Storage.

- [Cifrado del lado servidor de Azure Managed Disks](../virtual-machines/disk-encryption.md)

- [Azure Disk Encryption para máquinas virtuales Windows](../virtual-machines/windows/disk-encryption-overview.md)

- [Uso de las claves administradas por el cliente para una cuenta de Automation](https://docs.microsoft.com/azure/automation/automation-secure-asset-encryption#use-of-customer-managed-keys-for-an-automation-account)

- [Variables administradas en Azure Automation](shared-resources/variables.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de [sus recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy: Microsoft.Automation**:

[!INCLUDE [Resource Policy for Microsoft.Automation 4.8](../../includes/policy/standards/asb/rp-controls/microsoft.automation-4-8.md)]

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure, como componentes de red, cuentas de Azure Automation y runbooks. 

- [Registros de diagnóstico de un grupo de seguridad de red](https://docs.microsoft.com/azure/private-link/private-link-overview#logging-and-monitoring)

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en los recursos de Azure.

- [Recomendaciones de seguridad en Azure Security Center](../security-center/security-center-recommendations.md)

- [Referencia de recomendaciones de Security Center](../security-center/recommendations-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Exporte los resultados de análisis en intervalos coherentes y compare los resultados para comprobar que se han corregido las vulnerabilidades. Al usar una recomendación de administración de vulnerabilidades sugerida por Azure Security Center, el cliente puede ir al portal de la solución seleccionada para ver los datos de análisis históricos.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Instrucciones**: Use la clasificación del riesgo predeterminada (puntuación segura) proporcionada por Azure Security Center para ayudar a priorizar la corrección de las vulnerabilidades detectadas.

- [Descripción de la puntuación de seguridad de Azure Security Center](../security-center/secure-score-security-controls.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar todos los recursos de Azure Automation en las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía. 

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Instrucciones**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure Automation. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna. Elimine las cuentas de ejecución no utilizadas para minimizar la superficie expuesta a ataques.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Eliminación de una cuenta de ejecución o de ejecución clásica](delete-run-as-account.md)

- [Administración de una cuenta de ejecución de Azure Automation](manage-runas-account.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Instrucciones**: tendrá que crear un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas: 

- Tipos de recursos no permitidos 
- Tipos de recursos permitidos 

Además, use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones. Esto puede ayudar en entornos de alta seguridad, como aquellos con cuentas de Storage. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Complementos de ejemplo de Azure Policy para Azure Automation](policy-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Instrucciones**: El cliente puede impedir la creación o el uso de recursos con Azure Policy según las directrices de la empresa del cliente. Puede implementar su propio proceso para quitar recursos no autorizados. Dentro de la oferta de Azure Automation, es posible instalar, quitar y administrar los módulos de PowerShell o de Python a los que los runbooks pueden acceder a través del Portal o los cmdlets. El módulo no aprobado o antiguo se debe quitar o actualizar para los runbooks.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Administración de módulo en Azure Automation](shared-resources/modules.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Use directivas de acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure" de las ubicaciones o dispositivos no seguros o no aprobados. 

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de Azure Automation y los recursos relacionados. También puede usar definiciones de Azure Policy integradas.

Además, Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan o superen los requisitos de seguridad de la organización.

También puede usar las recomendaciones de Azure Security Center como línea de base de configuración segura para los recursos de Azure.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Complementos de ejemplo de Azure Policy para Azure Automation](policy-reference.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use las plantillas de Azure Resource Manager y Azure Policy para configurar de forma segura los recursos de Azure asociados a Azure Automation. Las plantillas de Azure Resource Manager son archivos basados en JSON que se usan para implementar los recursos de Azure, y las plantillas personalizadas deberán almacenarse y mantenerse de forma segura en un repositorio de código. Use la característica de integración del control de código fuente para mantener actualizados los runbooks de la cuenta de Automation con los scripts del repositorio de control de código fuente. Utilice las directivas [deny] y [deploy if not exist] de Azure Policy para aplicar una configuración segura en los recursos de Azure.

- [Uso de la integración del control de código fuente](source-control-integration.md)

- [Información sobre la creación de plantillas de Azure Resource Manager](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Implementación de una cuenta de Automation con una plantilla de Azure Resource Manager](/azure/automation/quickstart-create-account-template#deploy-the-template)

- [Complementos de ejemplo de Azure Policy para Azure Automation](policy-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Instrucciones**: Use Azure DevOps para almacenar y administrar de forma segura el código, como directivas de Azure personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. Para acceder a los recursos que administra en Azure DevOps, puede conceder o denegar permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integran con Azure DevOps, o Active Directory si se integran con TFS. Use la característica de integración del control de código fuente para mantener actualizados los runbooks de la cuenta de Automation con los scripts del repositorio de control de código fuente.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

- [Uso de la integración del control de código fuente](source-control-integration.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de Azure con Azure Policy. Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los recursos de Azure. También puede usar definiciones de directivas integradas relacionadas con recursos concretos. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

- [Complementos de ejemplo de Azure Policy para Azure Automation](policy-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Instrucciones**: Use Azure Policy para alertar y auditar las configuraciones de recursos de Azure, la directiva se puede usar para detectar ciertos recursos no configurados con un punto de conexión privado.

Al usar la característica Hybrid Runbook Worker, aproveche Azure Security Center para realizar exámenes de base de referencia para Azure Virtual Machines.  Los métodos adicionales para la configuración automatizada incluyen: State Configuration de Azure Automation.

- [Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

- [Introducción a State Configuration de Azure Automation State Configuration](automation-dsc-getting-started.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Complementos de ejemplo de Azure Policy para Azure Automation](policy-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: Use Azure Resource Manager para implementar las cuentas de Azure Automation y los recursos relacionados. Azure Resource Manager proporciona la capacidad de exportar plantillas que se pueden usar como copias de seguridad para restaurar las cuentas de Azure Automation y los recursos relacionados. Use Azure Automation para llamar a la API de exportación de plantillas de Azure Resource Manager de forma periódica.

Use la característica de integración del control de código fuente para mantener actualizados los runbooks de la cuenta de Automation con los scripts del repositorio de control de código fuente.

- [Introducción sobre Azure Resource Manager](../azure-resource-manager/management/overview.md)

- [Referencia de plantillas de Azure Resource Manager para recursos de Azure Automation](/azure/templates/microsoft.automation/allversions)

- [Creación de una cuenta de Automation con una plantilla de Azure Resource Manager](quickstart-create-automation-account-template.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Grupos de recursos: Exportar plantilla](/rest/api/resources/resourcegroups/exporttemplate)

- [Introducción a Azure Automation](automation-intro.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

- [Uso de las claves administradas por el cliente para una cuenta de Automation](https://docs.microsoft.com/azure/automation/automation-secure-asset-encryption#use-of-customer-managed-keys-for-an-automation-account)

- [Uso de la integración del control de código fuente](source-control-integration.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: Use Azure Resource Manager para implementar las cuentas de Azure Automation y los recursos relacionados. Azure Resource Manager proporciona la capacidad de exportar plantillas que se pueden usar como copias de seguridad para restaurar las cuentas de Azure Automation y los recursos relacionados. Use Azure Automation para llamar a la API de exportación de plantillas de Azure Resource Manager de forma periódica. Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault. Puede exportar los runbooks a archivos de script mediante Azure Portal o PowerShell.

- [Introducción sobre Azure Resource Manager](../azure-resource-manager/management/overview.md)

- [Referencia de plantillas de Azure Resource Manager para recursos de Azure Automation](/azure/templates/microsoft.automation/allversions)

- [Creación de una cuenta de Automation con una plantilla de Azure Resource Manager](quickstart-create-automation-account-template.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Grupos de recursos: Exportar plantilla](/rest/api/resources/resourcegroups/exporttemplate)

- [Introducción a Azure Automation](automation-intro.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

- [Uso de las claves administradas por el cliente para una cuenta de Automation](https://docs.microsoft.com/azure/automation/automation-secure-asset-encryption#use-of-customer-managed-keys-for-an-automation-account)

- [Copia de seguridad de datos de Azure para cuentas de Automation](https://docs.microsoft.com/azure/automation/automation-managing-data#data-backup)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: En caso necesario, asegúrese de poder realizar periódicamente la implementación de plantillas de Azure Resource Manager de forma habitual en una suscripción aislada. Pruebe la restauración de la copia de seguridad de las claves administradas por el cliente.

- [Implementación de recursos con Azure Portal y plantillas de Resource Manager](../azure-resource-manager/templates/deploy-portal.md)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

- [Uso de las claves administradas por el cliente para una cuenta de Automation](https://docs.microsoft.com/azure/automation/automation-secure-asset-encryption#use-of-customer-managed-keys-for-an-automation-account)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como las plantillas de Azure Resource Manager. Para proteger los recursos que administra en Azure DevOps, puede conceder o denegar permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integran con Azure DevOps, o en Active Directory si se integran con TFS.

Use la característica de integración del control de código fuente para mantener actualizados los runbooks de la cuenta de Automation con los scripts del repositorio de control de código fuente.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

- [Uso de la integración del control de código fuente](source-control-integration.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta. 

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) con etiquetas y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las funcionalidades de respuesta a los incidentes de los sistemas periódicamente; así, ayudará a proteger los recursos de Azure. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Guía**: Exporte las alertas y recomendaciones de Azure Security Center y mediante la característica de exportación continua para ayudar a identificar los riesgos para los recursos de Azure. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: Use la característica Automatización de flujo de trabajo de Azure Security Center para desencadenar automáticamente las respuestas a través de Logic Apps en las alertas y recomendaciones de seguridad para proteger los recursos de Azure.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos 

**Guía**: Siga las reglas de compromiso de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
