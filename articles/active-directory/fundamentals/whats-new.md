---
title: Novedades Notas de la versión de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de las novedades de Azure Active Directory, como son las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: active-directory
author: ajburnle
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d40e460c150fa616144180d4f677fe0211143e5
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2021
ms.locfileid: "97862421"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue la dirección URL `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` en el ![icono del lector de fuentes icono RSS](./media/whats-new/feed-icon-16x16.png) lector de fuentes.

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día de los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia. Si busca elementos que tengan más de seis meses, puede encontrarlos en el [Archivo de novedades de Azure Active Directory](whats-new-archive.md).

---
## <a name="december-2020"></a>Diciembre de 2020

### <a name="public-preview---azure-ad-b2c-phone-sign-up-and-sign-in-using-built-in-policy"></a>Versión preliminar: registro e inicio de sesión mediante teléfono en Azure AD B2C con una directiva integrada

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
El registro e inicio de sesión mediante teléfono en B2C permiten a los administradores de TI y desarrolladores de organizaciones dejar que sus usuarios finales inicien sesión y se registren con un número de teléfono en los flujos de usuario. Lea [Configuración del registro e inicio de sesión telefónico para flujos de usuario (versión preliminar)](../../active-directory-b2c/phone-authentication-user-flows.md) para obtener más información.

---

### <a name="general-availability---security-defaults-now-enabled-for-all-new-tenants-by-default"></a>Disponibilidad general: valores predeterminados de seguridad ahora habilitados para todos los inquilinos nuevos de forma predeterminada

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Para proteger las cuentas de usuario, todos los inquilinos nuevos creados el 12 de noviembre de 2020 o después vendrán con Valores predeterminados de seguridad habilitado. Valores predeterminados de seguridad aplica varias directivas, entre las que se incluyen:
- Requiere que todos los usuarios y administradores se registren para MFA mediante la aplicación de Microsoft Authenticator
- Requiere que los roles de administrador críticos usen MFA cada vez que inicien sesión. A todos los demás usuarios se les pedirá MFA cuando sea necesario. 
- La autenticación heredada se bloqueará en todo el inquilino. 

Para obtener más información, consulte [¿Cuáles son los valores de seguridad predeterminados?](../fundamentals/concept-fundamentals-security-defaults.md)

---

### <a name="general-availability---support-for-groups-with-up-to-250k-members-in-aadconnect"></a>Disponibilidad general: compatibilidad con grupos de hasta 250 000 miembros en AADConnect

**Tipo:** Característica modificada  
**Categoría del servicio:** AD Connect  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
Microsoft ha implementado un nuevo punto de conexión (API) para Azure AD Connect que mejora el rendimiento de las operaciones del servicio de sincronización para Azure Active Directory. Al usar el nuevo [punto de conexión V2](../hybrid/how-to-connect-sync-endpoint-api-v2.md), experimentará mejoras de rendimiento notables en la exportación y la importación respecto a Azure AD. Este nuevo punto de conexión admite los siguientes escenarios:

- Grupos de sincronización de hasta 250 000 miembros
- Mejoras en el rendimiento de la exportación y la importación a Azure AD

---

### <a name="general-availability---entitlement-management-available-for-tenants-in-azure-china-cloud"></a>Disponibilidad general: administración de derechos disponible para los inquilinos en la nube de Azure China

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 

Las funcionalidades de administración de derechos están ahora disponibles para todos los inquilinos en la nube de Azure China. Para obtener información, visite nuestro sitio de [documentación de Identity Governance](https://docs.azure.cn/zh-cn/active-directory/governance/).

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---december-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD: diciembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Bizagi Studio para la automatización de procesos digitales](../saas-apps/bizagi-studio-for-digital-process-automation-provisioning-tutorial.md)
- [CybSafe](../saas-apps/cybsafe-provisioning-tutorial.md)
- [GroupTalk](../saas-apps/grouptalk-provisioning-tutorial.md)
- [PaperCut Cloud Print Management](/azure/active-directory/saas-apps/papercut-cloud-print-management-provisioning-tutorial)
- [Parsable](../saas-apps/parsable-provisioning-tutorial.md)
- [Shopify Plus](../saas-apps/shopify-plus-provisioning-tutorial.md)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---
 
