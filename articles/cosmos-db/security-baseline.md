---
title: Línea de base de seguridad de Azure para Azure Cosmos DB
description: La línea de base de seguridad de Azure Cosmos DB proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 5f9cf6dba0a1e670e3a27cb3546de9237466e7ce
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101659433"
---
# <a name="azure-security-baseline-for-azure-cosmos-db"></a>Línea de base de seguridad de Azure para Azure Cosmos DB

Esta línea de base de seguridad aplica las instrucciones de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md) a Azure Cosmos DB. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices aplicables a Azure Cosmos DB. Se han excluido los **controles** no aplicables a Azure Cosmos DB.

 
Para ver cómo Azure Cosmos DB se adapta por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure Cosmos DB](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Al usar Azure Private Link, puede conectarse a una cuenta de Azure Cosmos a través de un punto de conexión privado. El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft, eliminando la exposición a la red pública de Internet. También puede reducir el riesgo de la exfiltración de datos mediante la configuración de un conjunto estricto de reglas de salida en un grupo de seguridad de red (NSG) y la Asociación de ese NSG con la subred.

También puede usar puntos de conexión de servicio para proteger su cuenta de Azure Cosmos. Al habilitar un punto de conexión de servicio, puede configurar las cuentas de Azure Cosmos DB para permitir el acceso solo desde una subred específica de una red virtual de Azure. Una vez habilitado el punto de conexión de servicio de Azure Cosmos DB, puede limitar el acceso a la cuenta de Azure Cosmos con conexiones de una subred en una red virtual.

Puede proteger los datos almacenados en su cuenta de Azure Cosmos mediante el uso de firewalls por IP. Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Puede establecer firewall por IP en la cuenta de Azure Cosmos mediante Azure Portal, plantillas de Azure Resource Manager, o a través de la CLI de Azure o Azure PowerShell.

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

- [Cómo configurar un punto de conexión privado para Azure Cosmos DB](how-to-configure-private-endpoints.md) 

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Configuración del firewall de IP en Azure Cosmos DB](how-to-configure-firewall.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: [Azure Security Benchmark](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) es la iniciativa de directiva predeterminada de Security Center y es la base de sus [recomendaciones](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Security Center habilita automáticamente las definiciones de Azure Policy relacionadas con este control. Puede que las alertas relacionadas con este control requieran un plan de [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) para los servicios relacionados.

**Definiciones integradas de Azure Policy: Microsoft.DocumentDB**:

[!INCLUDE [Resource Policy for Microsoft.DocumentDB 1.1](../../includes/policy/standards/asb/rp-controls/microsoft.documentdb-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Instrucciones**: Use Azure Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de red relacionados a la cuenta de Azure Cosmos.

Cuando se implementan máquinas virtuales en la misma red virtual que la cuenta de Azure Cosmos, puede usar los grupos de seguridad de red (NSG) para reducir el riesgo de exfiltración de datos. Habilite los registros de flujo de grupos de seguridad de red y envíe registros a una cuenta de Azure Storage para la auditoría del tráfico. También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: El uso compartido de recursos entre orígenes (CORS) es una característica de HTTP que permite que una aplicación web que se ejecuta en un dominio tenga acceso a recursos de otro dominio. Los exploradores web implementan una restricción de seguridad que se conoce como directiva de mismo origen que impide que una página web realice llamadas API en un dominio diferente. Sin embargo, CORS aporta un modo seguro de permitir que el dominio de origen llame a las API de otro dominio. Después de habilitar la compatibilidad con CORS para la cuenta de Azure Cosmos, solo se evalúan las solicitudes autenticadas para determinar si se admiten según las reglas especificadas.

- [Configuración del uso compartido de recursos entre orígenes (CORS)](how-to-configure-cross-origin-resource-sharing.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Use Advanced Threat Protection (ATP) para Azure Cosmos DB. Advanced Threat Protection para Azure Cosmos DB proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de Azure Cosmos DB o vulnerarlas. Esta capa de protección le permite afrontar las amenazas e integrarlas con sistemas centrales de supervisión de seguridad.

Habilite DDoS Protection estándar en las redes virtuales asociadas a las instancias de Azure Cosmos DB a fin de protegerse de los ataques DDoS. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas.

- [Cómo configurar Advanced Threat Protection para Azure Cosmos DB](cosmos-db-advanced-threat-protection.md)

- [Configuración de la protección contra DDoS](/azure/virtual-network/manage-ddos-protection)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](/azure/security-center/security-center-alerts-service-layer)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Habilite los registros de flujo del grupo de seguridad de red (NSG) y envíelos a una cuenta de Storage para auditar el tráfico. También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar conclusiones cognitivas sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: Use Advanced Threat Protection (ATP) para Azure Cosmos DB. Advanced Threat Protection para Azure Cosmos DB proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de Azure Cosmos DB o vulnerarlas. Esta capa de protección le permite afrontar las amenazas e integrarlas con sistemas centrales de supervisión de seguridad. 

- [Cómo configurar Advanced Threat Protection para Azure Cosmos DB](cosmos-db-advanced-threat-protection.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Instrucciones**: En el caso de los recursos que necesitan acceso a la cuenta de Azure Cosmos, use las etiquetas de servicio de Virtual Network con el fin de definir controles de acceso a la red en grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, AzureCosmosDB) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Más información sobre el uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos de red con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.DocumentDB" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de las instancias de Azure Cosmos DB. También puede usar definiciones de directivas integradas para Azure Cosmos DB, como:

- Implementar Advanced Threat Protection para cuentas de Cosmos DB

- Cosmos DB debe usar un punto de conexión de servicio de red virtual

También puede utilizar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como las plantillas de Azure Resource Manager, el control de acceso basado en roles de Azure (Azure RBAC) y las directivas, en una única definición de un plano técnico. Aplique fácilmente el plano técnico a nuevas suscripciones y entornos, y ajuste el control y la administración mediante el control de versiones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Instrucciones**: Use etiquetas para los recursos de red asociados a la implementación de Azure Cosmos DB a fin de organizarlos lógicamente en una taxonomía.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Instrucciones**: Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con las instancias de Azure Cosmos DB. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos. 

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Ingiera registros a través de Azure Monitor para agregar datos de seguridad generados por Azure Cosmos DB. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de almacenamiento para el almacenamiento de archivos a largo plazo. Como alternativa, puede incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros. 

- [Cómo habilitar los registros de diagnóstico para Azure Cosmos DB](/azure/cosmos-db/logging)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: habilite la configuración de diagnóstico para Azure Cosmos DB y envíe los registros a la misma área de trabajo de Log Analytics o cuenta de almacenamiento de Azure. La configuración de diagnóstico de Azure Cosmos DB se usa para recopilar los registros de los recursos. Los registros se capturan por solicitud y también se denominan **registros de plano de datos**. Algunos ejemplos de las operaciones del plano de datos son Delete, Insert y Read. 

También puede habilitar la configuración de diagnóstico del registro de actividad de Azure y enviar los registros a la misma área de trabajo de Log Analytics que se usa para los registros de Azure Cosmos DB.

- [Cómo habilitar la configuración de diagnóstico para Azure Cosmos DB](/azure/cosmos-db/logging)

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](/azure/azure-monitor/platform/diagnostic-settings-legacy)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: En Azure Monitor, establezca el período de retención de registro para las áreas de trabajo de Log Analytics asociadas a las instancias de Azure Cosmos DB según las normativas de cumplimiento de su organización.

- [Establecimiento de parámetros de retención de registros](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Realice consultas en el área de trabajo de Log Analytics para buscar términos, identificar tendencias, analizar patrones y proporcionar muchas otras conclusiones basadas en los datos del registro de actividad que se pueden recopilar para Azure Cosmos DB.

- [Cómo realizar consultas en áreas de trabajo de Log Analytics para Azure Cosmos DB](monitor-cosmos-db.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: En Azure Security Center, habilite protección contra amenazas avanzada para Azure Cosmos DB para supervisar actividades anómalas en registros y eventos de seguridad. Habilite la configuración de diagnóstico en Azure Cosmos DB y envíe registros a un área de trabajo de Log Analytics.

 

También puede incorporar el área de trabajo de Log Analytics a Azure Sentinel, ya que proporciona una solución de respuesta automatizada de orquestación de seguridad (SOAR). Esto permite crear cuadernos de estrategias (soluciones automatizadas) y usarlos para corregir problemas de seguridad. Además, puede crear alertas de registro personalizadas en el área de trabajo Log Analytics mediante Azure Monitor.

- [Lista de alertas de protección contra amenazas para Azure Cosmos DB](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Creación, visualización y administración de alertas de registro mediante Azure Monitor](/azure/azure-monitor/platform/alerts-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Instrucciones**: Puede usar el panel de identidades y control de acceso (IAM) en el Azure Portal para configurar el control de acceso basado en rol (RBAC) y mantener el inventario en los recursos de Azure Cosmos DB. Los roles se aplican a usuarios, grupos, entidades de servicio e identidades administradas en Azure Active Directory (Azure AD). Puede usar roles integrados o personalizados para usuarios y grupos.

Azure Cosmos DB proporciona un control de acceso basado en roles de Azure integrado para escenarios de administración comunes en Azure Cosmos DB. Un usuario que tiene un perfil en Azure AD puede asignar estos roles de Azure a usuarios, grupos, entidades de servicio o identidades administradas para conceder o denegar el acceso a recursos y operaciones en los recursos de Azure Cosmos DB.

Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

Además, algunas acciones de Azure Cosmos DB se pueden controlar con Azure AD y claves maestras específicas de la cuenta. Use la configuración de la cuenta "disableKeyBasedMetadataWriteAccess" para controlar el acceso a las claves.

- [Información acerca del control de acceso basado en roles de Azure Cosmos DB](role-based-access-control.md)

- [Creación de sus propios roles personalizados con acciones de Azure Cosmos DB (espacio de nombres Microsoft.DocumentDB)](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdocumentdb)

- [Creación de un nuevo rol en Azure AD](../role-based-access-control/custom-roles.md)

- [Obtención de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

- [Restricción del acceso de los usuarios solo a las operaciones de datos](how-to-restrict-user-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: el concepto de contraseñas en blanco o predeterminadas no existe en relación con Azure Active Directory (Azure AD) o Azure Cosmos DB. En su lugar, Azure Cosmos DB usa dos tipos de claves para autenticar usuarios y proporcionar acceso a sus datos y recursos; claves maestras y token de recurso. Las claves se pueden regenerar en cualquier momento.

- [Información sobre la protección del acceso a los datos en Azure Cosmos DB](secure-access-to-data.md)

- [Cómo regenerar las claves de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-with-powershell#regenerate-keys)

- [Cómo obtener acceso a las claves mediante programación con Azure AD](certificate-based-authentication.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Instrucciones**: No aplicable; Azure Cosmos DB no es compatible con las cuentas de administrador. Todo el acceso se integra con Azure Active Directory (Azure AD) y el control de acceso basado en roles (Azure RBAC) de Azure.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Azure Cosmos DB usa dos tipos de claves para autenticar a los usuarios y no admite el inicio de sesión único (SSO) en el nivel del plano de datos. El acceso al plano de control de Azure Cosmos DB está disponible mediante la API REST y admite SSO. Para autenticarse, establezca el encabezado de autorización de las solicitudes en JSON Web Token, que se obtiene de Azure Active Directory (Azure AD).

- [Información sobre la API REST de Azure Database para Cosmos DB](/rest/api/cosmos-db/)

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

**Guía**: Utilice estaciones de trabajo de acceso con privilegios (PAW) que tengan configurada la autenticación multifactor para iniciar sesión en los recursos de Azure y configurarlos. 

- [Más información sobre las estaciones de trabajo con privilegios de acceso](/security/compass/privileged-access-devices)
 
- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: Use Advanced Threat Protection (ATP) para Azure Cosmos DB. Advanced Threat Protection para Azure Cosmos DB proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de Azure Cosmos DB o vulnerarlas. Esta capa de protección le permite afrontar las amenazas e integrarlas con sistemas centrales de supervisión de seguridad.

Así mismo, puede usar Azure Active Directory (Azure AD) Privileged Identity Management (PIM) para la generación de registros y alertas cuando se produzca actividad sospechosa o insegura en el entorno.

Use las detecciones de riesgo de Azure AD para ver alertas e informes sobre el comportamiento de los usuarios de riesgo.

- [Cómo implementar Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Información sobre las detecciones de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risk-events)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Instrucciones**: Configure la condición de ubicación de una directiva de acceso condicional y administre las ubicaciones con nombre. Gracias a las ubicaciones con nombre, puede crear agrupaciones lógicas de rangos de direcciones IP o de países y regiones. Puede restringir el acceso a recursos confidenciales, como las instancias de Azure Cosmos DB, a las ubicaciones con nombre configuradas.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory-domain-services/tutorial-create-instance.md)

- [Configuración y administración de la autenticación de Azure AD con Azure SQL](/azure/sql-database/sql-database-aad-authentication-configure)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones**: Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, puede utilizar las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: puede simplificar este proceso creando una configuración de diagnóstico para las cuentas de usuario de Azure Active Directory (Azure AD) y enviando los registros de auditoría y de inicio de sesión a un área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use Advanced Threat Protection (ATP) para Azure Cosmos DB. Advanced Threat Protection para Azure Cosmos DB proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de Azure Cosmos DB o vulnerarlas. Esta capa de protección le permite afrontar las amenazas e integrarlas con sistemas centrales de supervisión de seguridad.

También puede usar las características de Azure Active Directory (Azure AD) Identity Protection y detección de riesgo para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. Además, puede ingerir datos en Azure Sentinel para investigarlos más a fondo.

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: Use etiquetas para ayudar a realizar el seguimiento de las instancias de Azure Cosmos DB que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Las instancias de Azure Cosmos DB deben separarse mediante una red virtual o subred, etiquetarse adecuadamente y protegerse con grupos de seguridad de red (NSG) o Azure Firewall. Las instancias de Azure Cosmos DB que almacenan datos confidenciales deben estar aisladas. Al usar Azure Private Link, puede conectarse a una cuenta de instancia de Azure Cosmos DB a través de un punto de conexión privado. El punto de conexión privado es un conjunto de direcciones IP privadas en una subred dentro de la red virtual. Puede limitar el acceso a las direcciones IP privadas seleccionadas. 

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

- [Cómo configurar un punto de conexión privado para Azure Cosmos DB](how-to-configure-private-endpoints.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: Puede implementar Advanced Threat Protection para Azure Cosmos DB que detecta actividades anómalas que indican intentos inusuales y potencialmente maliciosos de acceso o ataque a las bases de datos. Actualmente, puede desencadenar las siguientes alertas:

- Acceso desde ubicaciones inusuales

- Extracción de datos inusual

De manera adicional, al usar máquinas virtuales para tener acceso a las instancias de Azure Cosmos DB, aproveche Private Link, el firewall, los grupos de seguridad de red y las etiquetas de servicio para mitigar la posibilidad de que se produzca una filtración de datos. Microsoft administra la infraestructura subyacente para Azure Cosmos DB y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

- [Cómo configurar Advanced Threat Protection para Azure Cosmos DB](cosmos-db-advanced-threat-protection.md)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: Los datos automáticos de identificación, clasificación y prevención de pérdida de datos no están disponibles aún en Azure Cosmos DB. Sin embargo, puede usar la integración de Azure Cognitive Search para la clasificación y el análisis de datos. También puede implementar una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Indexación de datos de Azure Cosmos DB con Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb?toc=/azure/cosmos-db/toc.json&amp;bc=/azure/cosmos-db/breadcrumb/toc.json)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Azure Cosmos DB proporciona un control de acceso basado en roles de Azure (RBAC de Azure) integrado para escenarios de administración comunes en Azure Cosmos DB. Un usuario que tiene un perfil en Azure Active Directory (Azure AD) puede asignar estos roles de Azure a usuarios, grupos, entidades de servicio o identidades administradas para conceder o denegar el acceso a recursos y operaciones en los recursos de Azure Cosmos DB. Las asignaciones de roles están dirigidas únicamente al acceso al plano de control, que incluye el acceso a las cuentas, bases de datos, contenedores y ofertas de Azure Cosmos (rendimiento).

- [Cómo implementar RBAC de Azure en Azure Cosmos DB](role-based-access-control.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Todos los datos almacenados en Azure Cosmos DB se cifran en reposo. No hay ningún control para activarlo o desactivarlo. Azure Cosmos DB usa el cifrado de AES-256 en todas las regiones donde se ejecuta la cuenta.

de manera predeterminada, Microsoft administra las claves que se usan para cifrar los datos de la cuenta de Azure Cosmos. también puede optar por agregar una segunda capa de cifrado con sus propias claves.

- [Información sobre el cifrado en reposo con Azure Cosmos DB](database-encryption-at-rest.md)

- [Información sobre la administración de claves para el cifrado en reposo con Azure Cosmos DB](/azure/cosmos-db/cosmos-db-security-controls)

- [Cómo configurar las claves administradas por el cliente para la cuenta de Azure Cosmos DB](how-to-setup-cmk.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Instrucciones**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en las instancias de Azure Cosmos DB.

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Instrucciones**: Siga las recomendaciones de Azure Security Center para las instancias de Azure Cosmos DB. 

Microsoft realiza revisiones del sistema y administración de vulnerabilidades en los hosts subyacentes que admiten las instancias de Azure Cosmos DB. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Características compatibles disponibles en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use el Azure Portal o el Azure Resource Graph para detectar todos los recursos (no limitados a Azure Cosmos DB, sino también recursos como proceso, otro almacenamiento, red, puertos y protocolos, etc.) dentro de sus suscripciones. Asegúrese de que tiene los permisos adecuados en el suscriptor y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Información sobre el control de acceso basado en rol de Azure](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a las instancias de Azure Cosmos DB y recursos relacionados que proporcionen metadatos para organizarlos lógicamente en una taxonomía.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

- [Qué recursos de Azure Cosmos DB admiten etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-support#microsoftdocumentdb)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento, incluido pero no limitado a los recursos de Azure Cosmos DB. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar y detectar recursos en las suscripciones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones**: Use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación Microsoft Azure Management. Esto puede impedir la creación y los cambios en los recursos de un entorno de alta seguridad.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Instrucciones**: Defina e implemente configuraciones de seguridad estándar para las instancias de Azure Cosmos DB con Azure Policy. Use los alias de Azure Policy en el espacio de nombres **Microsoft.DocumentDB** para crear directivas personalizadas con el fin de auditar o aplicar la configuración de las instancias de Cosmos DB. También puede usar definiciones de directivas integradas para Azure Cosmos DB, como:
- Implementar Advanced Threat Protection para cuentas de Cosmos DB
- Cosmos DB debe usar un punto de conexión de servicio de red virtual

- [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use la directiva de Azure Policy [deny] y [deploy if not exist] para aplicar la configuración segura en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: si usa definiciones personalizadas de Azure Policy para los recursos de Cosmos DB u otros relacionados, use Azure Repos para almacenar y administrar las definiciones de directivas personalizadas mientras programa.

- [Diseño de flujos de trabajo de directiva como código](../governance/policy/concepts/policy-as-code.md)

- [Documentación de Azure Repos](/azure/devops/repos/git/gitworkflow)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: Use los alias de Azure Policy en el espacio de nombres "Microsoft.DocumentDB" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use los alias de Azure Policy en el espacio de nombres "Microsoft.DocumentDB" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Use [audit], [deny] y [deploy if not exist] de Azure Policy para aplicar automáticamente las configuraciones de las instancias de Azure Cosmos DB y recursos relacionados. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**: En el caso de las máquinas virtuales de Azure o las aplicaciones web que se ejecutan en Azure App Service y se usan para tener acceso a las instancias de Azure Cosmos DB, utilice Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos de Azure Cosmos DB. Asegúrese de que la eliminación temporal de Key Vault esté habilitada.

- [Integración con identidades administradas de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Creación de un almacén de claves](/azure/key-vault/quick-create-portal)

- [Autenticación en Azure Key Vault](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: En el caso de las máquinas virtuales de Azure o las aplicaciones web que se ejecutan en Azure App Service y se usan para tener acceso a las instancias de Azure Cosmos DB, utilice Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos de Azure Cosmos DB.

Use identidades administradas para proporcionar servicios de Azure con una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluyendo Key Vault, sin necesidad de credenciales en el código.

- [Configuración de las identidades administradas](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

- [Integración con identidades administradas de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Cómo configurar Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Cosmos DB), pero no se ejecuta en el contenido del cliente.

Es su responsabilidad realizar un análisis previo de los archivos que se cargan en recursos de Azure que no son de proceso, incluido Azure Cosmos DB. Microsoft no puede acceder a los datos del cliente y, por lo tanto, no puede realizar exámenes antimalware del contenido del cliente en su nombre.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: Azure Cosmos DB crea automáticamente copias de seguridad de los datos a intervalos regulares. En caso de que elimine accidentalmente la base de datos o un contenedor, puede presentar una incidencia de soporte técnico o llamar al servicio de soporte técnico de Azure para restaurar los datos a partir de copias de seguridad en línea automáticas. El soporte técnico de Azure está disponible solo para algunos planes, por ejemplo, Estándar o Desarrollador, y planes superiores. Para restaurar una instantánea específica de la copia de seguridad, Azure Cosmos DB requiere que los datos estén disponibles durante el ciclo de copia de seguridad de esa instantánea. 

Si usa Key Vault para almacenar las credenciales de las instancias de Cosmos DB, asegúrese de realizar copias de seguridad automatizadas de las claves.

- [Información sobre las copias de seguridad automatizadas de Azure Cosmos DB](online-backup-and-restore.md)

- [Cómo restaurar datos en Azure Cosmos DB](/azure/cosmos-db/how-to-backup-and-restore)

- [Procedimiento para realizar copias de seguridad de claves de Key Vault](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: En caso de que elimine accidentalmente la base de datos o un contenedor, puede presentar una incidencia de soporte técnico o llamar al servicio de soporte técnico de Azure para restaurar los datos a partir de copias de seguridad en línea automáticas. El soporte técnico de Azure está disponible solo para algunos planes, por ejemplo, Estándar o Desarrollador, y planes superiores. Para restaurar una instantánea específica de la copia de seguridad, Azure Cosmos DB requiere que los datos estén disponibles durante el ciclo de copia de seguridad de esa instantánea.

Pruebe la restauración de los secretos almacenados en Azure Key Vault con PowerShell. El cmdlet restore-AzureKeyVaultKey crea una clave en el almacén de claves especificado. Esta clave es una réplica de la clave de copia de seguridad en el archivo de entrada y tiene el mismo nombre que la clave original.

- [Información sobre las copias de seguridad automatizadas de Azure Cosmos DB](online-backup-and-restore.md)

- [Cómo restaurar datos en Azure Cosmos DB](/azure/cosmos-db/how-to-backup-and-restore)

- [Cómo restaurar secretos de Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Instrucciones**: Dato que todos los datos de usuario almacenados en Cosmos DB están cifrados en reposo y en tránsito, no es necesario hacer nada. Otra forma de decirlo es que el cifrado en reposo está "activado" de forma predeterminada. No hay ningún mando para activarlo o desactivarlo. Azure Cosmos DB usa el cifrado de AES-256 en todas las regiones donde se ejecuta la cuenta.

Habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Información sobre el cifrado de datos en Azure Cosmos DB](database-encryption-at-rest.md)

- [Habilitación de la eliminación temporal en Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [También puede utilizar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

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

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft. 

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
