---
title: Protección de Microsoft 365 contra ataques locales
description: Instrucciones para asegurarse de que un ataque local no afecte a Microsoft 365
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/22/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97893dece068dfdde85159f734095401288231d2
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201355"
---
# <a name="protecting-microsoft-365-from-on-premises-attacks"></a>Protección de Microsoft 365 contra ataques locales

Muchos clientes conectan sus redes corporativas privadas a Microsoft 365 para beneficiar a sus usuarios, dispositivos y aplicaciones. Sin embargo, existen muchas formas bien documentadas de comprometer estas redes privadas. Dado que Microsoft 365 actúa como el "sistema nervioso" de muchas organizaciones, es fundamental protegerlo de una infraestructura local en peligro.

En este artículo se muestra cómo configurar los sistemas para proteger el entorno en la nube de Microsoft 365 frente a riesgos locales. Nos centraremos principalmente en las opciones de configuración de los inquilinos de Azure AD, las maneras en que los inquilinos de Azure AD se pueden conectar de forma segura a los sistemas locales y los compromisos necesarios para usar los sistemas de manera que protejan los sistemas en la nube frente a riesgos locales.

Se recomienda encarecidamente implementar esta guía para proteger el entorno en la nube de Microsoft 365.
> [!NOTE]
> Este artículo se publicó inicialmente como una entrada de blog. Se ha trasladado aquí por motivos de durabilidad y mantenimiento. <br>
Para crear una versión sin conexión de este artículo, utilice la funcionalidad de impresión en PDF del explorador. Aquí puede comprobar con frecuencia si hay actualizaciones.

## <a name="primary-threat-vectors-from-compromised-on-premises-environments"></a>Vectores de amenazas principales de entornos locales en peligro


El entorno en la nube de Microsoft 365 se beneficia de una amplia infraestructura de supervisión y seguridad. El uso del aprendizaje automático y la inteligencia humana que examina el tráfico mundial puede detectar rápidamente los ataques y permitirle volver a realizar la configuración casi en tiempo real. En las implementaciones híbridas que conectan la infraestructura local a Microsoft 365, muchas organizaciones delegan la confianza en componentes locales para tomar decisiones críticas de administración del estado de los objetos de directorio y autenticación.
Desafortunadamente, si el entorno local está en peligro, estas relaciones de confianza dan la oportunidad a los atacantes de comprometer el entorno de Microsoft 365.

Los dos vectores de amenazas principales son las **relaciones de confianza de federación** y la **sincronización de cuentas**. Ambos vectores pueden conceder a un atacante acceso administrativo a la nube.

* Las **relaciones de confianza federadas**, como la autenticación SAML, se usan para autenticarse en Microsoft 365 a través de la infraestructura de identidad local. Si un certificado de firma de tokens de SAML está en peligro, la federación permitiría a cualquier usuario con ese certificado suplantar a cualquier usuario de la nube. **Se recomienda deshabilitar las relaciones de confianza de federación para la autenticación en Microsoft 365 cuando sea posible**.

* La **sincronización de cuentas** se puede usar para modificar los usuarios con privilegios (incluidas sus credenciales) o los grupos que tienen privilegios administrativos en Microsoft 365. **Se recomienda asegurarse de que los objetos sincronizados no tengan privilegios más allá de un usuario en Microsoft 365,** directamente o a través de la inclusión en roles o grupos de confianza. Asegúrese de que estos objetos no tienen ninguna asignación directa o anidada en grupos o roles en la nube de confianza.

## <a name="protecting-microsoft-365-from-on-premises-compromise"></a>Protección de Microsoft 365 contra vulnerabilidades locales


Para abordar los vectores de amenazas descritos anteriormente, se recomienda cumplir los principios que se ilustran a continuación:

![Arquitectura de referencia para proteger Microsoft 365 ](media/protect-m365/protect-m365-principles.png)

*  **Aísle completamente las cuentas de administrador de Microsoft 365**. Deben cumplir los siguientes requisitos:

    * Deben controlarse en Azure AD.

     * Deben autenticarse con Multi-Factor Authentication (MFA).

     *  Deben protegerse con el acceso condicional de Azure AD.

     *  Se debe acceder a ellas solo mediante las estaciones de trabajo administradas de Azure.

Se trata de cuentas de uso restringido. **No debe haber ninguna cuenta local con privilegios administrativos en Microsoft 365**. Para más información, vea esta [información general sobre los roles de administrador de Microsoft 365](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide).
Vea también [Roles para servicios de Microsoft 365 en Azure Active Directory](../roles/m365-workload-docs.md).