[1233182](https://identitydivision.visualstudio.com/IAM/IXR/_queries?id=1233182&triage=true&fullScreen=false&_a=edit)

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---december-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: diciembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En diciembre de 2020 se han agregado las siguientes 18 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[AwareGo](../saas-apps/awarego-tutorial.md), [HowNow SSO](https://gethownow.com/), [ZyLAB ONE Legal Hold](https://www.zylab.com/en/product/legal-hold), [Guider](http://www.guider-ai.com/), [Softcrisis](https://www.softcrisis.se/sv/), [Pims 365](http://www.omega365.com/pims), [InformaCast](../saas-apps/informacast-tutorial.md), [RetrieverMediaDatabase](../saas-apps/retrievermediadatabase-tutorial.md), [vonage](../saas-apps/vonage-tutorial.md), [Count Me In - Operations Dashboard](../saas-apps/count-me-in-operations-dashboard-tutorial.md), [ProProfs Knowledge Base](../saas-apps/proprofs-knowledge-base-tutorial.md), [RightCrowd Workforce Management](../saas-apps/rightcrowd-workforce-management-tutorial.md), [JLL TRIRIGA](../saas-apps/jll-tririga-tutorial.md), [Shutterstock](../saas-apps/shutterstock-tutorial.md), [FortiWeb Web Application Firewall](../saas-apps/linkedin-talent-solutions-tutorial.md), [LinkedIn Talent Solutions](../saas-apps/linkedin-talent-solutions-tutorial.md), [Equinix Federation App](../saas-apps/equinix-federation-app-tutorial.md) y [KFAdvance](../saas-apps/kfadvance-tutorial.md)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí: https://aka.ms/AzureADAppRequest.

--- 

## <a name="november-2020"></a>Noviembre de 2020

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation"></a>Entrada en desuso de TLS 1.0, TLS 1.1 y 3DES en Azure Active Directory

**Tipo:** Plan de cambio  
**Categoría del servicio:** todas las aplicaciones de Azure AD  
**Funcionalidad del producto:** Estándares

Azure Active Directory dejará de usar los siguientes protocolos en las regiones de todo el mundo de Azure Active Directory el 30 de junio de 2021:

- TLS 1.0
- TLS 1.1
- Conjunto de cifrado 3DES (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Los entornos afectados son:
- Nube comercial de Azure
- Office 365 GCC y WW

Anuncio relacionado: todas las combinaciones de cliente-servidor y explorador-servidor deben usar los conjuntos de cifrado modernos y TLS 1.2 para mantener una conexión segura con Azure Active Directory para los servicios de Azure, Office 365 y Microsoft 365. Este cambio está relacionado con la [entrada en desuso de TLS 1.0, TLS 1.1 y el conjunto de cifrado 3DES de Azure Active Directory en la nube US Gov](whats-new.md#azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---november-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, noviembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En noviembre de 2020 se han agregado las siguientes 52 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[Travel & Expense Management](https://app.expenseonce.com/Account/Login), [Tribeloo](../saas-apps/tribeloo-tutorial.md), [Itslearning File Picker](https://pmteam.itslearning.com/), [Crises Control](../saas-apps/crises-control-tutorial.md), [CourtAlert](https://www.courtalert.com/), [StealthMail](https://stealthmail.com/), [Edmentum - Study Island](https://app.studyisland.com/cfw/login/), [Virtual Risk Manager](../saas-apps/virtual-risk-manager-tutorial.md), [TIMU](../saas-apps/timu-tutorial.md), [Looker Analytics Platform](../saas-apps/looker-analytics-platform-tutorial.md), [Talview - Recruit](https://recruit.talview.com/login), Real Time Translator, [Klaxoon](https://access.klaxoon.com/login), [Podbean](../saas-apps/podbean-tutorial.md), [zcal](https://zcal.co/signup), [expensemanager](https://api.expense-manager.com/), [Netsparker Enterprise](../saas-apps/netsparker-enterprise-tutorial.md), [En-trak Tenant Experience Platform](https://portal.en-trak.app/), [Appian](../saas-apps/appian-tutorial.md), [Panorays](../saas-apps/panorays-tutorial.md), [Builterra](https://portal.builterra.com/), [EVA Check-in](https://my.evacheckin.com/organization), [HowNow WebApp SSO](../saas-apps/hownow-webapp-sso-tutorial.md), [Coupa Risk Assess](../saas-apps/coupa-risk-assess-tutorial.md), [Lucid (All Products)](../saas-apps/lucid-tutorial.md), [GoBright](https://portal.brightbooking.eu/), [SailPoint IdentityNow](../saas-apps/sailpoint-identitynow-tutorial.md),[Resource Central](../saas-apps/resource-central-tutorial.md), [UiPathStudioO365App](https://www.uipath.com/product/platform), [Jedox](../saas-apps/jedox-tutorial.md), [Cequence Application Security](../saas-apps/cequence-application-security-tutorial.md), [PerimeterX](../saas-apps/perimeterx-tutorial.md), [TrendMiner](../saas-apps/trendminer-tutorial.md), [Lexion](../saas-apps/lexion-tutorial.md), [WorkWare](../saas-apps/workware-tutorial.md), [ProdPad](../saas-apps/prodpad-tutorial.md), [AWS ClientVPN](../saas-apps/aws-clientvpn-tutorial.md), [AppSec Flow SSO](../saas-apps/appsec-flow-sso-tutorial.md), [Luum](../saas-apps/luum-tutorial.md), [Freight Measure](https://www.gpcsl.com/freight.html), [Terraform Cloud](../saas-apps/terraform-cloud-tutorial.md), [Nature Research](../saas-apps/nature-research-tutorial.md), [Play Digital Signage](https://login.playsignage.com/login), [RemotePC](../saas-apps/remotepc-tutorial.md), [Prolorus](../saas-apps/prolorus-tutorial.md), [Hirebridge ATS](../saas-apps/hirebridge-ats-tutorial.md), [Teamgage](https://www.teamgage.com/Account/ExternalLoginAzure), [Roadmunk](../saas-apps/roadmunk-tutorial.md), [Sunrise Software Relations CRM](https://cloud.relations-crm.com/), [Procaire](../saas-apps/procaire-tutorial.md), [Mentor® by eDriving: Business](https://www.edriving.com/), [Gradle Enterprise](https://gradle.com/)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí https://aka.ms/AzureADAppRequest.

---

### <a name="public-preview---custom-roles-for-enterprise-apps"></a>Versión preliminar pública: roles personalizados para aplicaciones empresariales

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
 [Los roles de RBAC personalizados para la administración delegada de aplicaciones empresariales](../users-groups-roles/roles-custom-available-permissions.md) ahora están en versión preliminar pública. Estos nuevos permisos se basan en los roles personalizados para la administración del registro de aplicaciones, lo que permite un control exhaustivo sobre el acceso que tienen los administradores. Con el tiempo, se publicarán permisos adicionales para la administración delegada de Azure AD.

Estos son algunos escenarios de delegación comunes:
- asignación de usuarios y grupos que pueden acceder a aplicaciones de inicio de sesión único basadas en SAML
- creación de aplicaciones de la galería de Azure AD
- actualización y lectura de configuraciones básicas de SAML para aplicaciones de inicio de sesión único basadas en SAML
- administración de certificados de firma para aplicaciones de inicio de sesión único basadas en SAML
- actualización de las direcciones de correo electrónico de las notificaciones de expiración de los certificados de inicio de sesión para aplicaciones de inicio de sesión único basadas en SAML
- actualización de la firma del token de SAML y del algoritmo de inicio de sesión para aplicaciones de inicio de sesión único basadas en SAML
- creación, eliminación y actualización de atributos y notificaciones de usuario de las aplicaciones de inicio de sesión único basadas en SAML
- capacidad de activar, desactivar y reiniciar los trabajos de aprovisionamiento
- actualizaciones en la asignación de atributos
- capacidad de leer la configuración de aprovisionamiento asociada al objeto
- capacidad de leer la configuración de aprovisionamiento asociada a la entidad de servicio
- capacidad de autorización del acceso a la aplicación para el aprovisionamiento

---

### <a name="azure-ad-application-proxy-natively-supports-single-sign-on-access-to-applications-that-use-headers-for-authentication"></a>Application Proxy de Azure AD admite de forma nativa el acceso de inicio sesión único a las aplicaciones que usan encabezados para la autenticación

**Tipo:** Nueva característica  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso
 
Application Proxy de Azure Active Directory (Azure AD) admite de forma nativa el acceso de inicio sesión único a las aplicaciones que usan encabezados para la autenticación. Puede configurar los valores de encabezado que necesita la aplicación en Azure AD. Los valores de encabezado se envían a la aplicación a través de Application Proxy. Para más información, consulte [Inicio de sesión único basado en encabezados para aplicaciones locales con App Proxy de Azure AD (versión preliminar)](../manage-apps/application-proxy-configure-single-sign-on-with-headers.md).
 
---

### <a name="general-availability---azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy"></a>Disponibilidad general: registro e inicio de sesión mediante teléfono en Azure AD B2C con una directiva personalizada

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Con el registro y el inicio de sesión mediante el número de teléfono, los desarrolladores y las empresas pueden permitir a sus clientes registrarse e iniciar sesión con una contraseña de un solo uso enviada al número de teléfono del usuario a través de un SMS. Esta característica también permite al cliente cambiar su número de teléfono si pierde el acceso a su dispositivo. Con la eficacia de las directivas personalizadas, permite a los desarrolladores y a las empresas comunicar su marca mediante la personalización de la página. Aprenda cómo [configurar el registro y el inicio de sesión en el teléfono con directivas personalizadas en Azure AD B2C](../../active-directory-b2c/phone-authentication.md).
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---november-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD: noviembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Adobe Identity Management](../saas-apps/adobe-identity-management-provisioning-tutorial.md)
- [Blogin](../saas-apps/blogin-provisioning-tutorial.md)
- [Clarizen One](../saas-apps/clarizen-one-provisioning-tutorial.md)
- [Contentful](../saas-apps/contentful-provisioning-tutorial.md)
- [GitHub AE](../saas-apps/github-ae-provisioning-tutorial.md)
- [Playvox](../saas-apps/playvox-provisioning-tutorial.md)
- [PrinterLogic SaaS](../saas-apps/printer-logic-saas-provisioning-tutorial.md)
- [Tic - Tac Mobile](../saas-apps/tic-tac-mobile-provisioning-tutorial.md)
- [Visibly](../saas-apps/visibly-provisioning-tutorial.md)

Para más información, consulte [Qué es el aprovisionamiento automatizado de usuarios de aplicaciones SaaS en Azure AD](../manage-apps/user-provisioning.md).
 
---

### <a name="public-preview---email-sign-in-with-proxyaddresses-now-deployable-via-staged-rollout"></a>Versión preliminar pública: el inicio de sesión mediante correo electrónico con direcciones proxy ahora se puede implementar mediante la implementación por fases

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios
 
Los administradores de inquilinos ahora pueden usar la implementación por fases para implementar el inicio de sesión mediante correo electrónico con direcciones proxy en grupos de Azure AD específicos. Esto puede ayudar a probar la característica antes de implementarla en todo el inquilino mediante la directiva de detección del dominio de inicio. Las instrucciones para implementar el inicio de sesión mediante correo electrónico con direcciones proxy con la implementación por fases se encuentran en la [documentación](../authentication/howto-authentication-use-email-signin.md).
 
---

### <a name="limited-preview---sign-in-diagnostic"></a>Versión preliminar limitada: diagnóstico de inicio de sesión

**Tipo:** Nueva característica  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
Con la versión preliminar inicial del diagnóstico de inicio de sesión, los administradores ahora pueden revisar los inicios de sesión de los usuarios. Los administradores pueden recibir detalles de importancia contextuales y específicos, e instrucciones sobre lo que ha sucedido durante un inicio de sesión y cómo solucionar los problemas. El diagnóstico está disponible tanto en el nivel de Azure AD como en las hojas de diagnóstico y resolución del acceso condicional. Los escenarios de diagnóstico admitidos en esta versión son el acceso condicional, Multi-Factor Authentication y el inicio de sesión correcto.

Para obtener más información, consulte [¿Qué es el diagnóstico de inicio de sesión en Azure AD?](../reports-monitoring/overview-sign-in-diagnostics.md)
 
---

### <a name="improved-unfamiliar-sign-in-properties"></a>Mejora de las propiedades de inicio de sesión desconocidas

**Tipo:** Característica modificada  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

  Se han actualizado las detecciones de propiedades de inicio de sesión desconocidas. Es posible que los clientes perciban más detecciones de propiedades de inicio de sesión desconocidas de alto riesgo. Para más información, consulte [¿Qué es el riesgo?](../identity-protection/concept-identity-protection-risks.md)
 
---

### <a name="public-preview-refresh-of-cloud-provisioning-agent-now-available-version-112810"></a>Ya está disponible la actualización de la versión preliminar pública del agente de aprovisionamiento en la nube (versión: 1.1.281.0)

**Tipo:** Característica modificada  
**Categoría del servicio:** Aprovisionamiento en la nube de Azure AD  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
El agente de aprovisionamiento en la nube se ha publicado en versión preliminar pública y ahora está disponible mediante el portal. Esta versión contiene varias mejoras, entre las que se incluyen la compatibilidad con GMSA para los dominios, lo que proporciona una mejor seguridad, ciclos de sincronización inicial mejorados y compatibilidad con grupos grandes. Consulte el [historial](../app-provisioning/provisioning-agent-release-version-history.md) de lanzamiento de versiones para más información. 
 
---

### <a name="bitlocker-recovery-key-api-endpoint-now-under-informationprotection"></a>El punto de conexión de la API de la clave de recuperación de BitLocker ahora está en /informationProtection

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso de dispositivos  
**Funcionalidad del producto:** Administración del ciclo de vida de dispositivos
 
Anteriormente, podía recuperar las claves de BitLocker mediante el punto de conexión /bitlocker. Finalmente vamos a dejar de usar este punto de conexión y los clientes deberían empezar a consumir la API que ahora está en /informationProtection. 

Consulte [Tipo de recurso bitlockerRecoveryKey](https://docs.microsoft.com/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) para conocer las actualizaciones de la documentación para reflejar estos cambios.

---

### <a name="general-availability-of-application-proxy-support-for-remote-desktop-services-html5-web-client"></a>Disponibilidad general de la compatibilidad de Application Proxy con el cliente web HTML5 de los Servicios de Escritorio remoto

**Tipo:** Característica modificada  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso
 
La compatibilidad de Application Proxy de Azure AD con el cliente web de los Servicios de Escritorio remoto (RDS) ahora está disponible con carácter general. El cliente web de RDS permite a los usuarios acceder a la infraestructura de Escritorio remoto mediante cualquier explorador compatible con HTLM5, como Microsoft Edge, Internet Explorer 11, Google Chrome, etc. Los usuarios pueden interactuar con escritorios o aplicaciones remotos como lo harían con un dispositivo local desde cualquier lugar. 

Mediante el uso de Application Proxy de Azure AD, puede aumentar la seguridad de la implementación de RDS mediante la exigencia de directivas de acceso condicional y autenticación previa para todos los tipos de aplicaciones cliente enriquecidas. Para más información, consulte [Publicación del Escritorio Remoto con Application Proxy de Azure AD](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md).
 
---

### <a name="new-enhanced-dynamic-group-service-is-in-public-preview"></a>El nuevo servicio de grupo dinámico mejorado está en versión preliminar pública

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración
 
El servicio de grupo dinámico mejorado está ahora en versión preliminar pública. Los nuevos clientes que creen grupos dinámicos en sus inquilinos utilizarán el nuevo servicio. El tiempo necesario para crear un grupo dinámico será proporcional al tamaño del grupo que se va a crear en lugar de al tamaño del inquilino. Esta actualización mejorará significativamente el rendimiento en inquilinos grandes cuando los clientes crean grupos más pequeños. 

El nuevo servicio también pretende completar la adición y eliminación de miembros debidas a cambios en los atributos en pocos minutos. Además, los errores de procesamiento únicos no bloquearán el procesamiento del inquilino. Para más información sobre la creación de grupos dinámicos, consulte nuestra [documentación](../enterprise-users/groups-create-rule.md).
 
---
## <a name="october-2020"></a>Octubre de 2020

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-tls-certificate-changes"></a>Agentes híbridos de Azure AD local afectados por los cambios de certificado TLS de Azure

**Tipo:** Plan de cambio  
**Categoría del servicio:** N/D  
**Funcionalidad del producto:** plataforma

Microsoft está actualizando los servicios de Azure para que usen los certificados TLS de un conjunto diferente de entidades de certificación (CA) raíz. El motivo de este cambio es que los certificados de CA actuales no cumplen uno de los requisitos de la base de referencia del foro CA/Browser. Este cambio afectará a los agentes híbridos de Azure AD instalados en el entorno local que tengan entornos protegidos con una lista fija de certificados raíz y se tendrán que actualizar para confiar en los nuevos emisores de certificados.

Este cambio producirá una interrupción del servicio si no realiza ninguna acción inmediatamente. Estos agentes incluyen [conectores de Application Proxy](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) para el acceso remoto al entorno local, agentes de [autenticación de paso a través](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) que permiten a los usuarios iniciar sesión en aplicaciones con las mismas contraseñas y agentes de la [versión preliminar del aprovisionamiento en la nube](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) que ejecutan la sincronización de AD con Azure AD. 

Si tiene un entorno con reglas de firewall establecidas para permitir llamadas salientes únicamente a la descarga de una lista de revocación de certificados (CRL) específica, deberá permitir las siguientes direcciones URL de CRL y OCSP. Para obtener detalles completos sobre el cambio y las direcciones URL de CRL y OCSP a las que se permitirá el acceso, consulte [Cambios en los certificados TLS de Azure](../../security/fundamentals/tls-certificate-changes.md).

---

### <a name="provisioning-events-will-be-removed-from-audit-logs-and-published-solely-to-provisioning-logs"></a>Los eventos de aprovisionamiento se quitarán de los registros de auditoría y se publicarán únicamente en los registros de aprovisionamiento

**Tipo:** Plan de cambio  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
La actividad del [servicio de aprovisionamiento](../app-provisioning/user-provisioning.md) de SCIM se registra en los registros de auditoría y en los registros de aprovisionamiento. Esto incluye actividades como la creación de un usuario en ServiceNow o un grupo en GSuite, o la importación de un rol de AWS. En el futuro, estos eventos solo se publicarán en los registros de aprovisionamiento. Este cambio se está implementando para evitar eventos duplicados entre los registros, así como los costos adicionales en los que incurren los clientes que consumen los registros de Log Analytics. 

Proporcionaremos una actualización cuando se complete una fecha. Este desuso no está previsto para el año natural 2020. 

> [!NOTE]
> Esto no afecta a los eventos de los registros de auditoría fuera de los eventos de sincronización emitidos por el servicio de aprovisionamiento. Los eventos como la creación de una aplicación, la directiva de acceso condicional, un usuario del directorio, etc. se seguirán emitiendo en los registros de auditoría. [Más información](../reports-monitoring/concept-provisioning-logs.md?context=azure%2factive-directory%2fapp-provisioning%2fcontext%2fapp-provisioning-context).
 

---

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-transport-layer-security-tls-certificate-changes"></a>Agentes híbridos de Azure AD local afectados por los cambios de certificado de Seguridad de la capa de transporte (TLS) de Azure

**Tipo:** Plan de cambio  
**Categoría del servicio:** N/D  
**Funcionalidad del producto:** plataforma
 
Microsoft está actualizando los servicios de Azure para que usen los certificados TLS de un conjunto diferente de entidades de certificación (CA) raíz. Se realizará una actualización porque los certificados de CA actuales no cumplen uno de los requisitos de la base de referencia del foro CA/Browser. Este cambio afectará a los agentes híbridos de Azure AD instalados en el entorno local que tengan entornos protegidos con una lista fija de certificados raíz. Estos agentes deberán actualizarse para confiar en los nuevos emisores de certificados.

Este cambio producirá una interrupción del servicio si no realiza ninguna acción inmediatamente. Estos agentes incluyen: 
- [Conectores de Application Proxy](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) para el acceso remoto al entorno local. 
- Agentes de [autenticación de paso a través](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) que permiten a los usuarios iniciar sesión en aplicaciones con las mismas contraseñas.
- Agentes de la [versión preliminar del aprovisionamiento en la nube](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) que realizan la sincronización de AD con Azure AD. 

Si tiene un entorno con reglas de firewall establecidas para permitir llamadas salientes únicamente a la descarga de una lista de revocación de certificados (CRL) específica, deberá permitir las direcciones URL de CRL y OCSP. Para obtener detalles completos sobre el cambio y las direcciones URL de CRL y OCSP a las que se permitirá el acceso, consulte [Cambios en los certificados TLS de Azure](../../security/fundamentals/tls-certificate-changes.md).
 
---

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud"></a>Desuso de Azure Active Directory TLS 1.0, TLS 1.1 y 3DES en la nube de US Gov

**Tipo:** Plan de cambio  
**Categoría del servicio:** todas las aplicaciones de Azure AD  
**Funcionalidad del producto:** Estándares
 
Azure Active Directory dejará de usar los siguientes protocolos antes del 31 de marzo de 2021:
- TLS 1.0
- TLS 1.1
- Conjunto de cifrado 3DES (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Todas las combinaciones de cliente-servidor y explorador-servidor deben usar los conjuntos de cifrado modernos y TLS 1.2 para mantener una conexión segura con Azure Active Directory para los servicios de Azure, Office 365 y Microsoft 365.

Los entornos afectados son:
- Azure US Gov
- [Office 365 GCC High y DoD](/microsoft-365/compliance/tls-1-2-in-office-365-gcc)
 
---

### <a name="assign-applications-to-roles-on-au-and-object-scope"></a>Asignación de aplicaciones a roles en el ámbito del objeto y la unidad administrativa

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
Esta característica permite asignar una aplicación (SPN) a un rol de administrador en el ámbito de la unidad administrativa. Para obtener más información, consulte [Asignación de roles con ámbito a una unidad administrativa](../roles/admin-units-assign-roles.md).

---

### <a name="now-you-can-disable-and-delete-guest-users-when-theyre-denied-access-to-a-resource"></a>Ahora puede deshabilitar y eliminar usuarios invitados cuando se les deniega el acceso a un recurso.

**Tipo:** Nueva característica  
**Categoría del servicio:** Revisiones de acceso  
**Funcionalidad del producto:** Identity Governance
 
Deshabilitar y eliminar es un control avanzado de las revisiones de acceso de Azure AD para ayudar a las organizaciones a administrar mejor los invitados externos en grupos y aplicaciones. Si los invitados se deniegan en una revisión de acceso, **deshabilitar y eliminar** los bloqueará automáticamente para que no inicien sesión durante 30 días. Después de 30 días, se quitarán del inquilino por completo.

Para obtener más información sobre esta característica, consulte [Deshabilitación y eliminación de identidades externas con las revisiones de acceso de Azure AD (versión preliminar)](../governance/access-reviews-external-users.md#disable-and-delete-external-identities-with-azure-ad-access-reviews-preview).
 
---

### <a name="access-review-creators-can-add-custom-messages-in-emails-to-reviewers"></a>Los creadores de la revisión de acceso pueden agregar mensajes personalizados en los mensajes de correo electrónico para los revisores

**Tipo:** Nueva característica  
**Categoría del servicio:** Revisiones de acceso  
**Funcionalidad del producto:** Identity Governance
 
En las revisiones de acceso de Azure AD, los administradores que crean revisiones ahora pueden escribir un mensaje personalizado a los revisores. Los revisores verán el mensaje en el correo electrónico que reciban, que les pedirá que completen la revisión. Para más información sobre el uso de esta característica, consulte el paso 14 de la sección [Creación de una o varias revisiones de acceso](../governance/create-access-review.md#create-one-or-more-access-reviews).

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---october-2020"></a>Nuevos conectores de aprovisionamiento disponibles en la galería de aplicaciones de Azure AD, octubre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Apple Business Manager](../saas-apps/apple-business-manager-provision-tutorial.md)
- [Apple School Manager](../saas-apps/apple-school-manager-provision-tutorial.md)
- [Code42](../saas-apps/code42-provisioning-tutorial.md)
- [AlertMedia](../saas-apps/alertmedia-provisioning-tutorial.md)
- [OpenText Directory Services](../saas-apps/open-text-directory-services-provisioning-tutorial.md)
- [Cinode](../saas-apps/cinode-provisioning-tutorial.md)
- [Global Relay Identity Sync](../saas-apps/global-relay-identity-sync-provisioning-tutorial.md)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="integration-assistant-for-azure-ad-b2c"></a>Asistente de integración para Azure AD B2C

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
La experiencia del asistente de integración (versión preliminar) ahora está disponible para los registros de aplicaciones de Azure AD B2C. Esta experiencia le guiará en la configuración de la aplicación para escenarios comunes. Para obtener más información, consulte [Procedimientos recomendados y recomendaciones de la plataforma de identidad de Microsoft](../develop/identity-platform-integration-checklist.md).
 
---

### <a name="view-role-template-id-in-azure-portal-ui"></a>Visualización del id. de plantilla de rol en la interfaz de usuario de Azure Portal

**Tipo:** Nueva característica  
**Categoría del servicio:** Roles de Azure  
**Funcionalidad del producto:** Control de acceso
 

Ahora puede ver el id. de plantilla de cada rol de Azure AD en Azure Portal. En Azure AD, seleccione la **descripción** del rol seleccionado. 

Se recomienda que los clientes usen los identificadores de plantilla de rol en el script y el código de PowerShell, en lugar del nombre para mostrar. El id. de plantilla de rol se puede usar para los objetos [directoryRoles](/graph/api/resources/directoryrole) y [roleDefinition](/graph/api/resources/unifiedroledefinition?view=graph-rest-beta). Para obtener más información sobre los identificadores de plantilla de rol, consulte [Identificadores de plantilla de rol](../roles/permissions-reference.md#role-template-ids).

---

### <a name="api-connectors-for-azure-ad-b2c-sign-up-user-flows-is-now-in-public-preview"></a>Los conectores de API para los flujos de usuario de registro de Azure AD B2C ahora están en versión preliminar pública

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 

Los conectores de API ahora están disponibles para su uso con Azure Active Directory B2C. Los conectores de API permiten usar las API web para personalizar los flujos de usuario de registro e integrarlos con los sistemas de nube externos. Puede usar los conectores de API para:

- Realizar la integración con flujos de trabajo de aprobación personalizados
- Validar los datos de entrada del usuario
- Sobrescribir atributos de usuario 
- Ejecutar una lógica de negocios personalizada 

 Visite la documentación [Uso de conectores de API para personalizar y extender el registro](../../active-directory-b2c/api-connectors-overview.md) para obtener más información.

---

### <a name="state-property-for-connected-organizations-in-entitlement-management"></a>Propiedad de estado para organizaciones conectadas en administración de derechos

**Tipo:** Nueva característica  
**Categoría del servicio:** **Funcionalidad del producto:** Administración de directorios Administración de derechos
 

 Todas las organizaciones conectadas tendrán ahora una propiedad adicional denominada "State". El estado controlará el uso de la organización conectada en las directivas que hacen referencia a "todas las organizaciones conectadas configuradas". El valor será "configured" (lo que significa que la organización se encuentra en el ámbito de las directivas que usan la cláusula "all") o "proposed" (lo que significa que la organización no está en el ámbito).  

Las organizaciones conectadas creadas manualmente tendrán un valor predeterminado de "configured". Mientras tanto, las creadas automáticamente (a través de directivas que permiten a cualquier usuario de Internet solicitar acceso) tendrán como valor predeterminado "proposed".  Las organizaciones conectadas creadas antes del 9 de septiembre de 2020 se establecerán en "configured". Los administradores pueden actualizar esta propiedad según sea necesario. [Más información](../governance/entitlement-management-organization.md#managing-a-connected-organization-programmatically).
 

---

### <a name="azure-active-directory-external-identities-now-has-premium-advanced-security-settings-for-b2c"></a>Las identidades externas de Azure Active Directory ahora tienen una configuración de seguridad avanzada prémium para B2C

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
Las características de detección de riesgo y acceso condicional basado en riesgos de Identity Protection ahora están disponibles en [Azure AD B2C](../..//active-directory-b2c/conditional-access-identity-protection-overview.md). Con estas características de seguridad avanzadas, los clientes ahora pueden:
- Aprovechar Intelligent Insights para evaluar el riesgo con las aplicaciones B2C y las cuentas de usuario final. Las detecciones incluyen viajes atípicos, direcciones IP anónimas, direcciones IP vinculadas a malware e Inteligencia sobre amenazas de Azure AD. También están disponibles los informes basados en el portal y la API.
- Solucione los riesgos automáticamente mediante la configuración de directivas de autenticación adaptables para los usuarios de B2C. Los desarrolladores y administradores de aplicaciones pueden mitigar el riesgo en tiempo real. Para ello, deben exigir la autenticación multifactor (MFA) o bloquear el acceso en función del nivel de riesgo del usuario detectado, con controles adicionales disponibles en función de la ubicación, el grupo y la aplicación.
- Realice la integración con los flujos de usuario de Azure AD B2C y las directivas personalizadas. Las condiciones se pueden desencadenar desde los flujos de usuario integrados en Azure AD B2C o se pueden incorporar a las directivas personalizadas de B2C. Como sucede con otros aspectos del flujo de usuario de B2C, la mensajería de experiencia del usuario final se puede personalizar. La personalización depende de las alternativas de voz, marca y mitigación de la organización.
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---october-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, octubre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En octubre de 2020 se han agregado las siguientes 27 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[Sentry](../saas-apps/sentry-tutorial.md), [Bumblebee - Productivity Superapp](https://app.yellowmessenger.com/user/login), [ABBYY FlexiCapture Cloud](../saas-apps/abbyy-flexicapture-cloud-tutorial.md), [EAComposer](../saas-apps/eacomposer-tutorial.md), [Genesys Cloud Integration for Azure](https://apps.mypurecloud.com/msteams-integration/), [Zone Technologies Portal](https://portail.zonetechnologie.com/signin), [Beautiful.ai](../saas-apps/beautiful.ai-tutorial.md), [Datawiza Access Broker](https://console.datawiza.com/), [ZOKRI](https://app.zokri.com/), [CheckProof](../saas-apps/checkproof-tutorial.md), [Ecochallenge.org](https://events.ecochallenge.org/users/login), [atSpoke](http://atspoke.com/login), [Appointment Reminder](https://app.appointmentreminder.co.nz/account/login), [Cloud.Market](https://cloud.market/), [TravelPerk](../saas-apps/travelperk-tutorial.md), [Greetly](https://app.greetly.com/), [OrgVitality SSO](../saas-apps/orgvitality-sso-tutorial.md), [Web Cargo Air](../saas-apps/web-cargo-air-tutorial.md), [Loop Flow CRM](../saas-apps/loop-flow-crm-tutorial.md), [Starmind](../saas-apps/starmind-tutorial.md), [Workstem](https://hrm.workstem.com/login), [Retail Zipline](../saas-apps/retail-zipline-tutorial.md), [Hoxhunt](../saas-apps/hoxhunt-tutorial.md), [MEVISIO](../saas-apps/mevisio-tutorial.md), [Samsara](../saas-apps/samsara-tutorial.md), [Nimbus](../saas-apps/nimbus-tutorial.md) y [Pulse Secure virtual Traffic Manager](../saas-apps/pulse-secure-virtual-traffic-manager-tutorial.md)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí https://aka.ms/AzureADAppRequest.

---

### <a name="provisioning-logs-can-now-be-streamed-to-log-analytics"></a>Los registros de aprovisionamiento ahora se pueden transmitir a Log Analytics

**Tipo:** Nueva característica  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 

Publique los registros de aprovisionamiento en Log Analytics con el fin de:
- Almacenar registros de aprovisionamiento durante más de 30 días.
- Definir alertas y notificaciones personalizadas.
- Crear paneles para visualizar los registros.
- Ejecutar consultas complejas para analizar los registros. 

Para obtener información sobre el uso de la característica, consulte [Entienda cómo se integra el aprovisionamiento con los registros de Azure Monitor](../app-provisioning/application-provisioning-log-analytics.md).
 
---

### <a name="provisioning-logs-can-now-be-viewed-by-application-owners"></a>Los propietarios de la aplicación ahora pueden ver los registros de aprovisionamiento.

**Tipo:** Característica modificada  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
Ahora puede permitir que los propietarios de aplicaciones supervisen la actividad del servicio de aprovisionamiento y solucionen problemas sin proporcionarles un rol con privilegios ni convertir TI en un cuello de botella. [Más información](../reports-monitoring/concept-provisioning-logs.md).
 
---

### <a name="renaming-10-azure-active-directory-roles"></a>Cambio de nombre de 10 roles de Azure Active Directory

**Tipo:** Característica modificada  
**Categoría del servicio:** Roles de Azure  
**Funcionalidad del producto:** Control de acceso
 
Algunos roles integrados de Azure Active Directory (AD) tienen nombres distintos de los que aparecen en el Centro de administración de Microsoft 365, el portal de Azure AD y Microsoft Graph. Esta incoherencia puede provocar problemas en los procesos automatizados. Con esta actualización, vamos a cambiar el nombre de 10 roles para que sean coherentes. En la tabla siguiente se indican los nombres de roles nuevos:

![Tabla de nombres de roles nuevos](media/whats-new/azure-role.png)

---

### <a name="azure-ad-b2c-support-for-auth-code-flow-for-spas-using-msal-js-2x"></a>Compatibilidad de Azure AD B2C con el flujo de código de autorización para aplicaciones de página única con MSAL JS 2.x

**Tipo:** Característica modificada  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
MSAL.js versión 2.x ahora incluye compatibilidad con el flujo de código de autorización para aplicaciones web de página única (SPA). Azure AD B2C ahora admitirá el uso del tipo de aplicación SPA en Azure Portal y el uso del flujo de código de autorización de MSAL.js con PKCE para las aplicaciones de página única. Esto permitirá que las aplicaciones SPA que usan Azure AD B2C mantengan el inicio de sesión único con exploradores más recientes y cumplan las recomendaciones del protocolo de autenticación más recientes. Comience con el tutorial [Registro de una aplicación de página única (SPA) en Azure Active Directory B2C](../../active-directory-b2c/tutorial-register-spa.md).

---

### <a name="updates-to-remember-multi-factor-authentication-mfa-on-a-trusted-device-setting"></a>Actualizaciones para recordar Multi-Factor Authentication (MFA) en una configuración de dispositivo de confianza

**Tipo:** Característica modificada  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 

Recientemente hemos actualizado la opción [Recordar Multi-Factor Authentication (MFA)](../authentication/howto-mfa-mfasettings.md#remember-multi-factor-authentication) en una característica de dispositivo de confianza para ampliar la autenticación hasta un máximo de 365 días. Las licencias de Azure Active Directory (Azure AD) Premium también pueden usar la [directiva Acceso condicional: frecuencia de inicio de sesión](../conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency), que proporciona más flexibilidad para la configuración de la reautenticación.

Para lograr una experiencia de usuario óptima, se recomienda usar la frecuencia de inicio de sesión Acceso condicional para ampliar la duración de las sesiones en dispositivos de confianza, ubicaciones o sesiones de bajo riesgo como alternativa a recordar MFA en un dispositivo de confianza. Para empezar, revise nuestras [instrucciones más recientes sobre la optimización de la experiencia de reautenticación](../authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md).

---

## <a name="september-2020"></a>Septiembre de 2020

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---september-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD, septiembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Coda](../saas-apps/coda-provisioning-tutorial.md)
- [Cofense Recipient Sync](../saas-apps/cofense-provision-tutorial.md)
- [InVision](../saas-apps/invision-provisioning-tutorial.md)
- [myday](../saas-apps/myday-provision-tutorial.md)
- [SAP Analytics Cloud](../saas-apps/sap-analytics-cloud-provisioning-tutorial.md)
- [Webroot Security Awareness](../saas-apps/webroot-security-awareness-training-provisioning-tutorial.md)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---
### <a name="cloud-provisioning-public-preview-refresh"></a>Actualización de la versión preliminar pública del aprovisionamiento en la nube

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento en la nube de Azure AD **Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
La actualización de la versión preliminar pública del aprovisionamiento en la nube de Azure AD Connect presenta dos mejoras importantes desarrolladas a partir de los comentarios de los clientes: 

- Experiencia de asignación de atributos a través de Azure Portal

    Con esta característica, los administradores de TI pueden asignar atributos de usuario, grupo o contacto de AD a Azure AD con varios tipos de asignación presentes hoy en día. La asignación de atributos es una característica que se usa para normalizar los valores de los atributos que fluyen desde Active Directory hasta Azure Active Directory. Puede determinar si se debe asignar directamente el valor del atributo tal como es de AD a Azure AD o usar expresiones para transformar los valores del atributo durante el aprovisionamiento de usuarios. [Más información](../cloud-provisioning/how-to-attribute-mapping.md)

- Experiencia del usuario de prueba o aprovisionamiento a petición

    Una vez que haya realizado la configuración, puede que quiere llevar a cabo una prueba para ver si la transformación de usuario funciona según lo esperado antes de aplicarla a todos los usuarios del ámbito. Con el aprovisionamiento a petición, los administradores de TI pueden escribir el nombre distintivo (DN) de un usuario de AD y comprobar si se está sincronizando según lo previsto. El aprovisionamiento a petición proporciona una excelente manera de asegurarse de que las asignaciones de atributos realizadas con anterioridad siguen funcionando según lo esperado. [Más información](../cloud-provisioning/how-to-on-demand-provision.md)
 
---

### <a name="audited-bitlocker-recovery-in-azure-ad---public-preview"></a>Recuperación de BitLocker auditada en Azure AD: versión preliminar pública

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de acceso de dispositivos  
**Funcionalidad del producto:** Administración del ciclo de vida de dispositivos
 
Ahora, cuando los administradores de TI o los usuarios finales leen las claves de recuperación de BitLocker a las que tienen acceso, Azure Active Directory genera un registro de auditoría que captura quién ha accedido a la clave de recuperación. La misma auditoría proporciona detalles del dispositivo al que está asociada la clave de BitLocker.

Los usuarios finales pueden [acceder a sus claves de recuperación a través de Mi cuenta](../user-help/my-account-portal-devices-page.md#view-a-bitlocker-key). Los administradores de TI pueden acceder a las claves de recuperación mediante la [API de claves de recuperación de BitLocker en versión beta](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) o a través del portal de Azure AD. Para obtener más información, vea [Ver o copiar las claves de BitLocker en el portal de Azure AD](../devices/device-management-azure-portal.md#view-or-copy-bitlocker-keys).

---

### <a name="teams-devices-administrator-built-in-role"></a>Rol integrado de Administrador de dispositivos de Teams

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
Los usuarios con el rol de [Administrador de dispositivos de Teams](../roles/permissions-reference.md#teams-devices-administrator) pueden administrar [dispositivos certificados para Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/across-devices/devices) desde el centro de administración de Teams. 

Este rol permite a los usuarios ver todos los dispositivos de un solo vistazo, con la posibilidad de buscar y filtrar dispositivos. El usuario también puede comprobar los detalles de cada dispositivo, por ejemplo, la cuenta con la que se ha iniciado sesión, la marca y el modelo del dispositivo. El usuario puede cambiar la configuración en el dispositivo y actualizar las versiones de software. Este rol no concede permisos para comprobar la actividad de Teams ni la calidad de las llamadas del dispositivo.
 
---

### <a name="advanced-query-capabilities-for-directory-objects"></a>Capacidades de consulta avanzada para objetos de directorio

**Tipo:** Nueva característica  
**Categoría del servicio:** MS Graph  
**Funcionalidad del producto:** Experiencia para el desarrollador
 
Todas las nuevas capacidades de consulta presentadas para objetos de directorio en las API de Azure AD ahora están disponibles en el punto de conexión v1.0 y están listas para producción. Los desarrolladores pueden contar, buscar, filtrar y ordenar objetos de directorio y vínculos relacionados mediante los operadores estándar de OData.

Para obtener más información, vea la documentación [aquí](https://aka.ms/BlogPostMezzoGA); también puede enviar comentarios mediante esta [breve encuesta](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_yN8EPoGo5OpR1hgmCp1XxUMENJRkNQTk5RQkpWTE44NEk2U0RIV0VZRy4u).
 
---

### <a name="public-preview-continuous-access-evaluation-for-tenants-who-configured-conditional-access-policies"></a>Versión preliminar pública: evaluación de acceso continua para inquilinos que han configurado directivas de acceso condicional

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
La evaluación de acceso continua (CAE) ahora está disponible en versión preliminar pública para inquilinos de Azure AD con directivas de acceso condicional. Con CAE, los eventos de seguridad críticos y las directivas se evalúan en tiempo real. Esto incluye la deshabilitación de cuentas, el restablecimiento de contraseñas y el cambio de ubicación. Para obtener más información, vea [Evaluación continua de acceso](../conditional-access/concept-continuous-access-evaluation.md).

---

### <a name="public-preview-ask-users-requesting-an-access-package-additional-questions-to-improve-approval-decisions"></a>Versión preliminar pública: preguntas adicionales a los usuarios que solicitan un paquete de acceso para mejorar las decisiones de aprobación

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 
Los administradores ahora pueden exigir que los usuarios que solicitan un paquete de acceso respondan a preguntas adicionales más allá de la justificación comercial en el portal Mi acceso de administración de derechos de Azure AD. Las respuestas de los usuarios se muestran a los aprobadores para ayudarles a tomar una decisión de aprobación de acceso más precisa. Para obtener más información, vea [Recopilación de información del solicitante adicional para su aprobación (versión preliminar)](../governance/entitlement-management-access-package-approval-policy.md#collect-additional-requestor-information-for-approval-preview).
 
---

### <a name="public-preview-enhanced-user-management"></a>Versión preliminar pública: administración de usuarios mejorada

**Tipo:** Nueva característica  
**Categoría del servicio:** User Management  
**Funcionalidad del producto:** User Management
 

El portal de Azure AD se ha actualizado para facilitar la búsqueda de usuarios en las páginas Todos los usuarios y Usuarios eliminados. Estos son algunos de los cambios de esta versión preliminar: 
- Propiedades de usuario más visibles, incluidos el identificador de objeto, el estado de sincronización de directorios, el tipo de creación y el emisor de identidades.
- Al buscar ahora se permite la búsqueda combinada de nombres, correos electrónicos e identificadores de objeto.
- Filtrado mejorado por tipo de usuario (miembro, invitado y ninguno), estado de sincronización de directorios, tipo de creación, nombre de la empresa y nombre de dominio.
- Nuevas capacidades de organización en propiedades como el nombre, el nombre principal de usuario y la fecha de eliminación.
- Un nuevo recuento de usuarios totales que se actualiza con cualquier búsqueda o filtro.

Para obtener más información, vea [Mejoras en la administración de usuarios (versión preliminar) en Azure Active Directory](../enterprise-users/users-search-enhanced.md).

---

### <a name="new-notes-field-for-enterprise-applications"></a>Nuevo campo de notas para aplicaciones empresariales

**Tipo:** Nueva característica  
**Categoría del servicio:** **Funcionalidad del producto:** Aplicaciones empresariales SSO

Puede agregar notas de texto libre a aplicaciones empresariales. Puede agregar cualquier información relevante que le ayude a administrar aplicaciones a aplicaciones empresariales. Para más información, consulte [Inicio rápido: Configuración de las propiedades de una aplicación en el inquilino de Azure Active Directory (Azure AD)](../manage-apps/add-application-portal-configure.md). 

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---september-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, septiembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En septiembre de 2020 se han agregado las siguientes 34 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[VMware Horizon - Unified Access Gateway](), [Pulse Secure PCS](../saas-apps/vmware-horizon-unified-access-gateway-tutorial.md), [Inventory360](../saas-apps/pulse-secure-pcs-tutorial.md), [Frontitude](https://services.enteksystems.de/sso/microsoft/signup), [BookWidgets](https://www.bookwidgets.com/sso/office365), [ZVD_Server](https://zaas.zenmutech.com/user/signin), [HashData for Business](https://hashdata.app/login.xhtml), [SecureLogin](https://securelogin.securelogin.nu/sso/azure/login), [CyberSolutions MAILBASEΣ/CMSS](../saas-apps/cybersolutions-mailbase-tutorial.md), [CyberSolutions CYBERMAILΣ](../saas-apps/cybersolutions-cybermail-tutorial.md), [LimbleCMMS](https://auth.limblecmms.com/), [Glint Inc](../saas-apps/glint-inc-tutorial.md), [zeroheight](../saas-apps/zeroheight-tutorial.md), [Gender Fitness](https://app.genderfitness.com/), [Coeo Portal](https://my.coeo.com/), [Grammarly](../saas-apps/grammarly-tutorial.md), [Fivetran](../saas-apps/fivetran-tutorial.md), [Kumolus](../saas-apps/kumolus-tutorial.md), [RSA Archer Suite](../saas-apps/rsa-archer-suite-tutorial.md), [TeamzSkill](../saas-apps/teamzskill-tutorial.md), [raumfürraum](../saas-apps/raumfurraum-tutorial.md), [Saviynt](../saas-apps/saviynt-tutorial.md), [BizMerlinHR](https://marketplace.bizmerlin.net/bmone/signup), [Mobile Locker](../saas-apps/mobile-locker-tutorial.md), [Zengine](../saas-apps/zengine-tutorial.md), [CloudCADI](https://app.cloudcadi.com/login), [Simfoni Analytics](https://simfonianalytics.com/accounts/microsoft/login/), [Priva Identity & Access Management](https://my.priva.com/), [Nitro Pro](https://www.gonitro.com/nps/product-details/downloads), [Eventfinity](../saas-apps/eventfinity-tutorial.md), [Fexa](../saas-apps/fexa-tutorial.md), [Secured Signing Enterprise Portal](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [Secured Signing Enterprise Portal AAD Setup](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [Wistec Online](https://wisteconline.com/auth/oidc), [Oracle PeopleSoft - Protected by F5 BIG-IP APM](../saas-apps/oracle-peoplesoft-protected-by-f5-big-ip-apm-tutorial.md)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar una aplicación en la galería de aplicaciones de Azure AD, lea los detalles de https://aka.ms/AzureADAppRequest.

---

### <a name="new-delegation-role-in-azure-ad-entitlement-management-access-package-assignment-manager"></a>Nuevo rol de delegación en la administración de derechos de Azure AD: Administrador de asignaciones de paquetes de acceso

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 
Se ha agregado un nuevo rol de Administrador de asignaciones de paquetes de acceso a la administración de derechos de Azure AD para proporcionar permisos pormenorizados para administrar asignaciones. Ahora puede delegar tareas a un usuario de este rol, que puede delegar la administración de asignaciones de un paquete de acceso a un propietario empresarial. Pero un administrador de asignaciones de paquetes de acceso no puede modificar las directivas de paquetes de acceso ni otras propiedades establecidas por los administradores. 

Con este nuevo rol, se beneficia de los privilegios mínimos necesarios para delegar la administración de asignaciones y mantener el control administrativo sobre todas las demás configuraciones de paquetes de acceso. Para obtener más información, vea [Roles de administración de derechos](../governance/entitlement-management-delegate.md#entitlement-management-roles).
 
---

### <a name="changes-to-privileged-identity-managements-onboarding-flow"></a>Cambios en el flujo de incorporación de Privileged Identity Management

**Tipo:** Característica modificada  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
Anteriormente, la incorporación a Privileged Identity Management (PIM) requería el consentimiento del usuario y un flujo de incorporación en la hoja de PIM que incluía la inscripción en Azure AD MFA. Con la reciente integración de la experiencia de PIM en la hoja de roles y administradores de Azure AD, se elimina dicha experiencia. Cualquier inquilino con una licencia P2 válida se incorpora automáticamente a PIM.

La incorporación a PIM no tiene ningún efecto adverso directo en el inquilino. Puede contar con los siguientes cambios:
- Opciones de asignación adicionales, como activa frente a válida con hora de inicio y finalización al realizar una asignación en la hoja de roles y administradores tanto de PIM como de Azure AD. 
- Mecanismos de ámbito adicionales, como Unidades administrativas y roles personalizados, incorporados directamente en la experiencia de asignación. 
- Si es un administrador global o un administrador de roles con privilegios, puede empezar a recibir algunos correos electrónicos adicionales, como el resumen semanal de PIM. 
- También podría ver la entidad de servicio MS-PIM en el registro de auditoría relacionado con la asignación de roles. Este cambio esperado no debe afectar al flujo de trabajo normal.

 Para obtener más información, vea [Empiece a usar Privileged Identity Management](../privileged-identity-management/pim-getting-started.md).

---

### <a name="azure-ad-entitlement-management-the-select-pane-of-access-package-resources-now-shows-by-default-the-resources-currently-in-the-selected-catalog"></a>Administración de derechos de Azure AD: el panel Seleccionar de los recursos de paquete de acceso muestra ahora de forma predeterminada los recursos contenidos actualmente en el catálogo seleccionado

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 

En el flujo de creación de paquetes de acceso, en la pestaña Roles de recursos, el comportamiento del panel Seleccionar cambia. Actualmente, el comportamiento predeterminado es mostrar todos los recursos que son propiedad del usuario y los recursos agregados al catálogo seleccionado. 

Esta experiencia se modifica para mostrar de forma predeterminada solo los recursos agregados actualmente al catálogo, de modo que los usuarios puedan elegir fácilmente los recursos del catálogo. La actualización ayuda a detectar los recursos que se van a agregar a los paquetes de acceso y reduce el riesgo de agregar de forma accidental los recursos que pertenecen al usuario y que no forman parte del catálogo. Para obtener más información, vea [Creación de un paquete de acceso en la administración de derechos de Azure AD](../governance/entitlement-management-access-package-create.md#resource-roles).
 
---

## <a name="august-2020"></a>Agosto de 2020 
 
### <a name="updates-to-azure-multi-factor-authentication-server-firewall-requirements"></a>Actualizaciones de los requisitos de firewall del Servidor Microsoft Azure Multi-Factor Authentication

**Tipo:** Plan de cambio  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
A partir del 1 de octubre de 2020, los requisitos de firewall del Servidor Azure MFA requerirán intervalos IP adicionales.

Si tiene reglas de firewall de salida en la organización, actualícelas para que los servidores MFA puedan comunicarse con todos los intervalos IP necesarios. Los intervalos IP se documentan en [Requisitos de firewall de Servidor Microsoft Azure Multi-Factor Authentication](../authentication/howto-mfaserver-deploy.md#azure-multi-factor-authentication-server-firewall-requirements).

---

### <a name="upcoming-changes-to-user-experience-in-identity-secure-score"></a>Próximos cambios en la experiencia del usuario en Puntuación segura de identidad

**Tipo:** Plan de cambio  
**Categoría del servicio:** **Funcionalidad del producto:** Identity Protection Seguridad y protección de la identidad

Vamos a actualizar el portal de Puntuación segura de identidad para que se corresponda con los cambios introducidos en la [nueva versión](/microsoft-365/security/mtp/microsoft-secure-score-whats-new) de Puntuación de seguridad de Microsoft. 

La versión preliminar con los cambios estará disponible a principios de septiembre. Los cambios en la versión preliminar incluyen:
- "Puntuación segura de identidad" se ha cambiado a "Puntuación de seguridad de la identidad" a fin de alinear la marca con Puntuación de seguridad de Microsoft.
- Puntos normalizados para la escala estándar que se indican en porcentajes en lugar de con puntos

En esta versión preliminar, los clientes pueden alternar entre la experiencia existente y la nueva. Esta versión preliminar pública durará hasta finales de noviembre de 2020. Después de la versión preliminar, los clientes se dirigirán automáticamente a la nueva experiencia del usuario.

---

### <a name="new-restricted-guest-access-permissions-in-azure-ad---public-preview"></a>Nuevos permisos de acceso de invitado restringido en Azure AD: versión preliminar pública

**Tipo:** Nueva característica  
**Categoría del servicio:** Control de acceso   
**Funcionalidad del producto:** User Management

Hemos actualizado los permisos del nivel de directorio para los usuarios invitados. Estos permisos permiten a los administradores requerir restricciones y controles adicionales en el acceso de los usuarios invitados externos. Los administradores ahora pueden agregar restricciones adicionales para el acceso de los invitados externos a la información de pertenencia y al perfil de los usuarios y los grupos. Con esta característica de la versión preliminar pública, los clientes pueden administrar el acceso de los usuarios externos a escala mediante la ofuscación de las pertenencias a grupos, incluida la restricción de los usuarios invitados para ver la pertenencia de los grupos en los que se encuentran.

Para más información, consulte [Restricción del acceso a usuarios invitados](../enterprise-users/users-restrict-guest-permissions.md) y [Permisos predeterminados de usuario](./users-default-permissions.md).
 
---

### <a name="general-availability-of-delta-queries-for-service-principals"></a>Disponibilidad general de consultas delta para entidades de servicio

**Tipo:** Nueva característica  
**Categoría del servicio:** MS Graph  
**Funcionalidad del producto:** Experiencia para el desarrollador
 
Microsoft Graph Delta Query ahora admite el tipo de recurso en v1.0:
- Entidad de servicio

Ahora los clientes pueden realizar un seguimiento eficaz de los cambios en esos recursos. Proporciona la mejor solución para sincronizar los cambios en esos recursos con un almacén de datos local. Para aprender a configurar estos recursos en una consulta, vea [Usar la consulta delta para realizar el seguimiento de los cambios en datos de Microsoft Graph](/graph/delta-query-overview).
 
---

### <a name="general-availability-of-delta-queries-for-oauth2permissiongrant"></a>Disponibilidad general de consultas delta para oAuth2PermissionGrant

**Tipo:** Nueva característica  
**Categoría del servicio:** MS Graph  
**Funcionalidad del producto:** Experiencia para el desarrollador

Microsoft Graph Delta Query ahora admite el tipo de recurso en v1.0:
- OAuth2PermissionGrant

Los clientes ahora pueden realizar el seguimiento de los cambios de esos recursos de manera eficaz. Proporciona la mejor solución para sincronizar los cambios en esos recursos con un almacén de datos local. Para aprender a configurar estos recursos en una consulta, vea [Usar la consulta delta para realizar el seguimiento de los cambios en datos de Microsoft Graph](/graph/delta-query-overview).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---august-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, agosto de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En agosto de 2020 se han agregado las siguientes 25 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[Backup365](https://portal.backup365.io/login), [Soapbox](https://app.soapboxhq.com/create?step=auth&provider=azure-ad2-oauth2), [Alma SIS](https://almau.getalma.com/), [Enlyft Dynamics 365 Connector](http://enlyft.com/), [Serraview Space Utilization Software Solutions](../saas-apps/serraview-space-utilization-software-solutions-tutorial.md), [Uniq](https://web.uniq.app/), [Visibly](../saas-apps/visibly-tutorial.md), [Zylo](../saas-apps/zylo-tutorial.md), [Edmentum - Courseware Assessments Exact Path](https://auth.edmentum.com/elf/login), [CyberLAB](https://cyberlab.evolvesecurity.com/#/welcome), [Altamira HRM](../saas-apps/altamira-hrm-tutorial.md), [WireWheel](../saas-apps/wirewheel-tutorial.md), [Zix Compliance and Capture](https://sminstall.zixcorp.com/teams/teams.php?install_request=true&tenant_id=common), [Greenlight Enterprise Business Controls Platform](../saas-apps/greenlight-enterprise-business-controls-platform-tutorial.md), [Genetec Clearance](https://www.clearance.network/), [iSAMS](../saas-apps/isams-tutorial.md), [VeraSMART](../saas-apps/verasmart-tutorial.md), [Amiko](https://amiko.web.rivero.app/), [Twingate](https://auth.twingate.com/signup), [Funnel Leasing](https://nestiolistings.com/sso/oidc/azure/authorize/), [Scalefusion](https://scalefusion.com/users/sign_in/), [Bpanda](https://goto.bpanda.com/login), [Vivun Calendar Connect](https://app.vivun.com/dashboard/calendar/connect), [FortiGate SSL VPN](../saas-apps/fortigate-ssl-vpn-tutorial.md), [Wandera End User](https://www.wandera.com/)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí https://aka.ms/AzureADAppRequest.

---

### <a name="resource-forests-now-available-for-azure-ad-ds"></a>Los bosques de recursos ahora están disponibles para Azure AD DS. 

**Tipo:** **Categoría del servicio:** Nueva característica Azure AD Domain Services   
**Funcionalidad del producto:** Azure AD Domain Services
 
La funcionalidad de los bosques de recursos en Azure AD Domain Services ahora está disponible con carácter general. Ahora puede habilitar la autorización sin sincronización de hash de contraseñas para usar Azure AD Domain Services, incluida la autorización con tarjetas inteligentes. Para más información consulte [Conceptos y características de los conjuntos de réplicas de Azure Active Directory Domain Services (versión preliminar)](../../active-directory-domain-services/concepts-replica-sets.md).
 
---

### <a name="regional-replica-support-for-azure-ad-ds-managed-domains-now-available"></a>Ya está disponible la compatibilidad con la réplica regional para dominios administrados de Azure AD DS.

**Tipo:** Nueva característica   
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services
 
Puede ampliar un dominio administrado para que tenga más de un conjunto de réplicas por cada inquilino de Azure AD. Los conjuntos de réplicas pueden agregarse a cualquier red virtual emparejada en cualquier región de Azure que admita Azure AD Domain Services. Contar con conjuntos de réplicas adicionales en diferentes regiones de Azure facilita la recuperación geográfica ante desastres de las aplicaciones heredadas si una región de Azure se queda sin conexión. Para más información consulte [Conceptos y características de los conjuntos de réplicas de Azure Active Directory Domain Services (versión preliminar)](../../active-directory-domain-services/concepts-replica-sets.md).

---

### <a name="general-availability-of-azure-ad-my-sign-ins"></a>Disponibilidad general de Mis inicios de sesión de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Experiencias de usuario final
 
Mis inicios de sesión de Azure AD es una característica nueva que permite a los usuarios empresariales revisar su historial de inicios de sesión para comprobar si hay alguna actividad inusual. Además, esta característica posibilita a los usuarios finales comunicar si fueron o no los causantes de actividades sospechosas. Para más información sobre el uso de esta característica, consulte [Visualización de las actividades de inicio de sesión recientes](../user-help/my-account-portal-sign-ins-page.md#confirm-unusual-activity).
 
---

### <a name="sap-successfactors-hr-driven-user-provisioning-to-azure-ad-is-now-generally-available"></a>El aprovisionamiento de usuarios controlado por SuccessFactors HR de SAP para Azure AD ya está disponible con carácter general

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
Ahora puede integrar SAP SuccessFactors como el origen de identidad de autoridad con Azure AD y automatizar el ciclo de vida de identidad de un extremo a otro mediante eventos de RR. HH. como nuevas contrataciones y conclusión de contratos para impulsar el aprovisionamiento y el desaprovisionamiento de las cuentas en Azure AD. 

Para más información sobre cómo configurar el aprovisionamiento de entrada de SAP SuccessFactors para Azure AD, consulte el tutorial [Configuración del aprovisionamiento de usuarios de SAP SuccessFactors a Active Directory](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md).
 
---

### <a name="custom-open-id-connect-ms-graph-api-support-for-azure-ad-b2c"></a>Compatibilidad con Open ID Connect personalizado de MS Graph API para Azure AD B2C

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
Anteriormente, los proveedores personalizados de Open ID Connect solo se podían agregar o administrar mediante Azure Portal. Ahora, los clientes de Azure AD B2C pueden agregarlos y administrarlos con la versión beta de Microsoft Graph API. Para obtener información sobre cómo configurar este recurso con las API, consulte [Tipo de recurso identityProvider](/graph/api/resources/identityprovider?view=graph-rest-beta).
 
---

### <a name="assign-azure-ad-built-in-roles-to-cloud-groups"></a>Asignación de roles integrados de Azure AD a grupos en la nube

**Tipo:** Nueva característica  
**Categoría del servicio:** Roles de Azure AD  
**Funcionalidad del producto:** Control de acceso

Ahora puede asignar roles integrados de Azure AD a grupos en la nube con esta nueva característica. Por ejemplo, puede asignar el rol Administrador de SharePoint al grupo Contoso_SharePoint_Admins. También puede usar PIM para convertir el grupo en miembro del rol, en lugar de conceder acceso permanente. Para obtener información sobre cómo configurar esta característica, consulte [Uso de grupos en la nube para administrar asignaciones de roles en Azure Active Directory (versión preliminar)](../roles/groups-concept.md).
 
---

### <a name="insights-business-leader-built-in-role-now-available"></a>Ya está disponible el rol integrado Coordinador de Insights de la empresa.

**Tipo:** Nueva característica  
**Categoría del servicio:** Roles de Azure AD  
**Funcionalidad del producto:** Control de acceso
 
Los usuarios con el rol Coordinador de Insights de la empresa pueden acceder a un conjunto de paneles y conclusiones a través de la [aplicación M365 Insights](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics). Esto incluye el acceso completo a todos los paneles y la funcionalidad de exploración de datos y conclusiones presentadas. Sin embargo, los usuarios con este rol no tienen acceso a las opciones de configuración del producto, que es responsabilidad del rol Administrador de Insights. Para más información acerca de este rol, consulte [Permisos de roles de administrador en Azure Active Directory](../roles/permissions-reference.md#insights-business-leader).
 
---

### <a name="insights-administrator-built-in-role-now-available"></a>El rol integrado Administrador de Insights ya está disponible.

**Tipo:** Nueva característica  
**Categoría del servicio:** Roles de Azure AD  
**Funcionalidad del producto:** Control de acceso
 
Los usuarios con el rol Administrador de Insights pueden tener acceso al conjunto completo de funcionalidades administrativas de la [aplicación M365 Insights](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics). Un usuario con este rol puede leer información de directorios, supervisar el mantenimiento del servicio, presentar vales de soporte técnico y acceder a los aspectos de configuración del administrador de Insights. Para más información sobre este rol, consulte [Permisos de roles de administrador en Azure Active Directory](../roles/permissions-reference.md#insights-administrator).
 
--- 

### <a name="application-admin-and-cloud-application-admin-can-manage-extension-properties-of-applications"></a>El administrador de aplicaciones y el administrador de aplicaciones en la nube pueden administrar las propiedades de extensión de las aplicaciones.

**Tipo:** Característica modificada  
**Categoría del servicio:** Roles de Azure AD  
**Funcionalidad del producto:** Control de acceso
 
Anteriormente, solo el administrador global podía administrar la [propiedad de extensión](/graph/api/application-post-extensionproperty?view=graph-rest-beta&tabs=http). Ahora vamos a habilitar esta funcionalidad para el administrador de la aplicación y también para el administrador de aplicaciones en la nube.
 
---

### <a name="mim-2016-sp2-hotfix-462630-and-connectors-1113010"></a>Revisión 4.6.263.0 de MIM 2016 SP2 y conectores 1.1.1301.0

**Tipo:** Característica modificada  
**Categoría del servicio:** Microsoft Identity Manager  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Un [paquete acumulativo de revisiones (compilación 4.6.263.0)](https://support.microsoft.com/help/4576473/hotfix-rollup-package-build-4-6-263-0-is-available-for-microsoft-ident) está disponible para Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2). Este paquete acumulativo contiene actualizaciones para los componentes de PAM, MIM CM y el administrador de sincronización de MIM. Además, la compilación de conectores genéricos de MIM 1.1.1301.0 incluye actualizaciones para el conector de Graph.

---
 
## <a name="july-2020"></a>Julio de 2020

### <a name="as-an-it-admin-i-want-to-target-client-apps-using-conditional-access"></a>Como administrador de TI, quiero dirigirme a las aplicaciones cliente mediante el acceso condicional

**Tipo:** Plan de cambio   
**Categoría del servicio:** Acceso condicional  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Con la versión de disponibilidad general (GA) de la condición de las aplicaciones de cliente en el acceso condicional, ahora las nuevas directivas se aplicarán de manera predeterminada a todas las aplicaciones cliente. Esto incluye los clientes de autenticación heredados. Las directivas existentes permanecerán sin cambios, pero la opción de alternancia *Configurar sí/no* se quitará de las directivas existentes para ver fácilmente las aplicaciones cliente que se aplican a través de la directiva. 

Al crear una nueva directiva, asegúrese de excluir los usuarios y las cuentas de servicio que siguen usando la autenticación heredada. Si no lo hace, se bloquearán. [Más información](../conditional-access/concept-conditional-access-conditions.md).
 
---

### <a name="upcoming-scim-compliance-fixes"></a>Próximas correcciones de cumplimiento de SCIM

**Tipo:** Plan de cambio  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
El servicio de aprovisionamiento de Azure AD aprovecha el estándar SCIM para la integración con las aplicaciones. Nuestra implementación del estándar SCIM está evolucionando y esperamos realizar cambios en nuestro comportamiento en torno a la realización de las operaciones de revisión, así como al establecimiento de la propiedad "activa" en un recurso. [Más información](../app-provisioning/application-provisioning-config-problem-scim-compatibility.md).
 
---

### <a name="group-owner-setting-on-azure-admin-portal-will-be-changed"></a>Se cambiará la configuración del propietario del grupo en el portal de administración de Azure

**Tipo:** Plan de cambio  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

La configuración del propietario en la página de configuración de grupos general puede configurarse para restringir los privilegios de asignación de propietario a un grupo limitado de usuarios en el Panel de acceso y el portal de administración de Azure. Pronto tendremos la capacidad de asignar privilegios de propietario de grupo no solo en estos dos portales de la experiencia de usuario, sino también aplicar la directiva en el back-end para proporcionar un comportamiento coherente entre los puntos de conexión, como PowerShell y Microsoft Graph. 

Comenzaremos a deshabilitar la configuración actual de los clientes que no la estén usando y ofreceremos una opción para el ámbito de los usuarios de privilegios de propietario de grupo en los próximos meses. Para obtener instrucciones sobre la actualización de la configuración de grupo, consulte la edición de la información de grupo mediante [Azure Active Directory](./active-directory-groups-settings-azure-portal.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context).

---

### <a name="azure-active-directory-registration-service-is-ending-support-for-tls-10-and-11"></a>El servicio de registro de Azure Active Directory va a finalizar la compatibilidad con TLS 1.0 y 1.1

**Tipo:** Plan de cambio  
**Categoría del servicio:** Administración y registro de dispositivos  
**Funcionalidad del producto:** plataforma

La Seguridad de la capa de transporte (TLS) 1.2 y los servidores y clientes de actualización se comunicarán pronto con el servicio Registro de dispositivos de Azure Active Directory. La compatibilidad con TLS 1.0 y 1.1 para la comunicación con el servicio Registro de dispositivos de Azure AD se retirará:
- El 31 de agosto de 2020, en todas las nubes independientes (GCC High, DoD, etc.).
- El 30 de octubre de 2020, en todas las nubes comerciales.

[Más información](../devices/reference-device-registration-tls-1-2.md) sobre TLS 1.2 para el servicio de registro de Azure AD.

---

### <a name="windows-hello-for-business-sign-ins-visible-in-azure-ad-sign-in-logs"></a>Inicios de sesión de Windows Hello para empresas visibles en registros de inicio de sesión de Azure AD

**Tipo:** Corregido  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
Windows Hello para empresas permite a los usuarios finales iniciar sesión en máquinas Windows con un gesto (como un PIN o datos biométricos). Los administradores de Azure AD pueden querer diferenciar los inicios de sesión de Windows Hello para empresas de otros inicios de sesión de Windows como parte del recorrido de una organización para la autenticación sin contraseña. 

Los administradores ya pueden ver si una autenticación de Windows ha usado Windows Hello para empresas mediante la comprobación de la pestaña Detalles de autenticación de un evento de inicio de sesión de Windows en la hoja Inicios de sesión de Azure AD en Azure Portal. Las autenticaciones de Windows Hello para empresas incluirán "WindowsHelloForBusiness" en el campo Método de autenticación. Para obtener más información sobre cómo interpretar los registros de inicio de sesión, consulte la [documentación de registros de inicio de sesión](../reports-monitoring/concept-sign-ins.md).
 
---

### <a name="fixes-to-group-deletion-behavior-and-performance-improvements"></a>Correcciones del comportamiento de eliminación de grupos y mejoras de rendimiento

**Tipo:** Corregido  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
Anteriormente, cuando un grupo cambiaba de "en el ámbito" a "fuera de ámbito" y un administrador hacía clic en reiniciar antes de que se completara el cambio, el objeto de grupo no se eliminaba. Ahora, el objeto de grupo de la aplicación de destino se eliminará cuando salga del ámbito (deshabilitado, eliminado, sin asignar o sin pasar el filtro de ámbito). [Más información](../app-provisioning/how-provisioning-works.md#incremental-cycles).
 
---

### <a name="public-preview-admins-can-now-add-custom-content-in-the-email-to-reviewers-when-creating-an-access-review"></a>Versión preliminar pública: los administradores ya pueden agregar contenido personalizado en el correo electrónico para los revisores al crear una revisión de acceso.

**Tipo:** Nueva característica  
**Categoría del servicio:** Revisiones de acceso  
**Funcionalidad del producto:** Identity Governance
 
Cuando se crea una nueva revisión de acceso, el revisor recibe un correo electrónico que le solicita que complete la revisión de acceso. Muchos de nuestros clientes han solicitado la posibilidad de agregar contenido personalizado al correo electrónico, como información de contacto, u otro contenido adicional de soporte técnico para guiar al revisor. 

Ya disponible en la versión preliminar pública, los administradores pueden especificar contenido personalizado en el correo electrónico que se envía a los revisores mediante la adición de contenido en la sección "Opciones avanzadas" de las revisiones de acceso de Azure AD. Para obtener instrucciones sobre cómo crear revisiones de acceso, consulte [Creación de una revisión de acceso de los grupos y las aplicaciones en las revisiones de acceso de Azure AD](../governance/create-access-review.md).
 
---

### <a name="authorization-code-flow-for-single-page-apps-available"></a>Flujo de código de autorización para aplicaciones de una sola página disponibles

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Experiencia para el desarrollador
 
Debido a las restricciones de cookies de terceros como Safari ITP aplicadas en el explorador, las aplicaciones de página única (SPA) tendrán que usar el flujo de código de autorización en lugar del flujo implícito para mantener el SSO. MSAL.js v2.x admite ahora el flujo de código de autorización. 

Las actualizaciones correspondientes están disponibles en Azure Portal. De este modo, podrá actualizar sus aplicaciones de página única para que sean del tipo "spa" y utilicen el flujo de código de autenticación. Consulte [Inicio de sesión de los usuarios y obtención de un token de acceso en una SPA de JavaScript mediante el flujo de código de autorización](../develop/quickstart-v2-javascript-auth-code.md) para obtener más instrucciones.
 
---

### <a name="azure-ad-application-proxy-now-supports-the-remote-desktop-services-web-client"></a>Azure AD Application Proxy ya admite el cliente web de Servicios de Escritorio remoto

**Tipo:** Nueva característica  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso

Azure AD Application Proxy ya admite el cliente web de Servicios de Escritorio remoto (RDS). El cliente web de RDS permite a los usuarios tener acceso a la infraestructura de Escritorio remoto a través de cualquier explorador compatible con HTLM5, como Microsoft Edge, Internet Explorer 11, Google Chrome, etc. Los usuarios pueden interactuar con escritorios o aplicaciones remotos como lo harían con un dispositivo local desde cualquier lugar. Mediante el uso de Azure AD Application Proxy, puede aumentar la seguridad de la implementación de RDS mediante la exigencia de directivas de acceso condicional y autenticación previa para todos los tipos de aplicaciones cliente enriquecidas. Para obtener instrucciones, consulte [Publicación de Escritorio Remoto con Azure AD Application Proxy](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md).
 
---

### <a name="next-generation-azure-ad-b2c-user-flows-in-public-preview"></a>Flujos de usuario de última generación de Azure AD B2C en versión preliminar pública

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
La experiencia simplificada de flujo de usuario ofrece paridad de características con características en vista previa (GB) y es donde se encuentran todas las características nuevas. Los usuarios podrán habilitar nuevas características en el mismo flujo de usuario, lo que reduce la necesidad de crear varias versiones con cada nueva versión de actualización de características. Por último, la nueva experiencia de usuario fácil de usar simplifica la selección y creación de flujos de usuario. Para probarla, [cree un flujo de usuario](../../active-directory-b2c/tutorial-create-user-flows.md). 

Para más información sobre los flujos de usuario, consulte [Versiones de flujos de usuario de Azure Active Directory B2C](../../active-directory-b2c/user-flow-versions.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---july-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, julio de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En julio de 2020, agregamos 55 aplicaciones nuevas a la galería de aplicaciones con compatibilidad de federación:

[Clap Your Hands](http://www.rmit.com.ar/), [Appreiz](https://microsoftteams.appreiz.com/), [Inextor Vault](https://inexto.com/inexto-suite/inextor), [Beekast](https://my.beekast.com/), [Templafy OpenID Connect](https://app.templafy.com/), [PeterConnects receptionist](https://msteams.peterconnects.com/), [AlohaCloud](https://appfusions.alohacloud.com/auth), [Control Tower](https://bpm.tnxcorp.com/sso/microsoft), [Cocoom](https://start.cocoom.com/), [COINS Construction Cloud](https://sso.coinsconstructioncloud.com/#login/), [Medxnote MT](https://task.teamsmain.medx.im/authorization), [Reflekt](https://reflekt.konsolute.com/login), [Rever](https://app.reverscore.net/access), [MyCompanyArchive](https://login.mycompanyarchive.com/), [GReminders](https://app.greminders.com/o365-oauth), [Titanfile](../saas-apps/titanfile-tutorial.md), [Wootric](../saas-apps/wootric-tutorial.md), [SolarWinds Orion](https://support.solarwinds.com/SuccessCenter/s/orion-platform?language=en_US),  [OpenText Directory Services](../saas-apps/opentext-directory-services-tutorial.md), [Datasite](../saas-apps/datasite-tutorial.md), [BlogIn](../saas-apps/blogin-tutorial.md), [IntSights](../saas-apps/intsights-tutorial.md), [kpifire](../saas-apps/kpifire-tutorial.md), [Textline](../saas-apps/textline-tutorial.md), [Cloud Academy - SSO](../saas-apps/cloud-academy-sso-tutorial.md), [Community Spark](../saas-apps/community-spark-tutorial.md), [Chatwork](../saas-apps/chatwork-tutorial.md), [CloudSign](../saas-apps/cloudsign-tutorial.md), [C3M Cloud Control](../saas-apps/c3m-cloud-control-tutorial.md), [SmartHR](https://smarthr.jp/), [NumlyEngage™](../saas-apps/numlyengage-tutorial.md), [Michigan Data Hub Single Sign-On](../saas-apps/michigan-data-hub-single-sign-on-tutorial.md), [Egress](../saas-apps/egress-tutorial.md), [SendSafely](../saas-apps/sendsafely-tutorial.md), [Eletive](https://app.eletive.com/), [Right-Hand Cybersecurity ADI](https://right-hand.ai/), [Fyde Enterprise Authentication](https://enterprise.fyde.com/), [Verme](../saas-apps/verme-tutorial.md), [Lenses.io](../saas-apps/lensesio-tutorial.md), [Momenta](../saas-apps/momenta-tutorial.md), [Uprise](https://app.uprise.co/sign-in), [Q](https://q.moduleq.com/login), [CloudCords](../saas-apps/cloudcords-tutorial.md), [TellMe Bot](https://tellme365liteweb.azurewebsites.net/), [Inspire](https://app.inspiresoftware.com/), [Maverics Identity Orchestrator SAML Connector](https://www.strata.io/identity-fabric/), [Smartschool (School Management System)](https://smartschoolz.com/login), [Zepto - Intelligent timekeeping](https://user.zepto-ai.com/signin), [Studi.ly](https://studi.ly/), [Trackplan](http://www.trackplanfm.com/), [Skedda](../saas-apps/skedda-tutorial.md), [WhosOnLocation](../saas-apps/whos-on-location-tutorial.md), [Coggle](../saas-apps/coggle-tutorial.md), [Kemp LoadMaster](https://kemptechnologies.com/cloud-load-balancer/), [BrowserStack Single Sign-on](../saas-apps/browserstack-single-sign-on-tutorial.md)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí: https://aka.ms/AzureADAppRequest.

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---july-2020"></a>Nuevos conectores de aprovisionamiento disponibles en la galería de aplicaciones de Azure AD, julio de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para la aplicación recién integrada [LinkedIn Learning](../saas-apps/linkedin-learning-provisioning-tutorial.md).

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).

---

### <a name="view-role-assignments-across-all-scopes-and-ability-to-download-them-to-a-csv-file"></a>Visualización de las asignaciones de roles en todos los ámbitos y capacidad de descargarlas en un archivo CSV

**Tipo:** Característica modificada  
**Categoría del servicio:** Roles de Azure AD  
**Funcionalidad del producto:** Control de acceso
 
Ahora puede ver las asignaciones de roles en todos los ámbitos de un rol en la pestaña "Roles y administradores" en el portal de Azure AD. También puede descargar estas asignaciones de roles para cada rol en un archivo CSV. Para obtener instrucciones sobre cómo ver y agregar asignaciones de roles, consulte [Visualización y asignación de roles de administrador en Azure Active Directory](../roles/manage-roles-portal.md).
 
---

### <a name="azure-multi-factor-authentication-software-development-azure-mfa-sdk-deprecation"></a>Desuso del desarrollo de software de Azure Multi-Factor Authentication (SDK de Azure MFA)

**Tipo:** Obsoleto  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
El desarrollo de software de Azure Multi-Factor Authentication (SDK de Azure MFA) alcanzó el final del ciclo de vida el 14 de noviembre de 2018, como se anunció por primera vez en noviembre de 2017. Microsoft apagará el servicio de SDK a partir del 30 de septiembre de 2020. Se producirá un error en todas las llamadas realizadas al SDK.

Si su organización usa el SDK de Azure MFA, debe realizar la migración antes del 30 de septiembre de 2020:
- SDK de Azure MFA para MIM:  Si usa el SDK con MIM, debe migrar al Servidor Azure MFA y activar Privileged Access Management (PAM) siguiendo estas [instrucciones](/microsoft-identity-manager/working-with-mfaserver-for-mim).   
- SDK de Azure MFA para aplicaciones personalizadas: Considere la posibilidad de integrar la aplicación en Azure AD y usar el acceso condicional para aplicar MFA. Para empezar, revise esta [página](../manage-apps/plan-an-application-integration.md). 

---

 
