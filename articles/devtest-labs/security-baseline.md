---
title: Base de referencia de seguridad de Azure para Azure DevTest Labs
description: La base de referencia de seguridad de Azure DevTest Labs proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: devtest-lab
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 13585173b54c79158161da54f968ba66eb018f6a
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101705745"
---
# <a name="azure-security-baseline-for-azure-devtest-labs"></a>Base de referencia de seguridad de Azure para Azure DevTest Labs

Esta línea de base de seguridad aplica las instrucciones descritas en [Información general sobre Azure Security Benchmark (v1)](../security/benchmarks/overview-v1.md) a Azure DevTest Labs. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y por las instrucciones relacionadas aplicables a Azure DevTest Labs. Se han excluido los **controles** no aplicables a Azure DevTest Labs.

Para ver cómo se adapta Azure DevTest Labs por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure DevTest Labs](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: al implementar los recursos de Azure DevTest Labs, debe crear o usar una red virtual existente. Asegúrese de que la red virtual elegida tenga aplicado un grupo de seguridad de red a sus subredes y configurados los controles de acceso a la red específicamente para los puertos y orígenes de confianza de la aplicación. Cuando se configuran los recursos de un laboratorio con una red virtual, no están disponibles públicamente y solo se puede acceder a ellos desde dentro de la red virtual. También puede optar por crear un laboratorio aislado de la red, donde, junto con los entornos de laboratorio, los recursos de laboratorio, como la cuenta de almacenamiento y los almacenes de claves, también estarán completamente aislados y solo se podrá acceder a ellos mediante los puntos de conexión especificados. 

En función de las necesidades de su organización, use el servicio Azure Firewall para crear, aplicar y registrar de forma centralizada directivas de conectividad de red y de aplicaciones entre suscripciones y redes virtuales.

- [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md)

- [Creación de una instancia de DevTest Labs con aislamiento de red](network-isolation.md)

- [Creación de un grupo de seguridad de red con reglas de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación y configuración de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

**Guía**: implemente un grupo de seguridad de red en la red donde se implementan los recursos de Azure DevTest Labs. Habilite los registros de flujo de grupos de seguridad de red en los grupos de seguridad de red para la auditoría del tráfico.

También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: implemente Azure Web Application Firewall (WAF) delante de las aplicaciones web críticas implementadas con las plantillas de Azure Resource Manager para la inspección adicional del tráfico entrante. Habilite la configuración de diagnóstico para WAF e ingiera los registros en una cuenta de almacenamiento, un centro de eventos o un área de trabajo de Log Analytics.

- [Implementación de WAF de Azure](../web-application-firewall/ag/create-waf-policy-ag.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: la implementación de una red virtual (VNet) de Azure para un laboratorio mejora la seguridad y el aislamiento del laboratorio. Las subredes, las directivas de control de acceso y otras características también ayudan a restringir el acceso. Al realizar la implementación en una red virtual, Azure DevTest Labs no está disponible públicamente y solo se podrá acceder a él desde máquinas virtuales y aplicaciones dentro de la red virtual.

Habilite la protección contra DDoS estándar en las redes virtuales de Azure para protegerse frente a ataques DDoS. Use la inteligencia sobre amenazas integrada en Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas.

Implemente Azure Firewall en cada uno de los límites de red de la organización con la inteligencia de amenazas habilitada y configurada para alertar y denegar el tráfico de red malintencionado. Use el acceso de red Just-in-Time de Azure Security Center para configurar los NSG para limitar la exposición de los puntos de conexión a las direcciones IP aprobadas durante un período limitado. Use la protección de redes adaptativa de Azure Security Center para recomendar configuraciones de NSG que limiten los puertos y las direcciones IP de origen según el tráfico real y la inteligencia sobre amenazas.

- [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md)

- [Configuración de la protección contra DDoS](/azure/virtual-network/manage-ddos-protection)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](/azure/security-center/threat-protection)

- [Descripción de la protección de red adaptable de Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)

- [Descripción del control de acceso de red Just-in-Time de Azure Security Center](../security-center/security-center-just-in-time.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: habilite la captura de paquetes de Network Watcher en la red virtual de laboratorio para investigar actividades anómalas. 

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: Use una instancia de Azure Firewall implementada en la red virtual con la inteligencia sobre amenazas habilitada. El filtrado basado en inteligencia sobre amenazas de Azure Firewall puede alertar y denegar el tráfico desde y hacia los dominios y las direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. Si su organización requiere una funcionalidad adicional aparte de la que Azure Firewall puede ofrecer, seleccione una oferta apropiada en Azure Marketplace que admita la funcionalidad de IDS/IPS con capacidad de inspección de cargas útiles.

Implemente la solución de firewall que prefiera en cada uno de los límites de red de su organización para detectar y/o denegar el tráfico malintencionado.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)
 

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)
 

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Guía**: al trabajar con entornos basados en Azure Resource Manager de DevTest Labs que incluyen aplicaciones web, implemente una instancia de Azure Application Gateway con HTTPS/TLS habilitado para certificados de confianza.

- [Implementación de Application Gateway](../application-gateway/quick-create-portal.md)

- [Configuración de Application Gateway para usar HTTPS](../application-gateway/create-ssl-portal.md)

- [Descripción del equilibrio de carga de capa 7 con las puertas de enlace de aplicaciones web de Azure](../application-gateway/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de Azure Virtual Network para definir controles de acceso a la red en los grupos de seguridad de red o las instancias de Azure Firewall configuradas para los recursos de DevTest Labs. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

También puede usar grupos de seguridad de aplicaciones para simplificar una configuración de seguridad compleja. Los grupos de seguridad de aplicaciones le permiten configurar la seguridad de red como una extensión natural de la estructura de una aplicación, lo que le permite agrupar máquinas virtuales y directivas de seguridad de red basadas en esos grupos.

- [Descripción y uso de las etiquetas de servicio](../virtual-network/service-tags-overview.md)

- [Descripción y uso de los grupos de seguridad de aplicaciones](/azure/virtual-network/security-overview#application-security-groups)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos de red con Azure Policy.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ejemplos de Azure Policy para redes](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: use etiquetas para los recursos de red asociados a la implementación de Azure DevTest Labs a fin de organizarlos lógicamente en una taxonomía. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de recursos y detectar cambios en los recursos de Azure. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Uso de orígenes de sincronización de hora aprobados

**Instrucciones**: Microsoft mantiene los orígenes de hora de los recursos de Azure. Sin embargo, puede administrar la configuración de sincronización de hora de los recursos de proceso.

- [Sincronizar la hora en las máquinas virtuales de Windows en Azure](../virtual-machines/windows/time-sync.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Los registros de actividad proporcionan información sobre las operaciones que se realizaron en las instancias de Azure DevTest Labs en el nivel de plano de administración. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) realizadas en el nivel del plano de administración para las instancias de DevTest Labs.

- [Creación de una configuración de diagnóstico para enviar registros de plataforma y métricas a diferentes destinos](/azure/azure-monitor/platform/diagnostic-settings)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Los registros de actividad proporcionan información sobre las operaciones que se realizaron en las instancias de Azure DevTest Labs en el nivel de plano de administración. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) realizadas en el nivel del plano de administración para las instancias de DevTest Labs.

- [Creación de una configuración de diagnóstico para enviar registros de plataforma y métricas a diferentes destinos](/azure/azure-monitor/platform/diagnostic-settings)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: el cliente crea y es propietario de las máquinas virtuales (VM) de Azure DevTest Labs. Por lo tanto, su supervisión es responsabilidad de la organización. Puede usar Azure Security Center para supervisar el sistema operativo del proceso. Entre los datos que recopila Security Center del sistema operativo se incluyen: tipo y versión del sistema operativo, sistema operativo (registros de eventos de Windows), procesos en ejecución, nombre de la máquina, direcciones IP y usuario conectado. El agente de Log Analytics también recopila los archivos de volcado de memoria.

Para más información, consulte los siguientes artículos.

- [Recopilación de registros de host internos de máquina virtual de Azure con Azure Monitor](/azure/azure-monitor/learn/quick-collect-azurevm)

- [Descripción de la recopilación de datos de Azure Security Center](../security-center/security-center-enable-data-collection.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: en Azure Monitor, establezca el período de retención de registro para las áreas de trabajo de Log Analytics asociadas a las instancias de Azure DevTest Labs según las normativas de cumplimiento de su organización.

- [Para más información, consulte el artículo siguiente](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period).

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: Habilite la configuración de diagnóstico en el registro de actividad de Azure y envíe los registros a un área de trabajo de Log Analytics. Ejecute consultas en Log Analytics para buscar términos, identificar tendencias, analizar patrones y proporcionar mucha otra información detallada de los datos del registro de actividad que se hayan recopilado para Azure DevTest Labs.

Para más información, consulte los siguientes artículos.

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/diagnostic-settings)

- [Recopilación y análisis de registros de actividad de Azure en un área de trabajo de Log Analytics en Azure Monitor](/azure/azure-monitor/platform/activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: use el área de trabajo de Azure Log Analytics para supervisar los registros de seguridad y eventos relacionados con Azure DevTest Labs y generar alertas sobre actividades anómalas en él.

- [Alertas sobre datos de registro de Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Azure Active Directory (Azure AD) tiene roles integrados que se deben asignar explícitamente y son consultables. Use el módulo de PowerShell de Azure AD para ejecutar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

- [Roles de Azure DevTest Labs](devtest-lab-add-devtest-user.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Active Directory (Azure AD) no tiene el concepto de contraseñas predeterminadas. Otros recursos de Azure que requieren una contraseña obligan a crear una contraseña con requisitos de complejidad y una longitud mínima, que cambia en función del servicio. Es responsable de las aplicaciones de terceros y de los servicios de Marketplace que pueden usar contraseñas predeterminadas.

DevTest Labs no tiene el concepto de contraseñas predeterminadas.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarle a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure Policy, por ejemplo:

- Debe haber más de un propietario asignado a su suscripción

- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

- [Uso de Azure Security Center para supervisar la identidad y el acceso (versión preliminar)](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Roles de Azure DevTest Labs](devtest-lab-add-devtest-user.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Guía**: DevTest Labs usa el servicio de Azure Active Directory (Azure AD) para la administración de identidades. Cuando conceda a los usuarios acceso a un entorno basado en DevTest Labs, tenga en cuenta estos dos aspectos clave:
- Administración de recursos: Proporciona acceso a Azure Portal para administrar recursos (crear máquinas virtuales; crear entornos; iniciar, detener, reiniciar, eliminar y aplicar artefactos, etc.). La administración de recursos se lleva a cabo en Azure mediante el control de acceso basado en rol de Azure (Azure RBAC). Usted asigna roles a los usuarios y establece permisos de nivel de acceso y recursos.
- Máquinas virtuales (nivel de red) : En la configuración predeterminada, las máquinas virtuales usan una cuenta de administrador local. Si hay un dominio disponible, como Azure Active Directory Domain Services (Azure AD DS), un dominio local o un dominio basado en la nube, las máquinas pueden unirse al dominio. Los usuarios pueden usar sus identidades basadas en dominio mediante el artefacto de unión a un dominio para conectarse a las máquinas.

- [Arquitectura de referencia para DevTest Labs](https://docs.microsoft.com/azure/devtest-labs/devtest-lab-reference-architecture#architecture)

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

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

**Guía**: Use los informes de seguridad de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzcan actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Administración de los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones**: Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/overview-reports.md)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: puede usar orígenes de registro de actividad de inicio de sesión de Azure Active Directory (Azure AD), auditoría y riesgos para la supervisión, lo que permite realizar la integración con cualquier herramienta de administración de eventos e información de seguridad (SIEM) y de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use las características de protección de identidad y detección de riesgo de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas que se detecten relacionadas con las identidades de los usuarios.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía**: use el control de acceso basado en rol de Azure (Azure RBAC) para controlar el acceso a los laboratorios de Azure DevTest Labs.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Descripción de los roles de DevTest Labs](devtest-lab-add-devtest-user.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: use Azure Monitor con el registro de actividad de Azure para crear alertas cuando se produzcan cambios en instancias de DevTest Labs y en otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

- [Creación de alertas para los eventos del registro de actividad de DevTest Labs](create-alerts.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: use Azure Resource Graph para consultar y detectar todos los recursos (incluidos los recursos de DevTest Labs) de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure y los recursos de las suscripciones.

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica según una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Configuración de etiquetas de DevTest Labs](devtest-lab-add-tag.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes (y, si procede, laboratorios independientes) para organizar y realizar un seguimiento de los laboratorios y los recursos relacionados con ellos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan rápidamente de la suscripción.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación de un laboratorio con DevTest Labs](devtest-lab-create-lab.md)

- [Creación y uso de etiquetas](devtest-lab-add-tag.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización. Como administrador de la suscripción, también podría usar los controles de aplicaciones adaptables, una característica de Azure Security Center para definir un conjunto de aplicaciones que se pueden ejecutar en los grupos configurados de máquinas de laboratorio. Esta característica está disponible tanto para máquinas virtuales y servidores de Azure como Windows (todas las versiones, clásica o Azure Resource Manager) y Linux que no son de Azure.

- [Habilitación de los controles de aplicaciones adaptables](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones. Esto puede ayudar en entornos de alta seguridad, como aquellos con cuentas de almacenamiento.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Azure Automation proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos. Como administrador de la suscripción, puede usar el inventario de Azure Virtual Machine para automatizar la recopilación de información sobre todo el software de las máquinas virtuales de DevTest Labs de la suscripción. Las propiedades de nombre del software, versión, publicador y hora de actualización están disponibles en Azure Portal. Para obtener acceso a la fecha de instalación y otra información, se requiere que el cliente habilite los diagnósticos de nivel de invitado y transfiera los registros de eventos de Windows a un área de trabajo de Log Analytics.

Además de usar Change Tracking para la supervisión de las aplicaciones de software, los controles de aplicaciones adaptables de Azure Security Center usan el aprendizaje automático para analizar las aplicaciones que se ejecutan en los equipos y crear una lista de permitidos a partir de esta inteligencia. Esta funcionalidad simplifica enormemente el proceso de configuración y mantenimiento de directivas de listas de aplicaciones permitidas, lo que le permite evitar el uso de software no deseado en su entorno. Puede configurar el modo de auditoría o de aplicación. El modo de auditoría solo audita la actividad en las máquinas virtuales protegidas. El modo de aplicación exige el cumplimiento de las reglas y se asegura de bloquear las aplicaciones cuya ejecución no está permitida. 

- [Introducción a Azure Automation](../automation/automation-intro.md)

- [Habilitación del inventario de máquinas virtuales de Azure](../automation/automation-tutorial-installed-software.md)

- [Información sobre los controles de aplicaciones adaptables](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: Azure Automation proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos. Como administrador de la suscripción, puede usar Change Tracking para identificar todo el software instalado en las máquinas virtuales hospedadas en DevTest Labs. Puede implementar su propio proceso o usar State Configuration de Azure Automation para quitar software no autorizado.

- [Introducción a Azure Automation](../automation/automation-intro.md)

- [Seguimiento de cambios en el entorno con la solución Change Tracking](../automation/change-tracking/overview.md)

- [Introducción a Azure Automation State Configuration](../automation/automation-dsc-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: como administrador de la suscripción, puede usar los controles de aplicaciones adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado esté bloqueado para que no se ejecute en máquinas virtuales de Azure hospedadas en DevTest Labs.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Material de referencia:

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía**: el control de aplicaciones adaptables es una solución integral, inteligente y automatizada de Azure Security Center que ayuda a controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales de Azure y que no son de Azure (Windows y Linux), hospedadas en DevTest Labs. Tenga en cuenta que debe ser administrador de la suscripción para configurar esta opción para los recursos de proceso subyacentes hospedados en DevTest Labs. Implemente una solución de terceros si esta configuración no cumple con los requisitos de la organización.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción **Bloquear acceso** en la aplicación **Administración de Microsoft Azure**.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="612-limit-users-ability-to-execute-scripts-in-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Guía**: según el tipo de scripts, puede usar configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en las máquinas virtuales hospedadas en DevTest Labs. También puede usar los controles de aplicaciones adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en las máquinas virtuales de Azure subyacentes.

- [Control de la ejecución de scripts de PowerShell en entornos Windows](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7&amp;preserve-view=true)

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: las aplicaciones de alto riesgo implementadas en el entorno de Azure se pueden aislar mediante la red virtual, la subred, las suscripciones, los grupos de administración, etc. y suficientemente protegidos con una instancia de Azure Firewall, un firewall de aplicaciones web (WAF) o un grupo de seguridad de red (NSG).

- [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md)

- [Información general de Azure Firewall](../web-application-firewall/overview.md)

- [Introducción al firewall de aplicaciones web](../virtual-network/network-security-groups-overview.md)

- [Introducción a la seguridad de red](security-baseline.md)

- [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)

- [Guía de decisiones de suscripción](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los recursos de Azure creados como parte de DevTest Labs. También puede usar definiciones de Azure Policy integradas.

Además, Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan o superen los requisitos de seguridad de la organización.

También puede usar las recomendaciones de Azure Security Center como línea de base de configuración segura para los recursos de Azure.

- [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: use las recomendaciones de Azure Security Center para mantener las configuraciones de seguridad de todos los recursos de proceso subyacentes creados como parte de DevTest Labs. Además, puede usar imágenes de sistema operativo personalizado, la configuración de estado de Azure Automation o artefactos de DevTest Labs para establecer la configuración de seguridad del sistema operativo que requiere su organización.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

- [Introducción a Azure Automation State Configuration](../automation/automation-dsc-overview.md)

- [Carga de un VHD y su uso para crear máquinas virtuales Windows nuevas en Azure](../virtual-machines/windows/upload-generalized-managed.md)

- [Creación de una VM Linux desde un disco personalizado con la CLI de Azure](../virtual-machines/linux/upload-vhd.md)

- [Creación y distribución de imágenes personalizadas en varias instancias de DevTest Labs](image-factory-save-distribute-custom-images.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: utilice las reglas **denegar** e **implementar si no existe** de Azure Policy para aplicar una configuración segura en los recursos de Azure creados como parte de DevTest Labs. Además, puede usar plantillas de Azure Resource Manager para mantener la configuración de seguridad de los recursos de Azure que requiere su organización.

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Información general sobre plantillas de Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en los recursos de proceso de Azure subyacentes creados como parte de un laboratorio. Además, puede usar plantillas de Azure Resource Manager, imágenes de sistema operativo personalizado o la configuración de estado de Azure Automation para mantener la configuración de seguridad del sistema operativo que requiere su organización. También puede usar una solución de generador de imágenes, que es una solución de configuración como código que crea y distribuye imágenes de forma automática y periódica con todas las configuraciones deseadas.

Además, las imágenes de máquinas virtuales de Azure Marketplace que publica Microsoft las administra y mantiene Microsoft mismo.

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

- [Introducción a Azure Automation State Configuration](../automation/automation-dsc-overview.md)

- [Script de ejemplo para cargar un disco duro virtual en Azure y crear una máquina virtual nueva](/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script)

- [Creación de un generador de imágenes personalizadas en Azure DevTest Labs](image-factory-create.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como directivas de Azure personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. Para acceder a los recursos que administra en Azure DevOps, puede conceder o denegar permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integran con Azure DevOps.

- [Tutorial de Git de Azure Repos](/azure/devops/repos/git/gitworkflow)

- [Acerca de los permisos y los grupos](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions?view=azure-devops&amp;tabs=preview-page&amp;preserve-view=true)

- [Integración entre Azure DevTest Labs y el flujo de trabajo de Azure DevOps](devtest-lab-dev-ops.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: Si usa imágenes personalizadas, use el control de acceso basado en rol de Azure (Azure RBAC) para asegurarse de que solo los usuarios autorizados pueden acceder a las imágenes. Con Shared Image Gallery, puede compartir sus imágenes con laboratorios específicos que las necesiten. En el caso de las imágenes de contenedor, almacénelas en Azure Container Registry y use Azure RBAC para asegurarse de que solo los usuarios autorizados puedan acceder a las imágenes.

- [Descripción de Azure RBAC](../role-based-access-control/rbac-and-directory-admin-roles.md)

- [Configuración de Azure RBAC](../role-based-access-control/quickstart-assign-role-user-portal.md)

- [Configuración de Shared Image Gallery para Azure DevTest Labs](configure-shared-image-gallery.md)

- [Descripción de Azure RBAC para Container Registry](../container-registry/container-registry-roles.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de Azure con Azure Policy. Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los recursos de Azure creados en DevTest Labs. También puede usar definiciones de directivas integradas relacionadas con recursos concretos. Además, puede usar Azure Automation para implementar los cambios de configuración.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Guía**: Azure Automation State Configuration es un servicio de administración de configuración para los nodos de Desired State Configuration (DSC) de cualquier centro de datos local o en la nube. Puede incorporar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento por parte de cada máquina del estado deseado especificado. También puede escribir un artefacto personalizado que pueda instalarse en todas las máquinas de laboratorio para asegurarse de que siguen las directivas de la organización. 

- [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](../automation/automation-dsc-onboarding.md)

- [Creación de artefactos personalizados para máquinas virtuales de DevTest Labs](devtest-lab-artifact-author.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: use Azure Security Center para realizar análisis de base de referencia para los recursos de Azure creados en DevTest Labs. Además, use Azure Policy para alertar y auditar las configuraciones de recursos de Azure.

- [Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía**: use Azure Security Center para realizar exámenes de base de referencia de la configuración del sistema operativo y de Docker para los contenedores que se ejecutan en el laboratorio.

- [Descripción de las recomendaciones sobre contenedores de Azure Security Center](../security-center/container-security.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones**: Use Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos para las aplicaciones en la nube.

- [Configuración de identidades administradas para implementar entornos de Azure Resource Manager en DevTest Labs](use-managed-identities-environments.md)

- [Configuración de identidades administradas para implementar máquinas virtuales en DevTest Labs](enable-managed-identities-lab-vms.md)

- [Creación de un almacén de claves](../key-vault/general/quick-create-portal.md)

- [Cómo proporcionar la autenticación de Key Vault con una identidad administrada](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Instrucciones**: Use identidades administradas para proporcionar servicios de Azure con una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluyendo Key Vault, sin necesidad de credenciales en el código.

- [Configuración de identidades administradas para implementar entornos de Azure Resource Manager en DevTest Labs](use-managed-identities-environments.md)

- [Configuración de identidades administradas para implementar máquinas virtuales en DevTest Labs](enable-managed-identities-lab-vms.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Cómo configurar Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-antimalware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: use Microsoft Antimalware para Azure, a fin de supervisar y proteger sus recursos continuamente. Para Linux, use una solución antimalware de terceros.  Use también la detección de amenazas de Azure Security Center para los servicios de datos para detectar malware cargado en cuentas de almacenamiento. 

- [Configuración de Microsoft Antimalware para Azure](../security/fundamentals/antimalware.md) 

- [Protección contra amenazas en Azure Security Center](/azure/security-center/threat-protection)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure App Service hospedado en un laboratorio), pero no se ejecuta en el contenido. 

Examine previamente los archivos que se cargan en recursos de Azure que no son de proceso, como App Service, Data Lake Storage, Blob Storage, etc. 

Use la detección de amenazas de Azure Security Center en los servicios de datos para detectar malware cargado en cuentas de almacenamiento. 

- [Descripción de Microsoft Antimalware para Azure](../security/fundamentals/antimalware.md) 

- [Descripción de la detección de amenazas de Azure Security Center para servicios de datos](/azure/security-center/security-center-alerts-data-services)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="83-ensure-antimalware-software-and-signatures-are-updated"></a>8.3: Garantía de la actualización del software y las firmas antimalware

**Guía**: Cuando se implementa, Microsoft Antimalware para Azure instalará automáticamente y de forma predeterminada las actualizaciones más recientes de firma, plataforma y motor. Siga las recomendaciones en Azure Security Center: "Procesos y aplicaciones" para asegurarse de que todos los puntos de conexión de los recursos de proceso subyacentes de DevTest Labs están actualizados con las firmas más recientes. El sistema operativo Windows se puede proteger aún más con seguridad adicional para limitar el riesgo de ataques basados en malware y virus con el servicio Microsoft Defender Advanced Threat Protection que se integra con Azure Security Center.

- [Implementación de Microsoft Antimalware para Azure](../security/fundamentals/antimalware.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: actualmente, Azure DevTest Labs no admite copias de seguridad de máquinas virtuales ni instantáneas. Sin embargo, puede habilitar y configurar Azure Backup en las máquinas virtuales de Azure subyacentes hospedadas en DevTest Labs. Además, también puede configurar la frecuencia y el período de retención deseados para las copias de seguridad automáticas, siempre que tenga acceso adecuado a los recursos de proceso subyacentes. 

- [Información general sobre la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md)

- [Copia de seguridad de una máquina virtual de Azure desde la configuración de esta](../backup/backup-azure-vms-first-look-arm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copias de seguridad de las claves administradas por el cliente

**Guía**: actualmente, Azure DevTest Labs no admite copias de seguridad de máquinas virtuales ni instantáneas. Sin embargo, puede crear instantáneas de las máquinas virtuales de Azure subyacentes hospedadas en DevTest Labs o en los discos administrados conectados a esas instancias mediante PowerShell o las API REST, siempre y cuando tenga acceso adecuado a los recursos de proceso subyacentes. También puede hacer una copia de seguridad de cualquier clave administrada por el cliente en Azure Key Vault.

Habilite Azure Backup en máquinas virtuales de Azure de destino y los períodos de retención y frecuencia deseados. Esto incluye una copia de seguridad completa del estado del sistema. Si usa Azure Disk Encryption, la copia de seguridad de la máquina virtual de Azure administra automáticamente la copia de seguridad de las claves administradas por el cliente.

- [Copia de seguridad en máquinas virtuales de Azure cifradas](../backup/backup-azure-vms-encryption.md)

- [Información general sobre la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md)

- [Información general sobre la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: Garantice la capacidad de realizar la restauración de datos de contenido en Azure Backup de forma periódica. Si es necesario, pruebe la restauración del contenido en una red virtual o suscripción aislada. Además, pruebe la restauración de la copia de seguridad de las claves administradas por el cliente.

Si usa Azure Disk Encryption, puede restaurar la máquina virtual de Azure con las claves de cifrado del disco. Cuando use Disk Encryption, podrá restaurar la máquina virtual de Azure con las claves de cifrado de discos.

- [Copia de seguridad en máquinas virtuales de Azure cifradas](../backup/backup-azure-vms-encryption.md)

- [Recuperación de archivos de la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-restore-files-from-vm.md)

- [Restauración de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

- [Copia de seguridad y restauración de una máquina virtual cifrada](../backup/backup-azure-vms-encryption.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: al realizar la copia de seguridad de discos administrados con Azure Backup, las máquinas virtuales se cifran en reposo con Storage Service Encryption (SSE). Azure Backup también puede hacer una copia de seguridad de las VM de Azure cifradas mediante Azure Disk Encryption (ADE). Azure Disk Encryption se integra con las claves de cifrado de BitLocker (BEK), que se protegen en un almacén de claves como secretos. Azure Disk Encryption también se integra con las claves de cifrado de las claves de Azure Key Vault (KEK). Habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Eliminación temporal para máquinas virtuales](../backup/soft-delete-virtual-machines.md)

- [Información general sobre la eliminación temporal de Azure Key Vault](../key-vault/general/soft-delete-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía**: desarrolle una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia. 

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Uso de la guía de control de incidentes de seguridad de equipos de NIST para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque las suscripciones con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente. 

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md) 

- [Uso de etiquetas para organizar los recursos de Azure](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las funcionalidades de respuesta a los incidentes de los sistemas periódicamente; así, ayudará a proteger los recursos de Azure. Identifique puntos débiles y brechas y después revise el plan de respuesta según sea necesario. 

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas. 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: exporte las alertas y recomendaciones de Azure Security Center y mediante la característica de exportación continua para ayudar a identificar riesgos en los recursos de Azure. La exportación continua permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para enviar las alertas a Azure Sentinel. 

- [Configuración de la exportación continua](../security-center/continuous-export.md) 

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: use la característica de automatización de flujos de trabajo de Azure Security Center para desencadenar automáticamente las alertas y recomendaciones de seguridad con el fin de proteger los recursos de Azure. 

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft. 

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
