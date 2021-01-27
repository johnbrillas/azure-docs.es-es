---
title: Línea de base de seguridad de Azure para Batch
description: La línea de base de seguridad de Batch proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: batch
ms.topic: conceptual
ms.date: 12/01/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 3678ead9f3e1ba2556fde3c2fbe30df4e7dc2225
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98737242"
---
# <a name="azure-security-baseline-for-batch"></a>Línea de base de seguridad de Azure para Batch

Esta línea de base de seguridad aplica las instrucciones de [Azure Security Benchmark, versión 1.0](../security/benchmarks/overview-v1.md) a Batch. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices relacionadas aplicables a Batch. Se han excluido los **controles** no aplicables a Batch.

 
Para ver cómo Batch se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de base de referencia de seguridad de Batch](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: implemente grupos de Azure Batch dentro de una red virtual. Para permitir que los nodos de proceso del grupo se comuniquen de manera segura con otras máquinas virtuales o con una red local, puede aprovisionar el grupo en una subred de una red virtual de Azure. Además, la implementación del grupo en una red virtual le proporciona control sobre el grupo de seguridad de red (NSG) que se usa para proteger las interfaces de red (NIC) de los nodos individuales, así como la subred. Configure el NSG para permitir el tráfico solo desde las direcciones IP y ubicaciones de confianza en Internet.

Si procede, deshabilite el acceso a la red pública mediante Azure Private Link para conectarse a la cuenta de Azure Batch a través de un punto de conexión privado. El servicio Azure Private Link está protegido y solo acepta conexiones de puntos de conexión privados autenticados y autorizados. Además, para limitar aún más la conectividad y la detectabilidad, puede deshabilitar los puntos de conexión de RDP/SSH expuestos públicamente para los nodos de proceso de un grupo de Batch.

- [Creación de un grupo de Azure Batch en una red virtual](batch-virtual-network.md)

- [Creación de una cuenta de Azure Batch con el acceso a la red deshabilitado](private-connectivity.md)

- [Creación de un punto de conexión privado](../private-link/create-private-endpoint-portal.md)

- [Denegación del acceso al tráfico de RDP/SSH](pool-endpoint-configuration.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: use Azure Security Center y corrija las recomendaciones de protección de red relacionadas con la red virtual o el grupo de seguridad de red (NSG) asociado al grupo de Batch. Habilite los registros de flujo en el NSG que se usa para proteger el grupo de Batch y envíe los registros a una cuenta de Azure Storage para la auditoría de tráfico. También puede enviar registros de flujo del grupo de seguridad de red a un área de trabajo de Azure Log Analytics y usar Análisis de tráfico de Azure para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas de Análisis de tráfico de Azure son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: habilite la protección estándar de DDoS (denegación de servicio distribuida) de Azure en la red virtual protegiendo el grupo de Azure Batch para la protección frente a ataques DDoS. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas.

- [Configuración de la protección contra DDoS](../ddos-protection/manage-ddos-protection.md)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](../security-center/azure-defender.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: habilite los registros de flujo en el grupo de seguridad de red (NSG) que se usa para proteger el grupo de Azure Batch y envíe los registros a una cuenta de Azure Storage para la auditoría de tráfico.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: si es necesario por motivos de cumplimiento, seleccione una aplicación virtual de red de Azure Marketplace que admita la funcionalidad de sistemas de detección de intrusiones (IDS) y sistemas de prevención de intrusiones (IPS) con funcionalidades de inspección de carga.

Si la detección y/o la prevención de intrusiones basadas en la inspección de carga no son un requisito, se puede usar Azure Firewall con la inteligencia sobre amenazas. El filtrado basado en inteligencia sobre amenazas de Azure Firewall puede alertar y denegar el tráfico desde y hacia los dominios y las direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

Implemente Azure Firewall con una dirección IP pública de la misma red virtual que los nodos del grupo de Azure Batch. Configure reglas de traducción de direcciones de red (NAT) entre las ubicaciones de confianza de Internet y las direcciones IP privadas de los nodos del grupo individuales. En Azure Firewall, en Inteligencia sobre amenazas, configure "Alertar y denegar" para alertar y bloquear el tráfico hacia y desde direcciones IP y dominios malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente de inteligencia sobre amenazas de Microsoft y solo se incluyen los registros de mayor confianza. 

- [Creación de un grupo de Azure Batch en una red virtual](batch-virtual-network.md)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de red virtual para definir controles de acceso a la red en los grupos de seguridad de red o las instancias de Azure Firewall asociadas a los grupos de Azure Batch. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de red asociados a los grupos de Azure Batch con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.Batch" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los grupos de Azure Batch.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: use etiquetas para los grupos de seguridad de red (NSG) y otros recursos relacionados con la seguridad de red y el flujo de tráfico que están asociados a los grupos de Azure Batch. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red](../virtual-network/tutorial-filter-network-traffic.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con los grupos de Azure Batch. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log) 

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: incorpore la cuenta de Azure Batch a Azure Monitor para agregar los datos de seguridad generados por los dispositivos del clúster. Aproveche las consultas personalizadas para detectar amenazas en el entorno y responder a ellas.  Para la supervisión en el nivel de recurso de Azure Batch, utilice las API de Batch para supervisar o consultar el estado de los recursos, como los trabajos, las tareas, los nodos y los grupos.

- [Incorporación de una cuenta de Azure Batch a Azure Monitor](batch-diagnostics.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Para la supervisión en el nivel de cuenta de Azure Batch, supervise cada cuenta de Batch con las características de Azure Monitor. Azure Monitor recopila métricas y, opcionalmente, registros de diagnóstico para los recursos del nivel de cuenta de Batch, como grupos, trabajos y tareas. Recopile y consuma estos datos manualmente o mediante programación para supervisar las actividades de la cuenta de Batch y para diagnosticar problemas.

Para la supervisión en el nivel de recurso de Azure Batch, utilice las API de Azure Batch para supervisar o consultar el estado de los recursos, como los trabajos, las tareas, los nodos y los grupos.

- [Configuración del registro y la supervisión en el nivel de cuenta de Azure Batch](monitoring-overview.md)

- [Descripción de la supervisión en el nivel de recurso de Batch](monitoring-overview.md#batch-resource-monitoring)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

#### <a name="azure-policy-built-in-definitions"></a>Definiciones integradas de Azure Policy

[!INCLUDE [microsoft.batch-2-3](../../includes/policy/standards/asb/rp-controls/microsoft.batch-2-3.md)]

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: Azure Monitor recopila métricas y registros de diagnóstico de los recursos de la cuenta de Azure Batch. Recopile y consuma estos datos de diversas maneras para supervisar la cuenta de Azure Batch y diagnosticar problemas. También puede configurar alertas de métricas para recibir notificaciones cuando una métrica alcance un valor especificado.

Si es necesario, es posible conectarse a los nodos individuales de un grupo mediante Secure Shell (SSH) o Protocolo de escritorio remoto (RDP) para acceder a los registros del sistema operativo local.

- [Recopilación de los registros de diagnóstico de la cuenta de Azure Batch](batch-diagnostics.md#batch-diagnostics)

- [Conexión de forma remota a los nodos del grupo de Azure Batch](./batch-service-workflow-features.md#basic-workflow)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: Incorpore una cuenta de Azure Batch a Azure Monitor. Asegúrese de que el área de trabajo de Azure Log Analytics utilizada tiene el período de retención establecido de acuerdo con la normativa de cumplimiento de la organización.

- [Configuración del registro y la supervisión de Azure Batch](monitoring-overview.md)

- [Configuración del período de retención del área de trabajo de Azure Log Analytics](../azure-monitor/platform/manage-cost-storage.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: cree alertas de métricas de Azure Batch que se desencadenan cuando el valor de una métrica específica cruza un umbral determinado.

- [Configuración de alertas de métricas de Azure Batch](batch-diagnostics.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: cree alertas de métricas de Azure Batch que se desencadenan cuando el valor de una métrica específica cruza un umbral determinado.

- [Configuración de alertas de métricas de Azure Batch](batch-diagnostics.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: use Windows Defender en los nodos de Batch individuales en el caso de los sistemas operativos Windows o proporcione su propia solución antimalware si usa Linux.

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: implemente una solución de terceros para el registro de DNS.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: configure manualmente el registro de consola y la transcripción de PowerShell en cada nodo.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: mantenga el registro de la cuenta administrativa local que se crea durante el aprovisionamiento del grupo de Azure Batch, así como de cualquier otra cuenta que cree. Además, si se usa la integración de Azure Active Directory, Azure AD tiene roles integrados que se deben asignar explícitamente y, por tanto, se pueden consultar. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

Además, puede usar las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?preserve-view=true&view=azureadps-2.0)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?preserve-view=true&view=azureadps-2.0)

- [Supervisión de identidad y acceso con Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: al aprovisionar un grupo de Azure Batch, se le ofrece la opción de crear cuentas de equipo local. No hay contraseñas predeterminadas para cambiar; sin embargo, puede especificar distintas contraseñas para el acceso de Secure Shell (SSH) y Protocolo de escritorio remoto (RDP). Una vez configurado el grupo de Azure Batch, puede generar un usuario aleatorio para los nodos individuales en Azure Portal o mediante la API de Azure Resource Manager.

- [Incorporación de un usuario a un nodo de proceso específico](/rest/api/batchservice/computenode/adduser)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: integre la autenticación para las aplicaciones de Azure Batch con Azure Active Directory. Cree directivas y procedimientos en torno al uso de cuentas administrativas dedicadas.

Además, puede usar las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Autenticación de aplicaciones de Batch con Azure Active Directory](batch-aad-auth.md)

- [Supervisión de identidad y acceso con Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: integre la autenticación para las aplicaciones de Azure Batch con Azure Active Directory. Habilite la autenticación multifactor de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

 

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Uso de estaciones de trabajo seguras y administradas por Azure para realizar tareas administrativas

**Guía**: utilice las PAW (estaciones de trabajo de acceso con privilegios) con la autenticación multifactor configurada para iniciar sesión y configurar los recursos de Azure Batch.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: si ha integrado la autenticación para las aplicaciones de Azure Batch con Azure Active Directory, use los informes de seguridad de Azure Active Directory para la generación de registros y alertas cuando se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: si ha integrado la autenticación para las aplicaciones de Azure Batch con Azure Active Directory, puede usar ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: use Azure Active Directory como sistema central de autenticación y autorización e integre la autenticación para las aplicaciones de Azure Batch con Azure AD. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory-domain-services/tutorial-create-instance.md)

- [Autenticación de aplicaciones de Batch con Azure AD](batch-aad-auth.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Active Directory proporciona registros para ayudar a descubrir cuentas obsoletas. Además, puede utilizar las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo los usuarios adecuados tengan acceso continuado.

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: cree una configuración de diagnóstico para las cuentas de usuario de Azure Active Directory mediante el envío de los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Azure Log Analytics. Configure las alertas deseadas en el área de trabajo de Azure Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: use las características de protección de identidad y detección de riesgo de Azure Active Directory para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. Además, puede ingerir datos en Azure Sentinel para investigarlos más a fondo.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico  

**Guía**: no disponible; Caja de seguridad del cliente todavía no se admite para Azure Batch.
 
- [Lista de servicios admitidos por la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Los grupos de Azure Batch deben separarse mediante una red virtual o subred, etiquetarse adecuadamente y protegerse con grupos de seguridad de red (NSG). Los datos de Azure Batch deben residir en una cuenta de Azure Storage protegida.

- [Creación de un grupo de Azure Batch en una red virtual](batch-virtual-network.md)

- [Protección de cuentas de Azure Storage](../storage/blobs/security-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: en el caso de las cuentas de Azure Storage asociadas a los grupos de Azure Batch que contienen información confidencial, márquelas como confidenciales mediante etiquetas y protéjalas con los procedimientos recomendados de Azure.

Las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

- [Protección de cuentas de Azure Storage](../storage/blobs/security-recommendations.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Instrucciones**: Cifre toda la información confidencial en tránsito. De forma predeterminada, los recursos de Microsoft Azure negociarán TLS 1.2. Asegúrese de que los clientes que se conectan a los grupos de Azure Batch o los almacenes de datos (cuentas de Azure Storage) pueden negociar TLS 1.2 o superior.

Asegúrese de que se requiere HTTPS para acceder a la cuenta de almacenamiento que contiene los datos de Azure Batch.

- [Descripción del cifrado en tránsito de la cuenta de Azure Storage](../storage/blobs/security-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: en el caso de las cuentas de Azure Storage asociadas a los grupos de Azure Batch que contienen información confidencial, márquelas como confidenciales mediante etiquetas y protéjalas con los procedimientos recomendados de Azure.

Las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

- [Protección de cuentas de Azure Storage](../storage/blobs/security-recommendations.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Use el control de acceso basado en rol (RBAC) para controlar el acceso al plano de administración de los recursos de Azure, como la cuenta de Batch, los grupos de Batch y las cuentas de almacenamiento.

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: en el caso de las cuentas de almacenamiento asociadas a la cuenta de Azure Batch, se recomienda permitir que Microsoft administre las claves de cifrado; sin embargo, tiene la opción de administrar sus propias claves si es necesario.

Azure Disk Encryption se puede usar para custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Todos los discos administrados, instantáneas e imágenes, así como los datos escritos en los discos existentes, se cifran automáticamente en reposo con claves administradas por la plataforma.

- [Administración de las claves de cifrado para cuentas de Azure Storage](../storage/common/customer-managed-keys-configure-key-vault.md)

- [Configuración de las claves de cifrado administradas por el cliente](../storage/common/customer-managed-keys-configure-key-vault.md)

- [Creación de un grupo con el cifrado de disco habilitado](disk-encryption.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure relacionados o asociados a las cuentas y grupos de Azure Batch.

Configure los valores de diagnóstico para las cuentas de almacenamiento asociadas al grupo de Azure Batch para supervisar y registrar todas las operaciones CRUD en los datos del grupo.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

- [Habilitación de registro o auditoría adicionales para una cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: en el caso de los nodos del grupo de Azure Batch, usted es responsable de administrar la solución de administración de vulnerabilidades.

Opcionalmente, si dispone de Rapid7, Qualys o cualquier otra suscripción de plataforma de administración de vulnerabilidades, puede instalar manualmente los agentes de evaluación de vulnerabilidades en los nodos del grupo de Batch y administrar los nodos mediante el portal correspondiente.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Guía**: Microsoft mantiene y actualiza las imágenes de nodo del grupo de Azure Batch de base. Asegúrese de que el sistema operativo de los nodos del grupo de Azure Batch permanece revisado a lo largo de la duración del clúster, lo que puede requerir la habilitación de actualizaciones automáticas, la supervisión de los nodos o la realización de reinicios periódicos.

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía**: Asegúrese de que las aplicaciones de terceros de los nodos del grupo de Azure Batch permanecen revisadas a lo largo de la duración del clúster, lo que puede requerir la habilitación de actualizaciones automáticas, la supervisión de los nodos o la realización de reinicios periódicos.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Si dispone de Rapid7, Qualys o cualquier otra suscripción de plataforma de administración de vulnerabilidades, puede usar el portal del proveedor para ver y comparar los exámenes de vulnerabilidades opuestos.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Instrucciones**: use un programa de puntuación de riesgos común (por ejemplo, Common Vulnerability Scoring System) o la clasificación de riesgos predeterminada proporcionada por su herramienta de examen de terceros.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, etc.) dentro de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Azure Resource Graph Explorer, se recomienda crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription?preserve-view=true&view=azps-4.8.0)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Instrucciones**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: defina una lista de los recursos de Azure y el software aprobados para los recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Instrucciones**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: en el caso de los nodos del grupo de Azure Batch, implemente una solución de terceros para supervisar los nodos del clúster en busca de aplicaciones de software no aprobadas.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: en el caso de los nodos del grupo de Azure Batch, implemente una solución de terceros para supervisar los nodos del clúster en busca de aplicaciones de software no aprobadas.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: en el caso de los nodos del grupo de Azure Batch, implemente una solución de terceros para evitar que se ejecute software no autorizado.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas: 
- Tipos de recursos no permitidos 
- Tipos de recursos permitidos 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía**: en el caso de los nodos del grupo de Azure Batch, implemente una solución de terceros para evitar que se ejecuten tipos de archivos no autorizados.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones**: use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: use alias de Azure Policy en el espacio de nombres "Microsoft.Batch" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de las cuentas y los grupos de Azure Batch.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: establezca configuraciones seguras para el sistema operativo de los nodos del grupo de Batch.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Instrucciones**: Use [deny] y [deploy if not exist] de Azure Policy para aplicar una configuración segura para los recursos de Azure relacionados con la cuenta y los grupos de Batch (por ejemplo, redes virtuales, subredes, instancias de Azure Firewall, cuentas de Azure Storage, etc.). Puede utilizar alias de Azure Policy procedentes de los siguientes espacios de nombres para crear directivas personalizadas:

- Microsoft.Batch

- Microsoft.Storage

- Microsoft.Network

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: Microsoft administra y mantiene las imágenes del sistema operativo del grupo de Azure Batch. Usted es responsable de implementar la configuración de estado en el nivel de sistema operativo.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Instrucciones**: Si usa definiciones personalizadas de Azure Policy para las cuentas y los grupos de Azure Batch, o para los recursos relacionados, use Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow?preserve-view=true&view=azure-devops)

- [Documentación de Azure Repos](/azure/devops/repos/?preserve-view=true&view=azure-devops)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: Si usa imágenes personalizadas para los grupos de Azure Batch, use el control de acceso basado en rol (RBAC) para asegurarse de que solo los usuarios autorizados pueden acceder a las imágenes.

- [Descripción de RBAC en Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)

- [Configuración de RBAC en Azure](../role-based-access-control/quickstart-assign-role-user-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: use definiciones de Azure Policy integradas para alertar, auditar y aplicar las configuraciones de los recursos relacionados con Azure Batch.  use alias de Azure Policy en el espacio de nombres "Microsoft.Batch" para crear directivas personalizadas para las cuentas y los grupos de Azure Batch. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Guía**: implemente una solución de terceros para mantener el estado deseado de los sistemas operativos de los nodos del grupo de Azure Batch.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: use alias de Azure Policy en el espacio de nombres "Microsoft.Batch" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de la instancia de Azure Batch. También puede usar cualquier directiva integrada creada específicamente para Azure Batch o los recursos usados por Azure Batch, como:
- Las subredes deben estar asociadas a un grupo de seguridad de red: las cuentas de almacenamiento deben usar un punto de conexión de servicio de red virtual.
- Se deben habilitar los registros de diagnóstico en las cuentas de Batch

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía**: implemente una solución de terceros para supervisar el estado de los sistemas operativos de los nodos del grupo de Azure Batch.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**: puede usar Azure Key Vault con las implementaciones de Azure Batch para administrar las claves para el almacenamiento del grupo en cuentas de Azure Storage.

- [Integración con identidades administradas de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Creación de una instancia de Azure Key Vault](../key-vault/general/quick-create-portal.md)

- [Autenticación en Key Vault](../key-vault/general/authentication.md)
- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: use Windows Defender en los nodos del grupo de Azure Batch individuales en el caso de los sistemas operativos Windows o proporcione su propia solución antimalware si usa Linux.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Batch), pero no se ejecuta en el contenido del cliente.

Examine previamente los archivos que se cargan en recursos de Azure que no son de proceso, como App Service, Data Lake Storage, Blob Storage, etc. Microsoft no tiene acceso a los datos del cliente en estas instancias.

- [Descripción de Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Asegúrese de que se han actualizado el software y las firmas antimalware

**Guía**: use Windows Defender en los nodos individuales del grupo de Azure Batch en el caso de los sistemas operativos Windows y asegúrese de que está habilitada la actualización automática. Proporcione su propia solución antimalware si usa Linux.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: Al usar una cuenta de Azure Storage para el almacén de datos del grupo de Azure Batch, elija la opción de redundancia adecuada (LRS,ZRS, GRS, RA-GRS). 

- [Configuración de la redundancia del almacenamiento para cuentas de Azure Storage](../storage/common/storage-redundancy.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: Al usar una cuenta de Azure Storage para el almacén de datos del grupo de Azure Batch, elija la opción de redundancia adecuada (LRS,ZRS, GRS, RA-GRS). Si usa Azure Key Vault en alguna parte de la implementación de Azure Batch, asegúrese de que se realizan copias de seguridad de las claves.

- [Configuración de la redundancia del almacenamiento para cuentas de Azure Storage](../storage/common/storage-redundancy.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey?preserve-view=true&view=azps-4.8.0)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: si administra sus propias claves para las cuentas de Azure Storage o cualquier otro recurso relacionado con la implementación de Azure Batch, pruebe de forma periódica la restauración de las claves de las que ha realizado copia de seguridad.

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey?preserve-view=true&view=azps-4.8.0)

- [Restauración de una clave administrada por el cliente con PowerShell](/powershell/module/az.keyvault/restore-azkeyvaultkey?preserve-view=true&view=azps-4.8.0)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Si se usa Azure Key Vault para contener las claves relacionadas con las cuentas de almacenamiento del grupo de Azure Batch, habilite la eliminación temporal en Azure Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Habilitación de la eliminación temporal en Azure Key Vault](../key-vault/general/key-vault-recovery.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía**: Asegúrese de que haya planes de respuesta ante incidentes escritos que definan los roles del personal, así como las fases de administración y gestión de los incidentes.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a las alertas, que le ayudan a priorizar el orden en el que asiste a cada alerta, de modo que cuando un recurso está en peligro, puede obtenerlo inmediatamente. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: la información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones**: use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: [siga las reglas de compromiso de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.).

A continuación encontrará más información sobre la estrategia y puesta en marcha de un equipo rojo de Microsoft, y las pruebas de penetración en sitios activos de la infraestructura en la nube, las aplicaciones y los servicios administrados por Microsoft: 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
