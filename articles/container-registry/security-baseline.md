---
title: Base de referencia de seguridad de Azure para Container Registry
description: La base de referencia de seguridad de Container Registry proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: container-registry
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 795b938913dad149739c4591bc2a9c221d784c84
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101716386"
---
# <a name="azure-security-baseline-for-container-registry"></a>Base de referencia de seguridad de Azure para Container Registry

Esta línea base de seguridad aplica la guía de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md) en Container Registry. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa mediante los **controles de seguridad** definidos en Azure Security Benchmark y las directrices relacionadas aplicables a Container Registry. Se han excluido los **controles** no aplicables a Container Registry.

 
Para ver cómo Container Registry se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de la base de referencia de seguridad de Cognitive Services](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Azure Virtual Network proporciona seguridad, acceso de red seguro y privado para recursos locales y de Azure. Al limitar el acceso a su registro de contenedor de Azure privado desde una red virtual de Azure, se asegura de que solo acceden al registro los recursos de la red virtual. En los escenarios entre locales, también puede configurar reglas de firewall para permitir el acceso al registro solo desde determinadas direcciones IP.
Desde detrás de un firewall, configure reglas de acceso de firewall y etiquetas de servicio para tener acceso al registro de contenedor.

- [Restricción del acceso a un registro de contenedor de Azure mediante una red virtual de Azure o reglas de firewall](container-registry-vnet.md) 

- [Configuración de reglas para acceder a un registro de contenedor de Azure detrás de un firewall](container-registry-firewall-access-rules.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de [sus recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy - Microsoft.ContainerRegistry**:

[!INCLUDE [Resource Policy for Microsoft.ContainerRegistry 1.1](../../includes/policy/standards/asb/rp-controls/microsoft.containerregistry-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: Use Azure Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de red en Azure. Habilite los registros de flujo de NSG y envíe registros a una cuenta de almacenamiento para la auditoría del tráfico.

- [Habilitación de los registros de flujo del grupo de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Protección de los recursos de red](../security-center/security-center-network-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Habilite la protección contra DDoS estándar en las redes virtuales de Azure para protegerse frente a ataques DDoS. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas. Implemente Azure Firewall en cada uno de los límites de red de la organización con la inteligencia de amenazas habilitada y configurada para alertar y denegar el tráfico de red malintencionado.

Use el acceso de red Just-in-Time de Azure Security Center para configurar los NSG con el fin de limitar la exposición de los puntos de conexión a las direcciones IP aprobadas durante un período limitado.
Use también la protección de redes adaptativa de Azure Security Center para recomendar configuraciones de NSG que limiten los puertos y las direcciones IP basadas en el tráfico real y la inteligencia sobre amenazas.

- [Configuración de la protección contra DDoS](/azure/virtual-network/manage-ddos-protection)
- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)
- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](../security-center/security-center-alerts-service-layer.md)
- [Descripción de la protección de red adaptable de Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)
- [Control de acceso de red Just-in-Time de Azure Security Center](../security-center/security-center-just-in-time.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Habilite los registros de flujo del grupo de seguridad de red (NSG) conectado a la subred que se usa para proteger el registro de contenedor de Azure. Anote los registros de flujo del grupo de seguridad de red en una cuenta de Azure Storage para generar registros de flujo. Si es necesario para investigar actividades anómalas, habilite la captura de paquetes de Azure Network Watcher.

- [Habilitación de los registros de flujo del grupo de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y de detección de intrusiones (IDS/IPS) basados en la red.

**Guía**: Seleccione una oferta de Azure Marketplace que admita la funcionalidad de IDS/IPS con funcionalidades de inspección de carga. Si la detección y/o la prevención de intrusiones basadas en la inspección de carga no es un requisito, se puede usar Azure Firewall con la inteligencia sobre amenazas. El filtrado basado en inteligencia sobre amenazas de Azure Firewall puede alertar y denegar el tráfico desde y hacia los dominios y las direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

Implemente la solución de firewall que prefiera en cada uno de los límites de red de su organización para detectar y/o denegar el tráfico malintencionado.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall) 

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: En el caso de los recursos que necesitan acceso al registro de contenedor, use las etiquetas de servicio de red virtual para Azure Container Registry con el fin de definir los controles de acceso a la red en grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio "AzureContainerRegistry" en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Permitir el acceso por etiqueta de servicio](https://docs.microsoft.com/azure/container-registry/container-registry-firewall-access-rules#allow-access-by-service-tag)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos de red asociados a los registros de contenedor de Azure con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.ContainerRegistry" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los registros de contenedor. 

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de Azure RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de Azure RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Instrucciones**: Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con los grupos de los registros de contenedor. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Ingiera registros a través de Azure Monitor para agregar datos de seguridad generados por el registro de contenedor de Azure. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y utilice cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Azure Monitor recopila registros de recursos (anteriormente denominados registros de diagnóstico) de eventos controlados por el usuario en el registro. Recopile y consuma estos datos para auditar los eventos de autenticación del registro y proporcionar un seguimiento de la actividad completo en los artefactos del registro, como los eventos de extracción e inserción, para que pueda diagnosticar problemas de seguridad con el registro.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: No aplicable. La prueba comparativa está pensada para recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: En Azure Monitor, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

- [Configuración de parámetros de retención de registros de áreas de trabajo de Log Analytics](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Analice y supervise los registros de Azure Container Registry en busca de comportamientos anómalos y revise los resultados con regularidad. Use el área de trabajo de Log Analytics de Azure Monitor para revisar los registros y realizar consultas en los datos del registro.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

- [Descripción del área de trabajo de Log Analytics](/azure/azure-monitor/log-query/get-started-portal)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Instrucciones**: Use el área de trabajo de Azure Log Analytics para supervisar los registros de seguridad y eventos relacionados con el registro de contenedor de Azure y generar alertas sobre actividades anómalas en ellos.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

- [Alertas sobre datos de registro de Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: No aplicable. Azure Container Registry no procesa ni genera registros relacionados con antimalware.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: No aplicable. Azure Container Registry es un punto de conexión y no inicia la comunicación, además el servicio no consulta DNS.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: No aplicable. La prueba comparativa está pensada para recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Azure Active Directory (Azure AD) tiene roles integrados que se deben asignar explícitamente y son consultables. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc para detectar cuentas que son miembros de grupos administrativos.

Para cada registro de contenedor de Azure, realice un seguimiento para saber si la cuenta de administrador integrada está habilitada o deshabilitada. Deshabilite la cuenta cuando no esté en uso.

- [Obtención de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

- [Cuenta de administrador de Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Active Directory (Azure AD) no tiene el concepto de contraseñas predeterminadas. Otros recursos de Azure que requieren una contraseña obligan a crear una contraseña con requisitos de complejidad y una longitud mínima, que cambia en función del servicio. Es responsable de las aplicaciones de terceros y de los servicios de Marketplace que pueden usar contraseñas predeterminadas.

Si la cuenta de administrador predeterminada de un registro de contenedor de Azure está habilitada, las contraseñas complejas se crean automáticamente y deben girarse. Deshabilite la cuenta cuando no esté en uso.

- [Cuenta de administrador de Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, cree procedimientos para habilitar la cuenta de administrador integrada de un registro de contenedor. Deshabilite la cuenta cuando no esté en uso.

- [Descripción de identidad y acceso en Azure Security Center](../security-center/security-center-identity-access.md)

- [Cuenta de administrador de Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Siempre que sea posible, use el inicio de sesión único de Azure Active Directory (Azure AD) en lugar de configurar credenciales independientes individuales por servicio. Use las recomendaciones de administración de identidades y acceso de Azure Security Center.

Para obtener acceso individual al registro de contenedor, use el inicio de sesión individual integrado con Azure AD.

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Inicio de sesión individual en un registro de contenedor](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: Utilice estaciones de trabajo de acceso con privilegios (PAW) que tengan configurada la autenticación multifactor para iniciar sesión en los recursos de Azure y configurarlos.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)
- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: Use los informes de seguridad de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzcan actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

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

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring/)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: Puede usar orígenes de registro de actividad de inicio de sesión de Azure Active Directory (Azure AD), auditoría y riesgos para la supervisión, lo que permite realizar la integración con cualquier herramienta de administración de eventos e información de seguridad (SIEM) y de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a una área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use las características de protección de identidad y detección de riesgo de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas que se detecten relacionadas con las identidades de los usuarios. 

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Instrucciones**: La Caja de seguridad del cliente todavía no se admite en Azure Container Registry.

- [Lista de servicios admitidos por la Caja de seguridad del cliente](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: Use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

Imágenes de contenedor de etiquetas y versiones u otros artefactos en un registro, y bloquear imágenes o repositorios, para ayudar a realizar el seguimiento de las imágenes que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

- [Recomendaciones para el etiquetado y el control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md)

- [Bloqueo de una imagen de contenedor en un registro de contenedor de Azure](container-registry-image-lock.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía**: Implemente registros de contenedor separados, suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Los recursos que almacenan o procesan datos confidenciales deben estar suficientemente aislados.

Los recursos deben separarse mediante una red virtual o subred, etiquetarse adecuadamente y protegerse con grupos de seguridad de red (NSG) o Azure Firewall.

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

- [Restricción del acceso a un registro de contenedor de Azure mediante una red virtual de Azure o reglas de firewall](container-registry-vnet.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas o alertas y denegación con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: Implemente una herramienta automatizada en los perímetros de red que supervise la transferencia no autorizada de información confidencial y bloquee dichas transferencias al tiempo que alerta a los profesionales de seguridad de la información.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Instrucciones**: Asegúrese de que los clientes que se conectan a Azure Container Registry pueden negociar un TLS 1.2 o superior. De forma predeterminada, los recursos de Microsoft Azure negociarán TLS 1.2.

Siga las recomendaciones de Azure Security Center para el cifrado en reposo y el cifrado en tránsito, si procede.

- [Descripción del cifrado en tránsito con Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: Las características de identificación, clasificación y prevención de pérdida de datos no están disponibles aún en Azure Container Registry. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Use el control de acceso basado en rol de Azure (Azure RBAC) para controlar el acceso a los datos y recursos de un registro de contenedor de Azure. 

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Roles y permisos de Azure Container Registry](container-registry-roles.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: Si se requiere, implemente una herramienta de terceros para el cumplimiento de los recursos de proceso, como una solución de prevención de pérdida de datos basada en host automatizada, para aplicar controles de acceso a los datos incluso cuando se copian datos de un sistema.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Use el cifrado en reposo en todos los recursos de Azure. De forma predeterminada, todos los datos de un registro de contenedor de Azure se cifran en reposo mediante claves administradas por Microsoft.

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md)

- [Claves administradas por el cliente en Azure Container Registry](https://aka.ms/acr/cmk)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Azure Monitor recopila registros de recursos (anteriormente denominados registros de diagnóstico) de eventos controlados por el usuario en el registro. Recopile y consuma estos datos para auditar los eventos de autenticación del registro y proporcionar un seguimiento de la actividad completo en los artefactos del registro, como los eventos de extracción e inserción, para que pueda diagnosticar problemas de seguridad con el registro.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en sus imágenes de contenedor. Opcionalmente, puede implementar soluciones de terceros de Azure Marketplace para realizar evaluaciones de vulnerabilidades de imagen.

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](/azure/security-center/security-center-vulnerability-assessment-recommendations)

- [Integración de Azure Container Registry con Security Center (versión preliminar)](/azure/security-center/azure-container-registry-integration)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Guía**: Microsoft realiza la administración de revisiones en los sistemas subyacentes que admiten Azure Container Registry.

Automatización de las actualizaciones de imágenes de contenedor cuando se detectan actualizaciones de las imágenes base del sistema operativo y otras revisiones.

- [Acerca de las actualizaciones de imagen base para ACR Tasks](container-registry-tasks-base-images.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía**: Puede usar una solución de terceros para aplicar revisiones a las imágenes de la aplicación.  Además, puede ejecutar tareas de Azure Container Registry para automatizar las actualizaciones de las imágenes de la aplicación en un registro de contenedor basado en revisiones de seguridad u otras actualizaciones de imágenes base.

- [Acerca de las actualizaciones de imagen base para ACR Tasks](container-registry-tasks-base-images.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Integre Azure Container Registry (ACR) con Azure Security Center para habilitar el análisis periódico de las imágenes de contenedor para detectar vulnerabilidades. Opcionalmente, puede implementar soluciones de terceros de Azure Marketplace para realizar evaluaciones de vulnerabilidades de imagen.

- [Integración de Azure Container Registry con Security Center (versión preliminar)](/azure/security-center/azure-container-registry-integration)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Instrucciones**: Integre Azure Container Registry (ACR) con Azure Security Center para habilitar el análisis periódico de las imágenes de contenedor con el fin de detectar vulnerabilidades y clasificar riesgos. Opcionalmente, puede implementar soluciones de terceros de Azure Marketplace para realizar evaluaciones de vulnerabilidades de imagen y clasificación de riesgos.

- [Integración de Azure Container Registry con Security Center (versión preliminar)](/azure/security-center/azure-container-registry-integration)

**Responsabilidad**: Customer

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

**Guía**: Azure Container Registry mantiene metadatos, como etiquetas y manifiestos para las imágenes de un registro. Siga las prácticas recomendadas para etiquetar artefactos.

- [Acerca de los registros, repositorios e imágenes](container-registry-concepts.md)

- [Recomendaciones para el etiquetado y el control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Azure Container Registry mantiene metadatos, como etiquetas y manifiestos para las imágenes de un registro. Siga las prácticas recomendadas para etiquetar artefactos.

- [Acerca de los registros, repositorios e imágenes](container-registry-concepts.md)

- [Recomendaciones para el etiquetado y el control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Tendrá que crear un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones.

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Analice y supervise los registros de Azure Container Registry en busca de comportamientos anómalos y revise los resultados con regularidad. Use el área de trabajo de Log Analytics de Azure Monitor para revisar los registros y realizar consultas en los datos del registro.

- [Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico](container-registry-diagnostics-audit-logs.md)

- [Descripción del área de trabajo de Log Analytics](/azure/azure-monitor/log-query/get-started-portal)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: Azure Automation proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos. Puede implementar su propia solución para quitar un software no autorizado.

- [Introducción a Azure Automation](../automation/automation-intro.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: No aplicable. La prueba comparativa está pensada para recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Instrucciones**: Use Azure Policy para restringir qué servicios puede aprovisionar en su entorno.

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía**: No aplicable. La prueba comparativa está pensada para recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Use configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitación de capacidad de los usuarios para ejecutar scripts en recursos de proceso

**Guía**: Use configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure.

- [Por ejemplo, cómo controlar la ejecución de scripts de PowerShell en entornos Windows](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: El software que se requiere para las operaciones empresariales, pero que puede suponer un riesgo mayor para la organización, debe aislarse en su propia máquina virtual o red virtual y estar lo suficientemente protegida con Azure Firewall o un grupo de seguridad de red.

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Use Azure Policy o Azure Security Center para mantener las configuraciones de seguridad para todos los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: Use la recomendación de Azure Security Center "Remediate Vulnerabilities in Security Configurations on your Virtual Machines (Corrija las vulnerabilidades en las configuraciones de seguridad de sus máquinas virtuales)" para mantener las configuraciones de seguridad en todos los recursos de proceso.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md)

- [Cómo corregir las recomendaciones de Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use la directiva de Azure Policy [deny] y [deploy if not exist] para aplicar la configuración segura en los recursos de Azure.

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: No aplicable. La prueba comparativa está pensada para recursos de proceso.

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Si usa definiciones de personalizadas de Azure Policy, use Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: No aplicable. La prueba comparativa se aplica principalmente a los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Instrucciones**: Use Azure Policy para alertar, auditar y aplicar las configuraciones del sistema. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Instrucciones**: No aplicable. La prueba comparativa se aplica principalmente a los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use Azure Security Center para realizar análisis de base de referencia para los recursos de Azure.

Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones.

- [Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

- [Auditoría del cumplimiento de los registros de contenedor de Azure mediante Azure Policy](container-registry-azure-policy.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Instrucciones**: No aplicable. La prueba comparativa se aplica principalmente a los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones**: Use Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos para las aplicaciones en la nube.

- [Integración con identidades administradas de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Creación de un almacén de claves](../key-vault/general/quick-create-portal.md)

- [Autenticación en Azure Key Vault](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

- [Uso de una identidad administrada con las tareas de Azure Container Registry](container-registry-tasks-authentication-managed-identity.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Instrucciones**: Use identidades administradas para proporcionar servicios de Azure con una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluyendo Key Vault, sin necesidad de credenciales en el código.

- [Configuración de las identidades administradas](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md)

- [Use la identidad administrada de Azure para autenticarse en un registro de contenedor de Azure](container-registry-authentication-managed-identity.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: Use Microsoft Antimalware para Azure Cloud Services y Virtual Machines para supervisar y defender continuamente sus recursos. Para Linux, use una solución antimalware de terceros.

- [Configuración de Microsoft Antimalware para Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Instrucciones**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Container Registry), pero no se ejecuta en el contenido del cliente.

Examine previamente los archivos que se cargan en recursos de Azure que no son de proceso, como App Service, Data Lake Storage, Blob Storage, etc.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Asegúrese de que se han actualizado el software y las firmas antimalware

**Instrucciones**: No aplicable. La prueba comparativa está pensada para recursos de proceso. Microsoft controla el antimalware de la plataforma subyacente.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Instrucciones**: Los datos de su cuenta de almacenamiento de Microsoft Azure se replican siempre para garantizar la durabilidad y la alta disponibilidad. Azure Container Registry copia los datos para protegerse de los eventos planeados y no planeados

Opcionalmente, puede replicar geográficamente un registro de contenedor para mantener las réplicas del registro en varias regiones de Azure. 

- [Replicación geográfica en Azure Container Registry](container-registry-geo-replication.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copias de seguridad de las claves administradas por el cliente

**Instrucciones**: Opcionalmente, haga una copia de seguridad de las imágenes de contenedor importando de un registro a otro.

Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault con las herramientas de línea de comandos o los SDK de Azure.

- [Importación de imágenes de contenedor en un registro de contenedor](container-registry-import-images.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault con las herramientas de línea de comandos o los SDK de Azure.

- [Restauración de las claves de Azure Key Vault en Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Habilite la eliminación temporal en Azure Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Habilitación de la eliminación temporal en Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

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

**Instrucciones**: use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de afiliación de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft: https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

- [Aquí encontrará más información sobre la estrategia y puesta en marcha de un equipo rojo de Microsoft, y las pruebas de penetración en sitios activos de la infraestructura en la nube, las aplicaciones y los servicios administrados por Microsoft](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