*  **Administre dispositivos desde Microsoft 365**. Use la administración de dispositivos móviles (MDM) basados en la nube y unidos a Azure AD para eliminar dependencias en la infraestructura de administración de dispositivos local, que puede comprometer los controles de seguridad y de los dispositivos.

* **Ninguna cuenta local tiene privilegios elevados para Microsoft 365**.
    Las cuentas que tienen acceso a las aplicaciones locales que requieren la autenticación NTLM, LDAP o Kerberos necesitan una cuenta en la infraestructura de identidad local de la organización. Asegúrese de que estas cuentas, incluidas las cuentas de servicio, no se incluyen en los grupos o roles en la nube con privilegios y de que los cambios en estas cuentas no pueden afectar a la integridad de su entorno en la nube. El software local con privilegios no debe tener la capacidad de afectar a los roles ni a las cuentas con privilegios de Microsoft 365.

*  **Use la autenticación en la nube de Azure AD** para eliminar las dependencias de las credenciales locales. Use siempre la autenticación sólida, como Windows Hello, FIDO, Microsoft Authenticator o Azure AD MFA.

## <a name="specific-recommendations"></a>Recomendaciones específicas


En las secciones siguientes se proporcionan instrucciones específicas sobre cómo implementar los principios descritos anteriormente.

### <a name="isolate-privileged-identities"></a>Aislamiento de identidades con privilegios


En Azure AD, los usuarios con roles con privilegios, como los administradores, son la raíz de confianza para compilar y administrar el resto del entorno. Implemente las siguientes prácticas para minimizar el impacto de un riesgo.

* Use cuentas solo en la nube para los roles con privilegios de Azure AD y Microsoft 365.

* Implemente [dispositivos de acceso con privilegios](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) para el acceso con privilegios para administrar Microsoft 365 y Azure AD.

*  Implemente [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) (PIM) para el acceso Just-in-Time (JIT) a todas las cuentas de usuarios que tienen roles con privilegios y que requieren una autenticación sólida para activar los roles.

* Proporcione a los roles administrativos los [privilegios mínimos posibles para realizar sus tareas](../roles/delegate-by-task.md).

* Para habilitar una experiencia de asignación de roles más enriquecida que incluye la delegación y varios roles al mismo tiempo, considere la posibilidad de usar grupos de seguridad de Azure AD o grupos de Microsoft 365 (de forma colectiva, "grupos en la nube") y [habilitar el control de acceso basado en rol](../roles/groups-assign-role.md). También puede usar [unidades administrativas](../roles/administrative-units.md) para restringir el ámbito de los roles a una parte de la organización.

* Implemente [cuentas de acceso de emergencia](../roles/security-emergency-access.md) y NO use almacenes de contraseñas locales para almacenar las credenciales.

