---
title: Línea de referencia de seguridad de Azure para Azure DNS
description: La base de referencia de seguridad de Azure DNS proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: dns
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: b870a0325646b01ae3a72bdd28d3ae33cba45b09
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101733132"
---
# <a name="azure-security-baseline-for-azure-dns"></a>Línea de referencia de seguridad de Azure para Azure DNS

Esta línea de base de seguridad se aplica en la guía de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md) en Microsoft Azure DNS. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** que define Azure Security Benchmark y las directrices correspondientes aplicables a Azure DNS. Se han excluido los **controles** no aplicables a Azure DNS.

 
Para ver cómo Azure DNS se adapta por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure DNS](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: el registro de actividad es un registro de la plataforma de Azure que proporciona información de los eventos en el nivel de suscripción. Envíe los registros a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Los registros de actividad proporcionan información sobre las operaciones llevadas a cabo en los recursos de Azure DNS en el nivel del plano de control. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para las zonas DNS.

Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una solución de administración de eventos e información de un sistema de terceros.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: No aplicable; el servicio DNS no es compatible con los registros de diagnóstico.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: En Azure Monitor, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos y a largo plazo.

- [Cambio del período de retención de datos en Log Analytics](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](/azure/storage/common/storage-monitor-storage-account#configure-logging)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: Analice y supervise los registros en busca de comportamientos anómalos y revise los resultados con regularidad. Use Azure Monitor y un área de trabajo de Log Analytics para revisar los registros y realizar consultas en los datos de dichos registros.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una solución de administración de eventos e información de un sistema de terceros.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Introducción a las consultas de Log Analytics](/azure/azure-monitor/log-query/log-analytics-tutorial)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: use Azure Security Center con las áreas de trabajo de Log Analytics para la supervisión y las alertas sobre actividades anómalas que se encuentran en los registros y eventos de seguridad.

Como alternativa, puede habilitar e incorporar datos a Azure Sentinel.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: El control de acceso basado en roles de Azure (RBAC de Azure) permite administrar el acceso a los recursos de Azure a través de las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas, como la CLI de Azure, Azure PowerShell o el Azure Portal.

En Azure DNS, existe el rol Colaborador de zona DNS, así como un RBAC de Azure de nivel de zona y de nivel de conjunto de registros. También es posible crear roles de Azure de cliente propios, para proporcionar un control más detallado. Tenga en cuenta que los recursos de la zona DNS privado usan un nombre de rol diferente, Colaborador de zona DNS privado.

- [Obtención de un rol de directorio en Azure Active Directory (Azure AD) con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

- [Descripción de RBAC de Azure en Azure DNS](https://docs.microsoft.com/azure/dns/dns-protect-zones-recordsets#azure-role-based-access-control)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Active Directory (Azure AD) no tiene el concepto de contraseñas predeterminadas. Otros recursos de Azure que sí que requieren una contraseña obligan a crearla con requisitos de complejidad y una longitud mínima. Los requisitos varían en función del servicio. Usted es responsable de las aplicaciones de terceros y de los servicios de Marketplace que pueden usar contraseñas predeterminadas.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidades y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarlo a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center, por ejemplo:
- Debe haber más de un propietario asignado a su suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

También puede habilitar el acceso Just-in-Time a cuentas administrativas mediante Azure Active Directory (Azure AD) Privileged Identity Management y Azure Resource Manager.

- [Más información acerca de Privileged Identity Management](/azure/active-directory/privileged-identity-management/)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Instrucciones**: Use un registro de aplicaciones de Azure (entidad de servicio) para recuperar un token que se pueda usar para interactuar con las zonas DNS y conjuntos de registros a través de llamadas API.

- [Llamada a las API REST de Azure](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [Registro de la aplicación cliente (entidad de servicio) con Azure Active Directory (Azure AD)](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Información de la API de Azure DNS Protection](/rest/api/dns/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Instrucciones**: use una estación de trabajo segura y administrada por Azure (también conocida como una estación de trabajo de acceso con privilegios o PAW) para las tareas administrativas que requieren privilegios elevados.

- [Descripción de las estaciones de trabajo seguras administradas por Azure](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: use los informes de seguridad y la supervisión de Azure Active Directory (Azure AD) para detectar cuándo se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Administración de los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: use ubicaciones con nombre de Azure Active Directory (Azure AD) para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre de Azure AD](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones**: Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso e identidades de Azure AD para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring/)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión de Azure Active Directory (Azure AD), auditoría y riesgo, que permiten la integración con cualquier solución de administración de eventos e información del sistema o una herramienta de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Instrucciones**: use las características de protección de identidad de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas que se detecten relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

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

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía**: El control de acceso basado en rol de Azure (Azure RBAC) permite realizar una administración detallada del acceso de usuarios, grupos y recursos de Azure. Con RBAC de Azure, puede conceder el nivel de acceso que necesitan los usuarios. 

En Azure DNS, existe el rol Colaborador de zona DNS, así como un RBAC de Azure de nivel de zona y de nivel de conjunto de registros. 

También es posible crear roles de Azure de cliente propios, para proporcionar un control más detallado. 

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md) 

- [Descripción de RBAC de Azure en Azure DNS](https://docs.microsoft.com/azure/dns/dns-protect-zones-recordsets#azure-role-based-access-control)

- [Descripción de RBAC de Azure en Azure DNS privado](dns-protect-private-zones-recordsets.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas cuando se produzcan cambios en Azure DNS, así como en otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) en las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos en las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Azure Resource Graph Explorer, se recomienda crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Utilice el nombre, la descripción y la categoría de la directiva para organizar lógicamente los recursos según una taxonomía.

Para más información sobre el etiquetado de recursos, consulte la documentación sobre las decisiones de nomenclatura y etiquetado de recursos. 

- [Guía de decisiones de nomenclatura y etiquetado de recursos](/azure/cloud-adoption-framework/decision-guides/resource-tagging)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones.

Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

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

**Guía**: Defina e implemente configuraciones de seguridad estándar para Azure DNS con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los almacenes de Recovery Services.

- [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use las directivas [deny] y [deploy if not exist] de Azure Policy para aplicar una configuración segura en los recursos de Azure.

Además, Azure Resource Manager admite otro tipo de control de seguridad: la posibilidad de bloquear recursos. Los bloqueos de recursos se aplican al recurso y están en vigor en todos los usuarios y roles. Existen dos tipos de bloqueos de recursos: CanNotDelete y ReadOnly.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Protección frente a los cambios en Azure DNS](dns-protect-zones-recordsets.md)

- [Protección frente a los cambios en Azure DNS privado](dns-protect-private-zones-recordsets.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Si usa definiciones personalizadas de Azure Policy, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Network" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Network" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Utilice los efectos de la directiva de Azure Policy [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (como Azure DNS), pero no se ejecuta en el contenido del cliente.

Es su responsabilidad realizar un análisis previo del contenido que se carga en recursos de Azure que no son de proceso. Microsoft no puede acceder a los datos del cliente y, por lo tanto, no puede realizar exámenes antimalware del contenido del cliente en su nombre.

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

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

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
