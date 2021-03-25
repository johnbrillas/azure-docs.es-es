---
title: Línea de base de seguridad de Azure para HDInsight
description: La línea de base de seguridad de HDInsight proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: bf2360bda55735aa8ef4258da5ae47f673f4d71b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738929"
---
# <a name="azure-security-baseline-for-hdinsight"></a>Línea de base de seguridad de Azure para HDInsight

Esta línea de base de seguridad aplica las instrucciones de [Azure Security Benchmark, versión 1.0](../security/benchmarks/overview-v1.md) a HDInsight. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices relacionadas aplicables a HDInsight. Se han excluido los **controles** no aplicables a HDInsight.

 
Para ver cómo HDInsight se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de base de referencia de seguridad de HDInsight](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: la seguridad del perímetro en Azure HDInsight se logra mediante redes virtuales. Un administrador de empresa puede crear un clúster dentro de una red virtual y usar un grupo de seguridad de red (NSG) para restringir el acceso a la red virtual. Solo las direcciones IP permitidas en las reglas de grupo de seguridad de red de entrada podrán comunicarse con el clúster de Azure HDInsight. Esta configuración proporciona la seguridad del perímetro. Todos los clústeres implementados en una red virtual también tendrán un punto de conexión privado que se resuelve a una dirección IP privada dentro de la red virtual para el acceso HTTP privado a las puertas de enlace del clúster.

Para reducir el riesgo de pérdida de datos mediante filtración, restrinja el tráfico de red saliente de los clústeres de Azure HDInsight con Azure Firewall.

- [Implementación de Azure HDInsight en una red virtual y protección con un grupo de seguridad de red](hdinsight-create-virtual-network.md)

- [Restricción del tráfico saliente para clústeres de Azure HDInsight con Azure Firewall](hdinsight-restrict-outbound-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: use Azure Security Center y siga las recomendaciones de protección para la red virtual, la subred y el grupo de seguridad de red que se van a usar para proteger el clúster de Azure HDInsight. Habilite los registros de flujo del grupo de seguridad de red (NSG) y envíelos a una cuenta de Azure Storage para auditar el tráfico. También puede enviar registros de flujo del grupo de seguridad de red a un área de trabajo de Azure Log Analytics y usar Análisis de tráfico de Azure para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas de Análisis de tráfico de Azure son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso de Análisis de tráfico de Azure](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: para protegerse de los ataques DDoS, habilite la protección estándar de DDoS de Azure en la red virtual donde está implementada su instancia de Azure HDInsight. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas.

- [Configuración de la protección contra DDoS](/azure/virtual-network/manage-ddos-protection)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](/azure/security-center/security-center-alerts-service-layer)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: habilite los registros de flujo del grupo de seguridad de red (NSG) conectado a la subred que se usa para proteger el clúster de Azure HDInsight. Anote los registros de flujo del grupo de seguridad de red en una cuenta de Azure Storage para generar registros de flujo. Si es necesario para investigar actividades anómalas, habilite la captura de paquetes de Azure Network Watcher.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: se puede cumplir el requisito de identificador de control de seguridad de Azure 1.1; implemente un clúster de Azure HDInsight en una red virtual y protéjalo con un grupo de seguridad de red (NSG).

Hay varias dependencias de Azure HDInsight que requieren tráfico entrante. El tráfico entrante de administración no se puede enviar a través de un dispositivo de firewall. Las direcciones de origen del tráfico de administración necesario se conocen y publican. Cree reglas de grupo de seguridad de red con esta información para permitir el tráfico solo desde ubicaciones de confianza, y proteger el tráfico que entra a los clústeres.

Para reducir el riesgo de pérdida de datos mediante filtración, restrinja el tráfico de red saliente de los clústeres de Azure HDInsight con Azure Firewall.

- [Implementación de HDInsight dentro de una red virtual y protección con un grupo de seguridad de red](hdinsight-create-virtual-network.md)

- [Descripción de las dependencias de HDInsight y uso del firewall](hdinsight-restrict-outbound-traffic.md)

- [Direcciones IP de administración de HDInsight](hdinsight-management-ip-addresses.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de red virtual para definir controles de acceso de red en los grupos de seguridad de red (NSG) que están conectados a la subred en la que está implementado el clúster de Azure HDInsight. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de etiquetas de servicio para Azure HDInsight](/azure/virtual-network/security-overview#service-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de red relacionados con el clúster de Azure HDInsight. Use alias de Azure Policy en los espacios de nombres "Microsoft.HDInsight" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red del clúster de Azure HDInsight.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos del entorno clave, como plantillas de Azure Resource Manager, controles de Azure RBAC y directivas, en una única definición de plano técnico. Aplique fácilmente el plano técnico a nuevas suscripciones y entornos, y ajuste el control y la administración mediante el control de versiones.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: use etiquetas para el grupo de seguridad de red (NSG) y otros recursos relacionados con la seguridad de red y el flujo de tráfico que están asociados al clúster de Azure HDInsight. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la interfaz de la línea de comandos (CLI) de Azure para buscar los recursos o realizar acciones en ellos en función de sus etiquetas.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en estos relacionados con las implementaciones de Azure HDInsight. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: puede incorporar el clúster de Azure HDInsight a Azure Monitor para agregar datos de seguridad generados por el clúster. Aproveche las consultas personalizadas para detectar amenazas en el entorno y responder a ellas. 

- [Incorporación de un clúster de Azure HDInsight a Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Creación de consultas personalizadas para un clúster de Azure HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: habilite Azure Monitor para el clúster de HDInsight y diríjalo a un área de trabajo de Log Analytics. Esta acción registrará información y métricas del sistema operativo de interés para todos los nodos del clúster de Azure HDInsight.

- [Habilitación del registro para clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Consulta de registros de HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: incorpore el clúster de Azure HDInsight a Azure Monitor. Asegúrese de que el área de trabajo de Log Analytics usada tenga el período de retención de registros establecido de acuerdo con la normativa de cumplimiento de la organización.

- [Incorporación de un clúster de Azure HDInsight a Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Configuración del período de retención del área de trabajo de Log Analytics](/azure/azure-monitor/platform/manage-cost-storage)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: incorpore el clúster de Azure HDInsight a Azure Monitor. Asegúrese de que el área de trabajo de Log Analytics usada tenga el período de retención de registros establecido de acuerdo con la normativa de cumplimiento de la organización.

- [Incorporación de un clúster de Azure HDInsight a Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Configuración del período de retención del área de trabajo de Log Analytics](/azure/azure-monitor/platform/manage-cost-storage)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: use las consultas del área de trabajo de Azure Log Analytics para consultar los registros de Azure HDInsight:

- [Creación de consultas personalizadas para los clústeres de Azure HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: use el área de trabajo de Azure Log Analytics para supervisar los registros de seguridad y eventos relacionados con el clúster de Azure HDInsight y generar alertas sobre actividades anómalas en ellos.

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: Azure HDInsight incluye Clamscan preinstalado y habilitado para las imágenes de nodo de clúster; sin embargo, debe administrar el software y agregar o supervisar manualmente los registros que produce Clamscan.

- [Descripción de Clamscan](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: implemente una solución de terceros para el registro de DNS.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Instrucciones**: configure manualmente el registro de la consola por nodo.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Instrucciones**: mantenga el registro de la cuenta administrativa local que se crea durante el aprovisionamiento del clúster de Azure HDInsight, así como de cualquier otra cuenta que cree. Además, si se usa la integración de Azure Active Directory (Azure AD), Azure AD tiene roles integrados que se deben asignar explícitamente y, por tanto, se pueden consultar. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

Además, puede usar las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

- [Supervisión de identidad y acceso con Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Instrucciones**: al aprovisionar un clúster, Azure le exige que cree contraseñas para el acceso a Secure Shell (SSH) y al portal web. No hay contraseñas predeterminadas para cambiar; sin embargo, puede especificar distintas contraseñas para el acceso a SSH y al portal web.

- [Establecimiento de contraseñas al aprovisionar un clúster de Azure HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: integre la autenticación del clúster de Azure HDInsight con Azure Active Directory (Azure AD). Cree directivas y procedimientos en torno al uso de cuentas administrativas dedicadas.

Además, puede usar las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Integración de la autenticación de Azure HDInsight con Azure AD](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- [Supervisión de identidad y acceso con Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: use el agente de identidad de Azure HDInsight para iniciar sesión en clústeres de Enterprise Security Package (ESP) mediante la autenticación multifactor, sin proporcionar ninguna contraseña. Si ya ha iniciado sesión en otros servicios de Azure, como Azure Portal, puede iniciar sesión en el clúster de Azure HDInsight con una experiencia de inicio de sesión único (SSO).

- [Habilitación del agente de identidad de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker#enable-hdinsight-id-broker)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center. los clústeres de Azure HDInsight que tengan configurado Enterprise Security Package se pueden conectar a un dominio para que los usuarios de este puedan usar sus credenciales de dominio para autenticarse con los clústeres y ejecutar trabajos de macrodatos. Al autenticarse con la autenticación multifactor habilitada, a los usuarios se les presenta el desafío de proporcionar un segundo factor de autenticación.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: use PAW (estaciones de trabajo de acceso con privilegios) con la autenticación multifactor configurada para iniciar sesión en los clústeres de Azure HDInsight y recursos relacionados y configurarlos.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Instrucciones**: los clústeres de Azure HDInsight con Enterprise Security Package configurado se pueden conectar a un dominio para que los usuarios de este puedan usar sus credenciales de dominio para autenticarse. Puede usar los informes de seguridad de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzcan actividades sospechosas o no seguras en el entorno de Azure AD. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Instrucciones**: los clústeres de Azure HDInsight con Enterprise Security Package configurado se pueden conectar a un dominio para que los usuarios de este puedan usar sus credenciales de dominio para autenticarse. Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

los clústeres de Azure HDInsight con Enterprise Security Package (ESP) configurado pueden conectarse a un dominio para que los usuarios de este puedan usar sus credenciales de dominio para autenticarse con los clústeres.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Configuración de Enterprise Security Package con Azure AD Domain Services en Azure HDInsight](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: use la autenticación de Azure Active Directory (Azure AD) con el clúster de Azure HDInsight. Azure AD proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: use los registros de inicio de sesión y auditoría de Azure Active Directory (Azure AD) para supervisar los intentos de acceso a cuentas desactivadas; estos registros se pueden integrar en cualquier herramienta de SIEM o de supervisión de terceros.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Azure Log Analytics. Configure las alertas deseadas en el área de trabajo de Azure Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Instrucciones**: los clústeres de Azure HDInsight con Enterprise Security Package (ESP) configurado pueden conectarse a un dominio para que los usuarios de este puedan usar sus credenciales de dominio para autenticarse con los clústeres. use las características de protección de identidad y detección de riesgo de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. Además, puede ingerir datos en Azure Sentinel para investigarlos más a fondo.

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Instrucciones**: no disponible; la Caja de seguridad del cliente todavía no se admite en Azure HDInsight.

- [Lista de servicios admitidos por la Caja de seguridad del cliente](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas en los recursos relacionados con las implementaciones de Azure HDInsight para ayudar a realizar un seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. los clústeres de Azure HDInsight y las cuentas de almacenamiento asociadas deben separarse por red virtual o subred, etiquetarse correctamente y protegerse dentro de un grupo de seguridad de red (NSG) o Azure Firewall. Los datos del clúster deben estar incluidos en una cuenta de Azure Storage protegida o en Azure Data Lake Storage (Gen1 o Gen2).

- [Selección de las opciones de almacenamiento para el clúster de Azure HDInsight](hdinsight-hadoop-compare-storage-options.md)

- [Protección de Azure Data Lake Storage](../data-lake-store/data-lake-store-security-overview.md)

- [Protección de cuentas de Azure Storage](/azure/storage/common/storage-security-guide)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Instrucciones**: en el caso de los clústeres de Azure HDInsight que almacenan o procesan información confidencial, marque el clúster y los recursos relacionados como confidenciales mediante etiquetas. Para reducir el riesgo de pérdida de datos mediante filtración, restrinja el tráfico de red saliente de los clústeres de Azure HDInsight con Azure Firewall.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Restricción del tráfico saliente para clústeres de Azure HDInsight con Azure Firewall](hdinsight-restrict-outbound-traffic.md)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Instrucciones**: Cifre toda la información confidencial en tránsito. asegúrese de que los clientes que se conectan al clúster de Azure HDInsight o a los almacenes de datos del clúster (cuentas de Azure Storage o Azure Data Lake Storage Gen1/Gen2) pueden negociar TLS 1.2 o superior. De forma predeterminada, los recursos de Microsoft Azure negociarán TLS 1.2. 

- [Descripción del cifrado en tránsito de Azure Data Lake Storage](../data-lake-store/data-lake-store-security-overview.md)

- [Descripción del cifrado en tránsito de la cuenta de Azure Storage](../storage/blobs/security-recommendations.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Instrucciones**: las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Uso de RBAC de Azure para controlar el acceso a los recursos 

**Instrucciones**: con Enterprise Security Package (ESP) de Azure HDInsight, puede usar Apache Ranger para crear y administrar directivas de control de acceso y ofuscación de datos específicas para los datos almacenados en archivos, carpetas, bases de datos, tablas y filas y columnas. El administrador de Hadoop puede configurar el control de acceso basado en rol (RBAC) para proteger Apache Hive, HBase y Kafka mediante esos complementos de Apache Ranger.

La configuración de las directivas RBAC con Apache Ranger permite asociar permisos a un rol de la organización. Esta capa de abstracción hace más fácil asegurar que los usuarios solo tengan los permisos necesarios para realizar sus responsabilidades laborales.

- [Configuraciones de Enterprise Security Package con Azure Active Directory (Azure AD) Domain Services en HDInsight](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- [Introducción a la seguridad de la empresa en Azure HDInsight](domain-joined/hdinsight-security-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Instrucciones**: en el caso de los clústeres de Azure HDInsight que almacenan o procesan información confidencial, marque el clúster y los recursos relacionados como confidenciales mediante etiquetas. Las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Instrucciones**: si usa Azure SQL Database para almacenar metadatos de Apache Hive y Apache Oozie, asegúrese de que los datos SQL permanecen cifrados en todo momento. En el caso de las cuentas de Azure Storage y Data Lake Storage (Gen1 o Gen2), se recomienda permitir que Microsoft administre las claves de cifrado; sin embargo, tiene la opción de administrar sus propias claves.

- [Administración de las claves de cifrado para cuentas de Azure Storage](/azure/storage/common/storage-encryption-keys-portal)

- [Creación de Azure Data Lake Storage mediante claves de cifrado administradas por el cliente](../data-lake-store/data-lake-store-get-started-portal.md)

- [Descripción del cifrado para Azure SQL Database](/azure/sql-database/sql-database-technical-overview#data-encryption)

- [Configuración del Cifrado de datos transparente para SQL Database mediante claves administradas por el cliente](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Instrucciones**: configure los valores de diagnóstico para las cuentas de Azure Storage asociadas a los clústeres de Azure HDInsight a fin de supervisar y registrar todas las operaciones CRUD en los datos del clúster. Habilite la auditoría para las cuentas de almacenamiento o las instancias de Data Lake Store asociadas con el clúster de Azure HDInsight.

- [Habilitación de registro o auditoría adicionales para una cuenta de Azure Storage](/azure/storage/common/storage-monitor-storage-account)

- [Habilitación de registro o auditoría adicionales para Azure Data Lake Storage](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Instrucciones**: implemente una solución de administración de vulnerabilidades de terceros.

Opcionalmente, si dispone de Rapid7, Qualys o cualquier otra suscripción a una plataforma de administración de vulnerabilidades, puede usar acciones de script para instalar agentes de evaluación de vulnerabilidades en los nodos del clúster de Azure HDInsight y administrar los nodos mediante el portal correspondiente.

- [Instalación manual del agente de Rapid7](https://insightvm.help.rapid7.com/docs/install)

- [Instalación manual del agente de Qualys](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

- [Uso de acciones de script](hdinsight-hadoop-customize-cluster-linux.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Instrucciones**: se han habilitado las actualizaciones automáticas del sistema para las imágenes de nodo de clúster; sin embargo, debe reiniciar periódicamente los nodos del clúster para asegurarse de que se aplican las actualizaciones.

Microsoft mantiene y actualiza las imágenes de nodo base de Azure HDInsight.

- [Configuración de la programación de la aplicación de revisiones del sistema operativo para clústeres de HDInsight](hdinsight-os-patching.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Instrucciones**: use acciones de script u otros mecanismos para aplicar revisiones a los clústeres de Azure HDInsight. Los clústeres creados recientemente siempre tendrán las últimas actualizaciones disponibles, incluidas las revisiones de seguridad más recientes.

- [Configuración de la programación de aplicación de revisiones del sistema operativo para clústeres de Azure HDInsight basados en Linux](hdinsight-os-patching.md)

- [Uso de acciones de script](hdinsight-hadoop-customize-cluster-linux.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Instrucciones**: implemente una solución de administración de vulnerabilidades de terceros que tenga la capacidad de comparar exámenes de vulnerabilidades a lo largo del tiempo. Si tiene una suscripción a Rapid7 o Qualys, puede usar el portal de ese proveedor para ver y comparar exámenes de vulnerabilidades opuestos.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Instrucciones**: use un programa de puntuación de riesgos común (por ejemplo, Common Vulnerability Scoring System) o la clasificación de riesgos predeterminada proporcionada por su herramienta de examen de terceros.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.), incluidos los clústeres de Azure HDInsight, dentro de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Azure Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Instrucciones**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Instrucciones**: Defina una lista de recursos de Azure aprobados y software aprobado para los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Instrucciones**: implemente una solución de terceros para supervisar los nodos del clúster en busca de aplicaciones de software no aprobadas.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.), incluidos los clústeres de Azure HDInsight, dentro de las suscripciones.  Quite los recursos de Azure no aprobados que detecte. En el caso de los nodos del clúster de Azure HDInsight, implemente una solución de terceros para quitar software no aprobado o alertas al respecto.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Instrucciones**: en el caso de los nodos del clúster de Azure HDInsight, implemente una solución de terceros para impedir que se ejecute software no autorizado.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Para más información, consulte las siguientes referencias:

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Instrucciones**: en el caso de los nodos del clúster de Azure HDInsight, implemente una solución de terceros para impedir que se ejecuten tipos de archivos no autorizados.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones**: use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Instrucciones**: use alias de Azure Policy en el espacio de nombres "Microsoft.HDInsight" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red del clúster de HDInsight.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Instrucciones**: Microsoft administra y mantiene las imágenes del sistema operativo de Azure HDInsight. El cliente es responsable de implementar configuraciones seguras para el sistema operativo de los nodos del clúster.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: use Azure Policy [denegar] e [impleméntela si no existe] para aplicar la configuración segura a los clústeres de Azure HDInsight y los recursos relacionados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Instrucciones**: Microsoft administra y mantiene las imágenes del sistema operativo de Azure HDInsight. El cliente es responsable de implementar la configuración de estado en el nivel de sistema operativo.

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Si usa definiciones de personalizadas de Azure Policy, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

- [Tutorial de Git de Azure Repos](/azure/devops/repos/git/gitworkflow)

- [Documentación de Azure Repos](/azure/devops/repos/index)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Instrucciones**: use alias de Azure Policy en el espacio de nombres "Microsoft.HDInsight" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Instrucciones**: implemente una solución de terceros para mantener el estado deseado de los sistemas operativos del nodo de clúster.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: use alias de Azure Policy en el espacio de nombres "Microsoft.HDInsight" para crear directivas personalizadas con el fin de auditar o aplicar la configuración del clúster de HDInsight.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Instrucciones**: implemente una solución de terceros para supervisar el estado de los sistemas operativos del nodo de clúster.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones**: Azure HDInsight incluye compatibilidad con Bring Your Own Key (BYOK) para Apache Kafka. Esta funcionalidad le permite ser el propietario y administrar las claves utilizadas para cifrar datos en reposo.

todos los discos administrados de Azure HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Al habilitar BYOK, le proporciona a Azure HDInsight la clave de cifrado para que la use y administre con Azure Key Vault.

Key Vault también puede usarse con implementaciones de Azure HDInsight para administrar las claves de almacenamiento del clúster (cuentas de Azure Storage y Azure Data Lake Storage).

- [Traer su propia clave para Apache Kafka en Azure HDInsight](/azure/hdinsight/kafka/apache-kafka-byok)

- [Administración de las claves de cifrado para cuentas de Azure Storage](/azure/storage/common/storage-encryption-keys-portal)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: se pueden usar identidades administradas en Azure HDInsight para permitir que los clústeres accedan a los servicios de dominio de Azure Active Directory (Azure AD), a Azure Key Vault o a archivos de Azure Data Lake Storage Gen2.

- [Descripción de las identidades administradas con Azure HDInsight](hdinsight-managed-identities.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: si usa cualquier código relacionado con la implementación de Azure HDInsight, puede implementar Credential Scanner para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Instrucciones**: Azure HDInsight incluye Clamscan preinstalado y habilitado para las imágenes de nodo de clúster; sin embargo, debe administrar el software y agregar o supervisar manualmente los registros que produce Clamscan.

- [Descripción de Clamscan para Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Instrucciones**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure, pero no se ejecuta en el contenido del cliente.

Examine previamente los archivos que se van a cargar en los recursos de Azure relacionados con la implementación del clúster de Azure HDInsight, como Data Lake Storage, Blob Storage, etc. Microsoft no tiene acceso a los datos del cliente en estas instancias.

- [Descripción de Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Asegúrese de que se han actualizado el software y las firmas antimalware

**Instrucciones**: Azure HDInsight incluye Clamscan preinstalado y habilitado para las imágenes de nodo de clúster. Clamscan realizará actualizaciones del motor y de las definiciones de forma automática; no obstante, la agregación y administración de registros deberá realizarse manualmente.

- [Descripción de Clamscan para Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: al usar una cuenta de Azure Storage para el almacén de datos del clúster de Azure HDInsight, elija la opción de redundancia adecuada (LRS, ZRS, GRS, RA-GRS).  Al usar una instancia de Azure SQL Database para el almacén de datos del clúster de Azure HDInsight, configure la replicación geográfica activa.

- [Configuración de la redundancia del almacenamiento para cuentas de Azure Storage](../storage/common/storage-redundancy.md)

- [Cómo configurar la redundancia para Azure SQL Database](/azure/sql-database/sql-database-active-geo-replication)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: al usar una cuenta de Azure Storage para el almacén de datos del clúster de Azure HDInsight, elija la opción de redundancia adecuada (LRS, ZRS, GRS, RA-GRS). Si usa Azure Key Vault en alguna parte de la implementación de Azure HDInsight, asegúrese de que se realizan copias de seguridad de las claves.

- [Selección de las opciones de almacenamiento para el clúster de Azure HDInsight](hdinsight-hadoop-compare-storage-options.md)

- [Configuración de la redundancia del almacenamiento para cuentas de Azure Storage](../storage/common/storage-redundancy.md)

- [Copias de seguridad de las claves de Key Vault en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: si Azure Key Vault se usa con la implementación de Azure HDInsight, pruebe la restauración de las copias de seguridad de las claves administradas por el cliente.

- [Traer su propia clave para Apache Kafka en Azure HDInsight](/azure/hdinsight/kafka/apache-kafka-byok)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: si Azure Key Vault se usa con la implementación de Azure HDInsight, habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Habilitación de la eliminación temporal en Azure Key Vault](/azure/key-vault/key-vault-ovw-soft-delete)

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

**Instrucciones**: Security Center asigna una gravedad a las alertas, que le ayudan a priorizar el orden en el que asiste a cada alerta, de modo que cuando un recurso está en peligro, puede obtenerlo inmediatamente. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos.

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
