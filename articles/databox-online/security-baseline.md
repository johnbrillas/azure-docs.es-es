---
title: Base de referencia de seguridad de Azure para Azure Stack Edge
description: La base de referencia de seguridad de Azure Stack Edge proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: databox
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 460fd66ed4651248639334caa55eb8facbce866d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452380"
---
# <a name="azure-security-baseline-for-azure-stack-edge"></a>Base de referencia de seguridad de Azure para Azure Stack Edge

Esta base de referencia de seguridad aplica la guía de la [versión 2.0 de Azure Security Benchmark](../security/benchmarks/overview.md) a Microsoft Azure Stack Edge. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y la guía relacionada aplicable a Azure Stack Edge. Se han excluido los **controles** no aplicables a Azure Stack Edge.

Para ver cómo Azure Stack Edge se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de base de referencia de seguridad de Azure Stack Edge](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

**Guía**: Los clientes implementan un dispositivo de Azure Stack Edge físico proporcionado por Microsoft en su red privada para el acceso interno y tienen opciones para protegerlo mejor. Por ejemplo, el dispositivo de Azure Stack Edge es accesible a través de la red interna del cliente y requiere una dirección IP configurada por este. Además, el cliente elige una contraseña de acceso a la interfaz de usuario del dispositivo. 

El tráfico interno se protege mejor de la manera siguiente:

- El protocolo de seguridad de la capa de transporte (TLS) versión 1.2 es necesario para la administración de Azure Portal y del SDK del dispositivo de Azure Stack Edge.

- Cualquier acceso de cliente al dispositivo se realiza a través de la interfaz de usuario web local mediante TLS 1.2 estándar como protocolo seguro predeterminado.

- Solo los dispositivos de Azure Stack Edge Pro autorizados pueden unirse al servicio Azure Stack Edge que el cliente crea en su suscripción de Azure.

Información adicional disponible en los vínculos indicados.
 
- [Configuración de TLS 1.2 en los clientes de Windows que acceden al dispositivo Azure Stack Edge Pro con GPU](azure-stack-edge-j-series-configure-tls-settings.md)

- [Inicio rápido: Introducción a Azure Stack Edge Pro con GPU](azure-stack-edge-gpu-quickstart.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas

**Guía**: Los clientes pueden elegir una red privada virtual (VPN) de punto a sitio para conectar un dispositivo de Azure Stack Edge desde su red privada local hasta la red de Azure. La VPN proporciona una segunda capa de cifrado para los datos que se mueven por la capa de seguridad de transporte del dispositivo del cliente a Azure. 

Los clientes pueden configurar una red privada virtual en su dispositivo de Azure Stack Edge mediante Azure Portal o Azure PowerShell.

- [Configuración de VPN de Azure mediante el script de Azure PowerShell para Azure Stack Edge Pro R y Azure Stack Edge Mini R](azure-stack-edge-mini-r-configure-vpn-powershell.md)

- [Configuración de TLS 1.2 en los clientes de Windows que acceden al dispositivo Azure Stack Edge Pro con GPU](azure-stack-edge-j-series-configure-tls-settings.md)

- [Tutorial: Configuración de certificados para un dispositivo Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: establecimiento del acceso de red privada a los servicios de Azure

**Guía**: Los clientes pueden elegir una red privada virtual (VPN) de punto a sitio para conectar un dispositivo de Azure Stack Edge desde su red privada local hasta la red de Azure. La VPN proporciona una segunda capa de cifrado para los datos que se mueven por la capa de seguridad de transporte del dispositivo del cliente a Azure. 

- [Configuración de VPN de Azure mediante el script de Azure PowerShell para Azure Stack Edge Pro R y Azure Stack Edge Mini R](azure-stack-edge-mini-r-configure-vpn-powershell.md)

- [Configuración de TLS 1.2 en los clientes de Windows que acceden al dispositivo Azure Stack Edge Pro con GPU](azure-stack-edge-j-series-configure-tls-settings.md)

- [Tutorial: Configuración de certificados para un dispositivo Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

**Guía**: El dispositivo de Azure Stack Edge incorpora características de protección de red estándar de Windows Server, que no son configurables por los clientes.

Los clientes pueden optar por proteger su red privada conectada con el dispositivo de Azure Stack Edge contra ataques externos mediante una aplicación virtual de red, como un firewall con protecciones avanzadas de denegación de servicio distribuido (DDoS).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

**Guía**: Microsoft administra todos los puntos de conexión que el dispositivo de Azure Stack Edge usa. Los clientes son responsables de cualquier control adicional que deseen implementar en sus sistemas locales.

El dispositivo de Azure Stack Edge usa sus propias características de detección de intrusiones para proteger el servicio. 

- [Descripción de la seguridad de Azure Stack Edge](azure-stack-edge-security.md)

- [Información de puerto para Azure Stack Edge](azure-stack-edge-gpu-system-requirements.md)

- [Obtención de registros de detección de intrusiones de hardware y software](azure-stack-edge-gpu-troubleshoot.md#gather-advanced-security-logs)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Administración de identidades de aplicaciones de forma segura y automática

**Guía**: Todos los dispositivos de Azure Stack Edge tienen automáticamente una identidad administrada asignada por el sistema en Azure Active Directory (Azure AD). Actualmente, la identidad administrada se usa para la administración en la nube de las máquinas virtuales hospedadas en Azure Stack Edge.

Los dispositivos de Azure Stack Edge arrancan en un estado bloqueado para el acceso local. Con la cuenta de administrador de dispositivos local, tendrá que conectarse a su dispositivo mediante la interfaz de usuario web local o la interfaz de PowerShell y establecer una contraseña segura. Almacene y administre la credencial del administrador de dispositivos en una ubicación segura, como Azure Key Vault, y rote la contraseña de administrador de acuerdo con los estándares de la organización.

- [Protección de un dispositivo de Azure Stack Edge a través de la contraseña](azure-stack-edge-security.md#protect-the-device-via-password)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Uso del inicio de sesión único de Azure AD para acceder a las aplicaciones

**Guía**: No se admite el inicio de sesión único en los dispositivos de punto de conexión de Azure Stack Edge. Sin embargo, puede optar por habilitar el inicio de sesión único estándar basado en Azure Active Directory (Azure AD) para proteger el acceso a los recursos de nube de Azure.

- [Información sobre el inicio de sesión único (SSO) en aplicaciones con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía**: La autenticación multifactor es un control de autenticación seguro, pero una característica opcional para los usuarios del servicio Azure Stack Edge. Se puede usar en escenarios admitidos, como la administración perimetral de dispositivos de Azure Stack Edge en Azure Portal. Tenga en cuenta que el acceso local a los dispositivos de Azure Stack Edge no admite la autenticación multifactor.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Supervisión y alerta de anomalías de cuenta

**Guía**: Habilite Azure Monitor para recopilar registros de contenedores o dispositivos para el servicio Azure Stack Edge. Supervise los contenedores, como los que ejecutan varias aplicaciones de proceso en el clúster de Kubernetes en el dispositivo.

Además, se puede recopilar un paquete de soporte técnico, que incluye los registros de auditoría, en la interfaz de usuario web local del dispositivo de Azure Stack Edge. Tenga en cuenta que el paquete de soporte técnico no está disponible en Azure Portal.
 
- [Habilitación de Azure Monitor en un dispositivo de Azure Stack Edge Pro](azure-stack-edge-gpu-enable-azure-monitor.md) 

- [Recopilación de un paquete de soporte técnico](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Restricción del acceso a recursos de Azure en función de las condiciones

**Guía**: El acceso condicional de Azure Active Directory (Azure AD) es una característica opcional para la autenticación en el servicio Azure Stack Edge. El servicio Azure Stack Edge es un recurso de Azure Portal que le permite administrar un dispositivo de Azure Stack Edge Pro en diferentes ubicaciones geográficas. 

- [¿Qué es el acceso condicional?](../active-directory/conditional-access/overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Elimine la exposición de credenciales no intencionada

**Guía**: Siga los procedimientosrecomendadospara proteger las credenciales, como:

- Uso de una clave de activación para activar el dispositivo con el recurso de Azure Stack Edge en Azure.
- Credenciales de inicio de sesión para acceder al dispositivo de Azure Stack Edge.
- Archivos de claves que pueden facilitar la recuperación de un dispositivo de Azure Stack Edge.
- Clave de cifrado de canal

Rote y, a continuación, sincronice las claves de la cuenta de almacenamiento periódicamente para proteger su cuenta de almacenamiento de usuarios no autorizados.

- [Seguridad y protección de datos de Azure Stack Edge Pro](azure-stack-edge-security.md)

- [Sincronización de claves de almacenamiento](azure-stack-edge-manage-shares.md#sync-storage-keys)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía**: La solución Azure Stack Edge tiene varios componentes con controles de acceso seguro para restringir el acceso a los dispositivos críticos para la empresa. Para configurar y administrar los dispositivos, su organización necesitará un Contrato Enterprise (EA), un proveedor de soluciones en la nube (CSP) o una suscripción a Patrocinio de Microsoft Azure: 

El servicio Azure Stack Edge está protegido por las características de seguridad de Azure como un servicio de administración hospedado en Azure. Puede obtener la clave de cifrado de su recurso en las propiedades del dispositivo de cualquier operación de administración del kit de desarrollo de software, pero solo puede verla si tiene los permisos adecuados para Resource Graph API.

El dispositivo Azure Stack Edge Pro es un dispositivo local que ayuda a transformar los datos, ya que los procesas localmente y, después, los envía a Azure. El dispositivo:

- Necesita una clave de activación para acceder al servicio Azure Stack Edge.
- Está protegido en todo momento mediante una contraseña de dispositivo.
- Tiene el arranque seguro habilitado.

- Es un dispositivo bloqueado. El controlador de administración de placa base (BMC) del dispositivo y el BIOS están protegidos mediante contraseña. El BIOS está protegido por acceso de usuario limitado.
- Ejecuta Device Guard de Windows Defender. Device Guard le permite ejecutar solo las aplicaciones de confianza que estén definidas en las directivas de integridad de código.

Siga los procedimientos recomendados para proteger todas las credenciales y los secretos que se usan para acceder a Azure Stack Edge. 

- [Prácticas recomendadas para contraseñas](azure-stack-edge-security.md#protect-the-device-via-password)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Revisión y conciliación de manera periódica del acceso de los usuarios

**Guía**: Azure Stack Edge tiene un usuario llamado "EdgeUser" que puede configurar el dispositivo. También tiene el usuario "EdgeArmUser" de Azure Resource Manager para las características locales de Azure Resource Manager del dispositivo. 

Se deben seguir los procedimientos recomendados para proteger:

- Las credenciales usadas para acceder al dispositivo local

- Las credenciales de recursos compartidos de SMB

- El acceso a los sistemas cliente que se han configurado para usar recursos compartidos de NFS

- Las claves empleadas para acceder a las cuentas de almacenamiento local cuando se usa la API REST del blob.

Se puede encontrar información adicional en el vínculo al que se hace referencia.

- [Información sobre la seguridad de los dispositivos de Azure Stack Edge](azure-stack-edge-security.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Uso de estaciones de trabajo con privilegios de acceso

**Guía**: De cara a la seguridad de los roles confidenciales, como administradores, desarrolladores y operadores de servicios críticos, es importante que las estaciones de trabajo sean seguras y estén aisladas. Use estaciones de trabajo de usuario de alta seguridad con o sin Azure Bastion para las tareas administrativas. Use Azure Active Directory (Azure AD), Protección contra amenazas avanzada (ATP) de Microsoft Defender y Microsoft Intune para implementar una estación de trabajo de usuario segura y administrada para las tareas administrativas. 

Las estaciones de trabajo protegidas se pueden administrar de forma centralizada para aplicar una configuración segura, como autenticación sólida, líneas de base de software y hardware y acceso lógico y de red restringido.

- [Descripción de las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [Implementación de una estación de trabajo con privilegios de acceso](/security/compass/privileged-access-deployment)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-2-protect-sensitive-data"></a>DP-2: Protección de datos confidenciales

**Guía**: Azure Stack Edge trata todos los datos interactivos como confidenciales, y solo los usuarios autorizados tienen acceso a estos datos. Para proteger las credenciales usadas para acceder al servicio Azure Stack Edge, debe seguir los procedimientos recomendados.

- [Seguridad y protección de datos de Azure Stack Edge Pro](azure-stack-edge-security.md#protect-the-device-via-password)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

**Guía**: Azure Stack Edge usa canales seguros para los datos en tránsito. Dichos componentes son:

- Se utiliza TLS 1.2 estándar para los datos que circulan entre el dispositivo y la nube de Azure. No hay ninguna reserva para TLS 1.1 y versiones anteriores. Si no se admite TLS 1.2, se bloqueará la comunicación del agente. TLS 1.2 también es necesario para la administración de Azure Portal y del kit de desarrollo de software (SDK).

- Cuando los clientes acceden al dispositivo mediante la interfaz de usuario web local de un explorador, se utiliza TLS 1.2 estándar como protocolo seguro predeterminado.

El procedimiento recomendado consiste en configurar el explorador para usar TLS 1.2. Use SMB 3.0 con cifrado para proteger los datos cuando se copien de los servidores de datos.

- [Procedimientos recomendados para proteger los datos en tránsito](azure-stack-edge-security.md#protect-data-in-flight)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="asset-management"></a>Administración de recursos

*Para más información, consulte [Azure Security Benchmark: Administración de recursos](../security/benchmarks/security-controls-v2-asset-management.md).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Asegurarse de que el equipo de seguridad tiene visibilidad sobre los riesgos para los recursos

**Guía**: Asegúrese de que se conceden a los equipos de seguridad permisos de lector de seguridad en el inquilino y las suscripciones de Azure para que puedan supervisar los riesgos de seguridad mediante Azure Security Center. 

En función de la estructura de las responsabilidades del equipo de seguridad, la supervisión de los riesgos de seguridad puede ser responsabilidad de un equipo de seguridad central o de un equipo local. Dicho esto, la información y los riesgos de seguridad siempre se deben agregar de forma centralizada dentro de una organización. 

Los permisos de lector de seguridad se pueden aplicar en general a un inquilino completo (grupo de administración raíz) o a grupos de administración o a suscripciones específicas. 

Es posible que se requieran permisos adicionales para obtener visibilidad sobre las cargas de trabajo y los servicios. 

- [Introducción al rol de lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader)

- [Información general sobre los grupos de administración de Azure](../governance/management-groups/overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Solo los usuarios autorizados (por ejemplo, "EdgeArmUser") pueden acceder a las API de dispositivos de Azure Stack Edge a través de la instancia local de Azure Resource Manager. Las contraseñas de cuentas de usuario solo se pueden administrar en Azure Portal. 

- [Establecimiento de la contraseña de Azure Resource Manager](azure-stack-edge-j-series-set-azure-resource-manager-password.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-6-use-only-approved-applications-in-compute-resources"></a>AM-6: Uso exclusivo de aplicaciones aprobadas en recursos de proceso

**Guía**: Puede traer sus propias aplicaciones para que se ejecuten en cualquier máquina virtual creada localmente. Use scripts de PowerShell para crear máquinas virtuales de proceso locales en el dispositivo de Stack Edge. Se recomienda encarecidamente que solo se ejecuten aplicaciones de confianza en las máquinas virtuales locales. 

- [Control de la ejecución de scripts de PowerShell en entornos Windows](/powershell/module/microsoft.powershell.security/set-executionpolicy)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](../security/benchmarks/security-controls-v2-logging-threat-detection.md).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Habilitación de la detección de amenazas para recursos de Azure

**Guía**: Azure Stack Edge no ofrece funcionalidades de detección de amenazas. Sin embargo, los clientes pueden recopilar registros de auditoría en un paquete de soporte técnico para el análisis y la detección de amenazas sin conexión. 

- [Recopilación de paquetes de soporte técnico para dispositivos de Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

**Guía**: Azure Stack Edge tiene habilitados registros de auditoría de red como parte del paquete de soporte técnico descargable. Estos registros se pueden analizar para implementar una supervisión en tiempo real en los dispositivos de Azure Stack Edge.

- [Recopilación de un paquete de soporte técnico de Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía**: Actualmente no se admite la supervisión en tiempo real con registros en Azure Stack Edge. Existe una funcionalidad para recopilar un paquete de soporte técnico que le permite analizar los distintos registros que contiene, como los de firewall, software, intrusiones de hardware y eventos del sistema del dispositivo de Azure Stack Edge Pro. Tenga en cuenta que los registros de intrusiones de software o de firewall predeterminados se recopilan para el tráfico entrante y saliente.

- [Recopilación de un paquete de soporte técnico para Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía**: Actualmente no se admite la supervisión en tiempo real con registros en Azure Stack Edge. Sin embargo, puede recopilar un paquete de soporte técnico que le permita analizar los distintos registros que contiene.  El paquete de soporte técnico se comprime y se descarga en la ruta de acceso de su elección. Descomprima el paquete y vea los archivos de registro del sistema que contiene. También puede enviar estos registros a una herramienta de administración de eventos de información de seguridad u otra ubicación de almacenamiento central para su análisis.

- [Recopilación de un paquete de soporte técnico para Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Configuración de la retención del almacenamiento de registros

**Guía**: No se puede cambiar el período de retención de almacenamiento de registros en el dispositivo de Azure Stack Edge. Los registros más antiguos se purgan según sea necesario. Puede recopilar paquetes de soporte técnico del dispositivo a intervalos periódicos si existe la necesidad de conservar los registros durante un período más largo. 

- [Recopilación de un paquete de soporte técnico para Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-7-use-approved-time-synchronization-sources"></a>LT-7: Uso de orígenes de sincronización de hora aprobados

**Guía**: Azure Stack Edge usa time.windows.com, un servidor de provisión de tiempo de red de Microsoft.  Azure Stack Edge también permite al cliente configurar el servidor de protocolo de tiempo de red de su elección.

- [Configuración del tiempo del dispositivo de Azure Stack Edge](azure-stack-edge-gpu-deploy-set-up-device-update-time.md#configure-time)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Preparación: actualización del proceso de respuesta a incidentes para Azure

**Guía**: Asegúrese de que el plan de respuesta a incidentes de la organización incluya procesos: 

- para responder a los incidentes de seguridad;

- que se hayan actualizado para la nube de Azure;
 
- que se ejerciten con regularidad para garantizar su preparación.

Se recomienda implementar la seguridad mediante procesos de respuesta a incidentes normalizados en todo el entorno de la empresa.

- [Implementación de la seguridad en todo el entorno empresarial](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de referencia de respuesta a incidentes](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Preparación: notificación de incidentes de configuración

**Guía**: Configure la información de contacto en caso de incidentes de seguridad en Azure Security Center. Microsoft utilizará esta información para ponerse en contacto con usted si el Centro de respuestas de seguridad de Microsoft (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a sus datos. También hay opciones para personalizar la alerta y notificación de incidentes en diferentes servicios de Azure en función de sus necesidades de respuesta a incidentes. 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Detección y análisis: creación de incidentes en función de alertas de alta calidad 

**Guía**: Asegúrese de que cuenta con un proceso para crear alertas de alta calidad y medir su calidad. De esta manera, podrá aprender de los incidentes pasados y clasificar por orden de prioridad las alertas para los analistas, a fin de evitar desperdiciar tiempo en el procesamiento de alertas de falsos positivos. Cree alertas de alta calidad basadas en sus experiencias de incidentes pasados, orígenes validados de la comunidad y herramientas diseñadas para generar y eliminar alertas con el registro y la correlación de diversos orígenes de alertas. 

Azure Security Center proporciona alertas de alta calidad en muchos recursos de Azure. Puede usar el conector de datos de Security Center para transmitir las alertas a Azure Sentinel. Cree reglas de alerta avanzadas con Azure Sentinel con el fin de generar incidentes automáticos para investigar. 

Exporte las alertas y recomendaciones de Security Center a Azure Sentinel mediante la característica de exportación para ayudar a identificar riesgos para los recursos de Azure. Se recomienda crear un proceso para exportar las alertas y recomendaciones de manera automatizada para una seguridad continua.

- [Configuración de la exportación](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Detección y análisis: investigación de incidentes

**Guía**: Asegúrese de que los analistas puedan consultar y usar diversos orígenes de datos para crear una vista completa de la actividad que se produjo al investigar posibles incidentes. Para evitar puntos ciegos, se deben recopilar diversos tipos de registros con el fin de realizar un seguimiento de las actividades de un posible atacante a lo largo de la cadena de eliminación.  Asegúrese también de que se capturan detalles y aprendizajes como referencia histórica.  

Los orígenes de datos para la investigación incluyen los orígenes de registro centralizados que ya se recopilan de los servicios en el ámbito y los sistemas en ejecución, pero también pueden incluir:

- Datos de red: use registros de flujo de grupos de seguridad de red, Azure Network Watcher y Azure Monitor para capturar registros de flujo de red y otra información de análisis. 

- Instantáneas de sistemas en ejecución: 

    - Use la funcionalidad de instantáneas de la máquina virtual de Azure para crear una instantánea del disco del sistema en ejecución. 

    - Elija la funcionalidad nativa de volcado de memoria del sistema operativo para crear una instantánea de la memoria del sistema en ejecución.

    - Use la característica de instantáneas de los servicios de Azure o una funcionalidad de software de terceros para crear instantáneas de los sistemas en ejecución.

Azure Sentinel proporciona amplios análisis de datos en prácticamente cualquier origen de registro y un portal de administración de casos para administrar todo el ciclo de vida de los incidentes. La información de inteligencia durante una investigación puede asociarse a un incidente con fines de seguimiento e informes. 

- [Instantánea del disco de una máquina Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Instantánea del disco de una máquina Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Recopilación del volcado de memoria e información de diagnóstico del servicio de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Investigación de incidentes con Azure Sentinel](../sentinel/tutorial-investigate-cases.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Detección y análisis: clasificar incidentes

**Guía**: Proporcione contexto a los analistas sobre los incidentes en los que deberán centrarse en primer lugar en función de la gravedad de la alerta y la confidencialidad de los recursos. Use la gravedad asignada a cada alerta de Azure Security Center para ayudarle a clasificar por orden de prioridad qué alertas se deben investigar primero. La gravedad se basa en la confianza que tiene Security Center en el hallazgo o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque los recursos con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Contención, erradicación y recuperación: automatización del control de incidentes

**Guía**: Automatice las tareas repetitivas manuales para acelerar el tiempo de respuesta y reducir la carga de los analistas. Las tareas manuales tardan más tiempo en ejecutarse, lo que ralentiza cada incidente y reduce el número de incidentes que un analista puede manejar. Las tareas manuales también aumentan la fatiga del analista, lo que aumenta el riesgo del error humano que produce retrasos y reduce la capacidad de los analistas de centrarse de manera efectiva en tareas complejas. Use las características de automatización de flujo de trabajo de Azure Security Center y Azure Sentinel para desencadenar acciones automáticamente o ejecutar un cuaderno de estrategias para responder a las alertas de seguridad entrantes. El cuaderno de estrategias lleva a cabo acciones, como el envío de notificaciones, la deshabilitación de cuentas y el aislamiento de redes problemáticas. 

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Security Center](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="posture-and-vulnerability-management"></a>administración de posturas y vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de posturas y vulnerabilidades](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md).*

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>PV-3: establecimiento de configuraciones seguras para los recursos de proceso

**Guía**: Azure Stack Edge ofrece compatibilidad con la creación de configuraciones seguras para las máquinas virtuales locales creadas por los clientes. Se recomienda encarecidamente usar las directrices proporcionadas por Microsoft para establecer bases de referencia de seguridad al crear máquinas virtuales locales.

- [Descarga de las bases de referencia de seguridad incluidas en el kit de herramientas de cumplimiento de seguridad](/windows/security/threat-protection/windows-security-baselines)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pv-4-sustain-secure-configurations-for-compute-resources"></a>PV-4: sostenimiento de configuraciones seguras para los recursos de proceso

**Guía**: Azure Stack Edge no ofrece compatibilidad con el mantenimiento de configuraciones seguras para las máquinas virtuales locales creadas por los clientes. Se recomienda encarecidamente que los clientes usen los kits de herramientas de cumplimiento de seguridad (SCT) para ayudar a mantener configuraciones seguras para sus recursos de proceso.

El sistema operativo host y las máquinas virtuales administradas por Azure Stack Edge mantienen sus configuraciones de seguridad. 

- [Líneas base de seguridad de Windows](/windows/security/threat-protection/windows-security-baselines#using-security-baselines-in-your-organization)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="pv-5-securely-store-custom-operating-system-and-container-images"></a>PV-5: Almacenamiento seguro de las imágenes de contenedor y de sistema operativo personalizadas

**Guía**: Los sistemas operativos host y las máquinas virtuales administradas por Azure Stack Edge se almacenan de forma segura. 

Los clientes pueden traer sus propias imágenes de contenedores y máquinas virtuales y son responsables de su administración segura.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>PV-7: corrección rápida y automática de vulnerabilidades de software

**Guía**: Azure Stack Edge proporciona actualizaciones periódicas de revisiones y avisa a los clientes cuando dichas actualizaciones están disponibles para corregir las vulnerabilidades. Es responsabilidad del cliente mantener sus dispositivos y máquinas virtuales (creados por ellos) al día con las revisiones más recientes.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: realización de una simulaciones de ataques periódicas

**Guía**: Según sea necesario, realice pruebas de penetración o actividades de equipo rojo en los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos.
siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Pruebas de penetración en Azure](../security/fundamentals/pen-testing.md)

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="endpoint-security"></a>seguridad de los puntos de conexión

*Para más información, consulte [Azure Security Benchmark: seguridad de los puntos de conexión](../security/benchmarks/security-controls-v2-endpoint-security.md).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uso de la detección y respuesta de puntos de conexión (EDR)

**Guía**: Azure Stack Edge no es compatible directamente con la detección de puntos de conexión y respuesta (EDR). Sin embargo, se puede recopilar un paquete de soporte técnico y recuperar los registros de auditoría. Después, estos registros se pueden analizar para comprobar las actividades anómalas. 

- [Recopilación de paquetes de soporte técnico para dispositivos de Azure Stack Edge](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: Uso de software antimalware moderno administrado centralmente

**Guía**: Azure Stack Edge usa Windows Defender Application Control (WDAC) con una directiva estricta de integridad del código (CI) que solo permite la ejecución de aplicaciones y scripts predeterminados. El antimalware de protección en tiempo real (RTP) de Windows Defender también está habilitado. El cliente puede optar por ejecutar antimalware en las máquinas virtuales de proceso que crean para ejecutarse localmente en el dispositivo de Azure Stack Edge.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="backup-and-recovery"></a>Copia de seguridad y recuperación

*Para más información, consulte [Azure Security Benchmark: Copia de seguridad y recuperación](../security/benchmarks/security-controls-v2-backup-recovery.md).*

### <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Garantía de copias de seguridad automáticas periódicas

**Guía**: Se recomienda realizar copias de seguridad periódicas del dispositivo de Azure Stack Edge; estas copias se pueden realizar con Azure Backup y otras soluciones de protección de datos de terceros para salvaguardar los datos contenidos en las máquinas virtuales implementadas en el dispositivo. 

Las soluciones de protección de datos de terceros, como Cohesity, Commvault y Veritas, también pueden proporcionar una opción de copia de seguridad para los datos de los recursos compartidos de SMB o NFS locales. 

Información adicional disponible en los vínculos indicados.

- [Preparación para un error en un dispositivo](azure-stack-edge-gpu-prepare-device-failure.md)

- [Protección de archivos y carpetas en máquinas virtuales](azure-stack-edge-gpu-prepare-device-failure.md#protect-files-and-folders-on-vms)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="br-2-encrypt-backup-data"></a>BR-2: Cifrado de los datos de copia de seguridad

**Guía**: Asegúrese de que las copias de seguridad están protegidas frente a los ataques. Esto debe incluir el cifrado de las copias de seguridad para proteger frente a la pérdida de confidencialidad.  Para más información, consulte la solución de copia de seguridad que prefiera para conocer los detalles.

- [Protección de datos de recursos compartidos locales de Edge](azure-stack-edge-gpu-prepare-device-failure.md#protect-data-in-edge-local-shares)

- [Protección de archivos y carpetas en máquinas virtuales](azure-stack-edge-gpu-prepare-device-failure.md#protect-files-and-folders-on-vms)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: Realice periódicamente la restauración de los datos de las copias de seguridad. 

- [Procedimientos de recuperación para Azure Stack Edge](azure-stack-edge-gpu-recover-device-failure.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Mitigación del riesgo de pérdida de claves

**Guía**: Asegúrese de que todas las copias de seguridad de Azure Stack Edge, incluidas las claves administradas por el cliente, estén protegidas según los procedimientos recomendados de la organización. El dispositivo de Azure Stack Edge está asociado a una cuenta de Azure Storage, que se usa como repositorio de destino de los datos en Azure. 

El acceso a la cuenta de Azure Storage se controla mediante las suscripciones de Azure y las dos claves de acceso de almacenamiento de 512 bits asociadas a ella. Una de las claves se utiliza para la autenticación cuando el dispositivo de Azure Stack Edge accede a la cuenta de almacenamiento. La otra se mantiene en reserva para la rotación de claves periódica. Asegúrese de que se usan los procedimientos recomendados de seguridad para la rotación de claves.

- [Protección de los datos de dispositivos de Azure Stack Edge en las cuentas de Azure Storage](azure-stack-edge-pro-r-security.md#protect-data-in-storage-accounts)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="governance-and-strategy"></a>Gobernanza y estrategia

*Para más información, consulte [Azure Security Benchmark: gobernanza y estrategia](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definición de la estrategia de protección de datos y administración de recursos 

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para la protección y supervisión continua de sistemas y datos. Dé prioridad a la detección, evaluación, protección y supervisión de los sistemas y datos críticos para la empresa. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Norma de clasificación de datos de acuerdo con los riesgos empresariales

-   Visibilidad en la organización de seguridad de los riesgos y el inventario de recursos 

-   Aprobación de la organización de seguridad de los servicios de Azure para su uso 

-   Seguridad de los recursos durante su ciclo de vida

-   Estrategia de control de acceso necesaria según la clasificación de datos de la organización

-   Uso de las funcionalidades de protección de datos nativa de Azure y de terceros

-   Requisitos de cifrado de datos para casos de uso en tránsito y en reposo

-   Normas criptográficas adecuadas

Para más información, consulte las siguientes referencias:
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Security Benchmark: administración de recursos](../security/benchmarks/security-controls-v2-asset-management.md)

- [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definición de una estrategia de segmentación empresarial 

**Guía**: Establezca en toda la empresa una estrategia para segmentar el acceso a los recursos mediante una combinación de identidad, red, aplicación, suscripción, grupo de administración y otros controles.

Equilibre concienzudamente la necesidad de separación de seguridad con la necesidad de habilitar el funcionamiento diario de los sistemas que necesitan comunicarse entre sí y acceder a los datos.

Asegúrese de que la estrategia de segmentación se implementa de forma coherente en todos los tipos de control, como la seguridad de red, los modelos de identidad y acceso, y los modelos de acceso y permisos de las aplicaciones, y los controles de los procesos humanos.

- [Guía de la estrategia de segmentación en Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Guía de la estrategia de segmentación en Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alineación de la segmentación de red con la estrategia de segmentación empresarial](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definición de la estrategia de administración de la posición de seguridad

**Guía**: Mida y mitigue continuamente los riesgos de los recursos individuales y el entorno en el que se hospedan. Dé prioridad a los recursos de gran valor y a las superficies de ataque muy expuestas, como las aplicaciones publicadas, los puntos de entrada y salida de red, los puntos de conexión de usuario y administrador, etc.

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Alineación de los roles y responsabilidades de la organización

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para los roles y las responsabilidades de la organización de seguridad. Dé prioridad a ofrecer una responsabilidad clara en las decisiones de seguridad, a proporcionar a todos los usuarios formación sobre el modelo de responsabilidad compartida y a los equipos técnicos sobre la tecnología para proteger la nube.

- [Procedimiento recomendado de seguridad de Azure 1. Personas: Formación del equipo sobre el recorrido de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Procedimiento recomendado de seguridad de Azure 2. Personas: Formación del equipo en tecnología de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Procedimiento recomendado de seguridad de Azure 3. Proceso: Asignación de responsabilidades por las decisiones de seguridad en la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definición de la estrategia de seguridad de red

**Guía**: Establezca un enfoque de seguridad de red de Azure como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Centralización de la responsabilidad de seguridad y la administración de redes

-   Modelo de segmentación de la red virtual alineado con la estrategia de segmentación empresarial

-   Estrategia de corrección en diferentes escenarios de amenazas y ataques

-   Estrategia de entrada y salida y perimetral de Internet

-   Estrategia de interconectividad entre el entorno local y la nube híbrida

-   Artefactos de seguridad de red actualizados (por ejemplo, diagramas de red y arquitectura de red de referencia)

Para más información, consulte las siguientes referencias:
- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Security Benchmark: seguridad de red](../security/benchmarks/index.yml)

- [Azure Network Security Overview (Información general sobre Azure Network Security)](../security/fundamentals/network-overview.md)

- [Estrategia para la arquitectura de red empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definición de la estrategia de acceso con privilegios e identidades

**Guía**: Establezca una identidad de Azure y enfoques de acceso con privilegios como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Un sistema de identidad y autenticación centralizado y su interconectividad con otros sistemas de identidad internos y externos

-   Métodos de autenticación sólida en diferentes casos de uso y condiciones

-   Protección de usuarios con privilegios elevados

-   Supervisión y control de anomalías en las actividades de los usuarios  

-   Proceso de revisión y conciliación del acceso y la identidad de los usuarios

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md)

- [Azure Security Benchmark: acceso con privilegios](../security/benchmarks/security-controls-v2-privileged-access.md)

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Información general sobre seguridad de administración de identidades de Azure](../security/fundamentals/identity-management-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definición de la estrategia de registro y respuesta a amenazas

**Guía**: Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Para dar prioridad, proporcione a los analistas alertas de alta calidad y experiencias sin problemas para que puedan centrarse en las amenazas en lugar de los pasos manuales y de integración. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Las responsabilidades y el rol de la organización en las operaciones de seguridad (SecOps) 

-   Un proceso de respuesta a incidentes bien definido que esté alineado con NIST u otro marco del sector. 

-   Captura y retención de registros para admitir la detección de amenazas, la respuesta ante incidentes y las necesidades de cumplimiento

-   Visibilidad y correlación centralizada de la información sobre amenazas, mediante SIEM, funcionalidades nativas de Azure y otros orígenes 

-   Plan de comunicación y notificación con sus clientes, proveedores y entidades públicas de interés

-   Uso de plataformas nativas de Azure y de terceros para el tratamiento de incidentes, como el registro y la detección de amenazas, los análisis forenses y la corrección y erradicación de ataques

-   Procesos para controlar incidentes y actividades posteriores a incidentes, como las lecciones aprendidas y la retención de pruebas

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: registro y detección de amenazas](../security/benchmarks/security-controls-v2-logging-threat-detection.md)

- [Azure Security Benchmark: respuesta a incidentes](../security/benchmarks/security-controls-v2-incident-response.md)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