Para obtener más información, vea [Protección del acceso con privilegios](https://aka.ms/SPA), que tiene instrucciones detalladas sobre este tema. Vea también [Procedimientos de acceso seguro para administradores en Azure AD](../roles/security-planning.md).

### <a name="use-cloud-authentication"></a>Uso de la autenticación en la nube 

Las credenciales son un vector de ataque principal. Implemente los siguientes procedimientos para mejorar la seguridad de las credenciales.

* [Implementación de autenticación sin contraseña](../authentication/howto-authentication-passwordless-deployment.md): reduzca el uso de contraseñas tanto como sea posible mediante la implementación de credenciales sin contraseña. Estas credenciales se administran y validan de forma nativa en la nube. Elija de entre las siguientes opciones:

   * [Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/passwordless-strategy)

   * [Aplicación Authenticator](../authentication/howto-authentication-passwordless-phone.md)

   * [Llaves de seguridad FIDO2](../authentication/howto-authentication-passwordless-security-key-windows.md)

* [Implementación de Multi-Factor Authentication](https://aka.ms/deploymentplans/mfa): aprovisione [varias credenciales seguras mediante Azure AD MFA](../fundamentals/resilience-in-credentials.md). De este modo, el acceso a los recursos en la nube requerirá una credencial administrada en Azure AD, además de una contraseña local que se pueda manipular.

   * Para más información, vea [Crear una estrategia de administración de control de acceso resistente con Azure Active Directory](https://aka.ms/resilientaad).

**Limitaciones y compromisos**

* La administración de contraseñas de cuentas híbrida requiere componentes híbridos, como agentes de protección de contraseñas y agentes de escritura diferida de contraseñas Si la infraestructura local está en peligro, los atacantes pueden controlar los equipos en los que residen estos agentes. Aunque esto no pondrá en peligro la infraestructura en la nube, las cuentas en la nube no protegerán estos componentes frente al riesgo local.

*  Las cuentas locales sincronizadas desde Active Directory se marcan para que no expiren nunca en Azure AD, según la suposición de que las directivas de contraseñas de AD locales mitigarán esto. Si su instancia de AD local se ve comprometida y es necesario deshabilitar la sincronización de AD Connect, debe establecer la opción [EnforceCloudPasswordPolicyForPasswordSyncedUsers](../hybrid/how-to-connect-password-hash-synchronization.md).

## <a name="provision-user-access-from-the-cloud"></a>Aprovisionamiento de acceso de usuarios desde la nube

El aprovisionamiento se refiere a la creación de cuentas de usuario y grupos en aplicaciones o proveedores de identidades.

![Diagrama de la arquitectura de aprovisionamiento](media/protect-m365/protect-m365-provision.png)

* **Aprovisionamiento de aplicaciones de RR. HH. en la nube en Azure AD**: esto permite aislar un riesgo local sin interrumpir el ciclo de tipo alta, baja o traslado desde las aplicaciones de Recursos Humanos en la nube a Azure AD.

* **Aplicaciones en la nube**: siempre que sea posible, implemente el [aprovisionamiento de aplicaciones de Azure AD](../app-provisioning/user-provisioning.md) en lugar de las soluciones de aprovisionamiento local. Esto protegerá a algunas aplicaciones SaaS de los daños provocados con perfiles de usuario malintencionados debido a las vulneraciones locales. 

* **Identidades externas**: use la [colaboración B2B de Azure AD](../external-identities/what-is-b2b.md).
    Esto reducirá la dependencia de las cuentas locales para la colaboración externa con asociados, clientes y proveedores. Evalúe detenidamente cualquier federación directa con otros proveedores de identidades. Se recomienda limitar las cuentas de invitado de B2B de las siguientes maneras.

   *  Limitar el acceso de invitado a los grupos de exploración y otras propiedades del directorio. Use la configuración de colaboración externa para restringir la capacidad del invitado para leer los grupos de los que no son miembros. 

    *   Bloquear el acceso a Azure Portal. Puede hacer excepciones raras, pero necesarias.  Cree una directiva de acceso condicional que incluya a todos los usuarios invitados y externos y luego [implemente una directiva para bloquear el acceso](/azure/role-based-access-control/conditional-access-azure-management). 

* **Bosques desconectados**: use el [aprovisionamiento en la nube de Azure AD](../cloud-provisioning/what-is-cloud-provisioning.md). Esto le permite conectarse a los bosques desconectados, eliminando la necesidad de establecer una conexión o relaciones de confianza entre los bosques, lo que puede ampliar el impacto de una vulneración local. * 
 
**Limitaciones y compromisos**:

* Cuando se usan para aprovisionar cuentas híbridas, Azure AD en los sistemas de Recursos Humanos en la nube se basa en la sincronización local para completar el flujo de datos de AD a Azure AD. Si se interrumpe la sincronización, los nuevos registros de empleados no estarán disponibles en Azure AD.

## <a name="use-cloud-groups-for-collaboration-and-access"></a>Uso de los grupos en la nube para la colaboración y el acceso

Los grupos en la nube permiten desacoplar la colaboración y el acceso de la infraestructura local.

* **Colaboración**: use Grupos de Microsoft 365 y Microsoft Teams para la colaboración moderna. Retire las listas de distribución locales y [actualice las listas de distribución a Grupos de Microsoft 365 en Outlook](https://docs.microsoft.com/office365/admin/manage/upgrade-distribution-lists?view=o365-worldwide).

* **Acceso**: use grupos de seguridad de Azure AD o Grupos de Microsoft 365 para autorizar el acceso a aplicaciones en Azure AD.
* **Licencias de Office 365**: use las licencias basadas en grupos para aprovisionar en Office 365 usando grupos solo en la nube. Esto desacopla el control de la pertenencia a grupos de la infraestructura local.

Los propietarios de los grupos utilizados para el acceso deben considerarse identidades con privilegios para evitar la toma del control de la pertenencia a partir de un riesgo local.
La toma de control incluye la manipulación directa de la pertenencia a grupos local o la manipulación de atributos locales que pueden afectar a la pertenencia dinámica a grupos en Microsoft 365.

## <a name="manage-devices-from-the-cloud"></a>Administrar dispositivos de la nube


Use las funcionalidades de Azure AD para administrar dispositivos de forma segura.

-   **Usar estaciones de trabajo de Windows 10**: [Implemente dispositivos unidos a Azure AD](../devices/azureadjoin-plan.md) con directivas MDM. Habilite [Windows Autopilot](https://docs.microsoft.com/mem/autopilot/windows-autopilot) para una experiencia de aprovisionamiento totalmente automatizada.

    -   Deje de usar máquinas con Windows 8.1 y versiones anteriores.

    -   No implemente máquinas de sistemas operativos de servidor como estaciones de trabajo.

    -   Use [Microsoft Intune](https://www.microsoft.com/en/microsoft-365/enterprise-mobility-security/microsoft-intune) como el origen de la autoridad de todas las cargas de trabajo de administración de dispositivos.

-   [**Implemente dispositivos de acceso con privilegios**](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) para el acceso con privilegios para administrar Microsoft 365 y Azure AD.

 ## <a name="workloads-applications-and-resources"></a>Cargas de trabajo, aplicaciones y recursos 

-   **Sistemas SSO locales**: deje de usar cualquier infraestructura de administración de acceso web y federación local y configure las aplicaciones para que usen Azure AD.  

-   **Aplicaciones SaaS y de línea de negocio que admiten protocolos de autenticación modernos**: [use Azure AD para el inicio de sesión único](../manage-apps/what-is-single-sign-on.md). Cuanto más aplicaciones configure para usar Azure AD para la autenticación, menor es el riesgo en el caso de un ataque local.


* **Aplicaciones heredadas** 

   * La autenticación, la autorización y el acceso remoto a las aplicaciones heredadas que no admiten la autenticación moderna se pueden habilitar mediante [Azure AD Application Proxy](../manage-apps/application-proxy.md). También se pueden habilitar con una solución de controlador de entrega de aplicaciones o red mediante [integraciones de asociados de acceso híbrido seguro](../manage-apps/secure-hybrid-access.md).   

   * Elija un proveedor de VPN que admita la autenticación moderna e integre su autenticación con Azure AD. Si se trata de un riesgo que no es local, puede usar Azure AD para deshabilitar o bloquear el acceso deshabilitando la VPN.

*  **Servidores de cargas de trabajo y aplicaciones**

   * Las aplicaciones o los recursos que requerían servidores pueden migrarse a Azure IaaS y usar [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/overview) (Azure AD DS) para desacoplar la relación de confianza y la dependencia de AD local. Para lograr este desacoplamiento, las redes virtuales que se usan para Azure AD DS no deben tener conexión a las redes corporativas.

   * Siga las instrucciones de los [niveles de credenciales](https://aka.ms/TierModel). Los servidores de aplicaciones se suelen considerar recursos de nivel 1.

 ## <a name="conditional-access-policies"></a>Directivas de acceso condicional

Use el acceso condicional de Azure AD para interpretar las señales y tomar decisiones de autenticación basadas en ellas. Para más información, vea [Planeamiento de la implementación del acceso condicional](https://aka.ms/deploymentplans/ca).

* [Protocolos de autenticación heredados](../fundamentals/auth-sync-overview.md): use el acceso condicional para [bloquear los protocolos de autenticación heredados](../conditional-access/howto-conditional-access-policy-block-legacy.md) siempre que sea posible. Además, deshabilite los protocolos de autenticación heredados en el nivel de aplicación mediante la configuración específica de la aplicación.

   * Vea los detalles específicos de [Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online#how-basic-authentication-works-in-exchange-online) y [SharePoint Online](https://docs.microsoft.com/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps).

* Implemente las [configuraciones de acceso de dispositivos e identidades](https://docs.microsoft.com/microsoft-365/security/office-365-security/identity-access-policies?view=o365-worldwide) recomendadas.

* Si usa una versión de Azure AD que no incluye el acceso condicional, asegúrese de que usa los [valores predeterminados de seguridad de Azure AD](../fundamentals/concept-fundamentals-security-defaults.md).

   * Para más información sobre las licencias de características de Azure AD, vea la [guía de precios de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="monitoring"></a>Supervisión 

Una vez que haya configurado el entorno para proteger Microsoft 365 de un riesgo local, [supervise proactivamente](../reports-monitoring/overview-monitoring.md) el entorno.
### <a name="scenarios-to-monitor"></a>Escenarios que supervisar

Supervise los siguientes escenarios clave, además de los escenarios específicos de su organización. Por ejemplo, debe supervisar de forma proactiva el acceso a sus aplicaciones y recursos críticos para la empresa.

* **Actividad sospechosa**: Todos los [eventos de riesgo de Azure AD](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection#risk-detection-and-remediation) deben supervisarse en busca de actividad sospechosa. [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection) se integra de forma nativa con Azure Security Center.

   * Defina las [ubicaciones con nombre](../reports-monitoring/quickstart-configure-named-locations.md) de red para evitar detecciones ruidosas en las señales basadas en la ubicación. 
*  **Alertas de análisis de comportamiento de entidades de usuario (UEBA)** : use UEBA para obtener información sobre la detección de anomalías.
   * Microsoft Cloud App Discovery (MCAS) proporciona [UEBA en la nube](https://docs.microsoft.com/cloud-app-security/tutorial-ueba).

   * Puede [integrar UEBA local desde Azure ATP](https://docs.microsoft.com/defender-for-identity/install-step2). MCAS lee las señales de Azure AD Identity Protection. 

* **Actividad de las cuentas de acceso de emergencia**: se debe supervisar cualquier acceso que use [cuentas de acceso de emergencia](../roles/security-emergency-access.md) y se deben crear alertas para las investigaciones. Esta supervisión debe incluir lo siguiente: 

   * Inicios de sesión. 

   * Administración de credenciales. 

   * Todas las actualizaciones de pertenencias a grupos. 

   *    Asignaciones de aplicaciones. 
* **Actividad de roles con privilegios**: configure y revise las [alertas de seguridad generadas por Azure AD PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-configure-security-alerts?tabs=new#security-alerts).
    Supervise la asignación directa de roles con privilegios fuera de PIM mediante la generación de alertas cada vez que se asigne un usuario directamente.
* **Configuraciones de todo el inquilino en Azure AD**: cualquier cambio en las configuraciones de todo el inquilino debe generar alertas en el sistema. Estas incluyen, entre otras:
  *  Actualización de dominios personalizados  

  * Cambios en la lista de permitidos y bloqueados de Azure AD B2B
  * Proveedores de identidades permitidos de Azure AD B2B (IDP de SAML mediante federación directa o inicios de sesión sociales)  
  * Cambios en la directiva de riesgo o de acceso condicional 

* **Objetos de aplicación y de entidad de servicio**:
   * Nuevas aplicaciones o entidades de servicio que pueden requerir directivas de acceso condicional 

   * Credenciales adicionales agregadas a las entidades de servicio
   * Actividad de consentimiento de la aplicación 

* **Roles personalizados**:
   * Actualizaciones de las definiciones de roles personalizados 

   * Nuevos roles personalizados creados 

### <a name="log-management"></a>Administración de registros

Defina un almacenamiento de registros y una estrategia de retención, el diseño y la implementación para facilitar un conjunto de herramientas coherente como los sistemas SIEM, como por ejemplo, Azure Sentinel, consultas comunes e investigación y cuadernos de estrategias forenses.

* **Registros de Azure AD**. Ingiera registros y señales producidos a partir de procedimientos recomendados coherentes, como la configuración de diagnóstico, la retención de registros y la ingesta de SIEM. La estrategia de registro debe incluir los siguientes registros de Azure AD:
   * Actividad de inicio de sesión 

   * Registros de auditoría 

   * Eventos de riesgo 

Azure AD proporciona la [integración de Azure Monitor](../reports-monitoring/concept-activity-logs-azure-monitor.md) para los registros de auditoría y el registro de actividad de inicio de sesión. Los eventos de riesgo se pueden ingerir mediante [Microsoft Graph API](https://aka.ms/AzureADSecuredAzure/32b). Puede [transmitir registros de Azure AD a registros de Azure Monitor](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).

* **Registros de seguridad del sistema operativo de infraestructura híbrida**. Todos los registros del sistema operativo de infraestructura de identidad híbrida deben archivarse y supervisarse detenidamente como un <br>sistema de nivel 0, dadas las implicaciones del área expuesta. Esto incluye: 

   *  Azure AD Connect. [Azure AD Connect Health](https://aka.ms/AzureADSecuredAzure/32e) debe implementarse para supervisar la sincronización de identidades.

   *  Agentes de Application Proxy 


   * Agentes de escritura diferida de contraseñas 

   * Máquinas de puertas de enlace de protección de contraseñas  

   * NPS que tiene la extensión RADIUS de Azure MFA 

## <a name="next-steps"></a>Pasos siguientes
* [Creación de resistencia en la administración de identidad y acceso con Azure AD](resilience-overview.md)

* [Acceso externo seguro a los recursos](secure-external-access-resources.md) 
* [Integración de todas sus aplicaciones en Azure AD](five-steps-to-full-application-integration-with-azure-ad.md)
