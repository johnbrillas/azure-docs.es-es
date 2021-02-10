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
ms.date: 1/29/2021
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c835ddcee332499ef7853d0d2fa4fcdbcf8a1cf
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988871"
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
## <a name="january-2021"></a>Enero de 2021

### <a name="secret-token-will-be-a-mandatory-field-when-configuring-provisioning"></a>El token secreto será un campo obligatorio al configurar el aprovisionamiento

**Tipo:** Plan de cambio  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Anteriormente, el campo de token secreto podía quedarse vacío al configurar el aprovisionamiento en la aplicación personalizada/BYOA. Esta función estaba pensada para usarse únicamente para realizar pruebas. Actualizaremos la interfaz de usuario para que el campo sea obligatorio. 

Los clientes pueden evitar este requisito para fines de prueba mediante una marca de características en la dirección URL del explorador. [Más información](../app-provisioning/use-scim-to-provision-users-and-groups.md#authorization-to-provisioning-connectors-in-the-application-gallery).
 
---

### <a name="public-preview---customize-and-configure-android-shared-devices-for-firstline-workers-at-scale"></a>Versión preliminar pública: personalización y configuración de dispositivos compartidos Android a gran escala para trabajadores de primera línea

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración y registro de dispositivos  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Los equipos de Azure AD y Microsoft Endpoint Manager se han combinado para ofrecer la funcionalidad de personalizar, escalar y proteger los dispositivos de los trabajadores de primera línea.

Las siguientes funcionalidades en versión preliminar le permitirán hacer lo siguiente:
- Aprovisionar dispositivos compartidos Android a gran escala con Microsoft Endpoint Manager
- Proteger el acceso de los trabajadores por turnos mediante el acceso condicional basado en dispositivos
- Personalizar las experiencias de inicio de sesión de los trabajadores por turnos con Managed Home Screen

Para más información, vea [Personalización y configuración de dispositivos compartidos a gran escala para trabajadores de primera línea](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-and-configure-shared-devices-for-firstline-workers-at/ba-p/1751708).

---

### <a name="public-preview---provisioning-logs-can-now-be-downloaded-as-a-csv-or-json"></a>Versión preliminar pública: los registros de aprovisionamiento ahora se pueden descargar como CSV o JSON

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Los clientes pueden descargar los registros de aprovisionamiento como un archivo CSV o JSON desde la interfaz de usuario y con Graph API. Para más información, vea [Informes de aprovisionamiento en el portal de Azure Active Directory](../reports-monitoring/concept-provisioning-logs.md).

---

### <a name="public-preview---assign-cloud-groups-to-azure-ad-custom-roles-and-admin-unit-scoped-roles"></a>Versión preliminar pública: asignación de grupos en la nube a roles con ámbito de unidad de administración y roles personalizados de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
Los clientes pueden asignar un grupo en la nube a un rol con ámbito de unidad de administración o a roles personalizados de Azure AD. Para obtener información sobre cómo usar esta característica, vea [Uso de grupos en la nube para administrar asignaciones de roles en Azure Active Directory](../roles/groups-concept.md).

---

### <a name="general-availability---azure-ad-connect-cloud-sync-previously-known-as-cloud-provisioning"></a>Disponibilidad general: sincronización en la nube de Azure AD Connect (conocida anteriormente como aprovisionamiento en la nube)

**Tipo:** Nueva característica  
**Categoría del servicio:** sincronización en la nube de Azure AD Connect  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
La sincronización en la nube de Azure AD Connect ahora está disponible con carácter general para todos los clientes.

La nube de Azure AD Connect transfiere el trabajo pesado de la lógica de transformación a la nube, lo que reduce la superficie local. Además, hay disponibles varias implementaciones de agentes ligeros para una mayor disponibilidad de la sincronización. [Más información](https://aka.ms/cloudsyncGA).
 
---
### <a name="general-availability---attack-simulation-administrator-and-attack-payload-author-built-in-roles"></a>Disponibilidad general: roles integrados Administrador de simulación de ataque y Autor de carga de ataque

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
Hay disponibles dos nuevos roles en el control de acceso basado en rol para asignar a los usuarios, que son Administrador de simulación de ataque y Autor de carga de ataque. 

Los usuarios del rol [Administrador de simulación de ataque](../roles/permissions-reference.md#attack-simulation-administrator) tienen acceso a todas las simulaciones del inquilino y pueden hacer lo siguiente:
- Crear y administrar todos los aspectos de la creación de simulaciones de ataque
- Iniciar y programar una simulación
-  Revisar los resultados de la simulación 

Los usuarios del rol [Autor de carga de ataque](../roles/permissions-reference.md#attack-payload-author) pueden crear cargas de ataque, pero no iniciarlas ni programarlas. Las cargas de ataque están disponibles para todos los administradores del inquilino, que pueden usarlas para crear una simulación.

---

### <a name="general-availability---usage-summary-reports-reader-built-in-role"></a>Disponibilidad general: rol integrado Lector de informes de resumen de uso

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso
 
Los usuarios con el rol Lector de informes de resumen de uso pueden acceder a los datos agregados de nivel de inquilino y a la información asociada del Centro de administración de Microsoft 365 para obtener la puntuación de uso y productividad. Sin embargo, no pueden acceder a los detalles ni a la información de nivel de usuario. 

En el Centro de administración de Microsoft 365 de los dos informes, se diferencia entre los datos agregados de nivel de inquilino y los detalles de nivel de usuario. Este rol agrega una capa adicional de protección a los datos de identificación de usuarios individuales. [Más información](../roles/permissions-reference.md#usage-summary-reports-reader).

---

### <a name="general-availability---require-app-protection-policy-grant-in-azure-ad-conditional-access"></a>Disponibilidad general: concesión Requerir la directiva de protección de aplicaciones en el acceso condicional de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Acceso condicional  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
La concesión de acceso condicional de Azure AD para "Requerir la directiva de protección de aplicaciones" ahora se encuentra en disponibilidad general. 

La directiva ofrece las siguientes funcionalidades:
- Permite el acceso solo cuando se usa una aplicación móvil que admite Intune App Protection.
- Permite el acceso solo cuando un usuario tiene una directiva de protección de aplicaciones de Intune que se entrega a la aplicación móvil.

Obtenga más información [aquí](../conditional-access/app-protection-based-conditional-access.md) sobre cómo configurar una directiva de acceso condicional para la protección de aplicaciones.
 
---

### <a name="general-availability---email-one-time-passcode"></a>Disponibilidad general: código de acceso de un solo uso de correo electrónico

**Tipo:** Nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 
El código de acceso de un solo uso de correo electrónico permite a las organizaciones de todo el mundo colaborar con cualquier persona mediante el envío de un vínculo o una invitación por correo electrónico. Los usuarios invitados pueden comprobar su identidad con el código de acceso de un solo uso enviado a su correo electrónico para acceder a los recursos de su asociado. [Más información](../external-identities/one-time-passcode.md). 
 
---

 ### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2021"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD (enero de 2021)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:
- [Fortes Change Cloud](../saas-apps/fortes-change-cloud-provisioning-tutorial.md)
- [Gtmhub](../saas-apps/gtmhub-provisioning-tutorial.md)
- [monday.com](../saas-apps/mondaycom-provisioning-tutorial.md)
- [Splashtop](../saas-apps/splashtop-provisioning-tutorial.md)
- [Templafy OpenID Connect](../saas-apps/templafy-openid-connect-provisioning-tutorial.md)
- [WEDO](../saas-apps/wedo-provisioning-tutorial.md)

Para más información, vea [Qué es el aprovisionamiento automatizado de usuarios de aplicaciones SaaS en Azure AD](../app-provisioning/user-provisioning.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---january-2021"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD, enero de 2021

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En enero de 2021, agregamos 29 aplicaciones nuevas a la galería de aplicaciones con compatibilidad de federación:

[mySCView](https://dev.myscview.com/), [Talentech](https://talentech.com/contact/), [Bipsync](https://www.bipsync.com/), [OroTimesheet](https://app.orotimesheet.com/login.php), [Mio](https://app.m.io/auth/install/microsoft?scopetype=hub), [Sovelto Easy](https://login.soveltoeasy.fi/), [Supportbench](https://account.supportbench.net/agent/login/),[Bienvenue Formation](https://formation.bienvenue.pro/login), [AIDA Healthcare SSO](https://aidaforparents.com/login/organizations), [International SOS Assistance Products](../saas-apps/international-sos-assistance-products-tutorial.md), [NAVEX One](../saas-apps/navex-one-tutorial.md), [LabLog](../saas-apps/lablog-tutorial.md), [Oktopost SAML](../saas-apps/oktopost-saml-tutorial.md), [EPHOTO DAM](../saas-apps/ephoto-dam-tutorial.md), [Notion](../saas-apps/notion-tutorial.md), [Syndio](../saas-apps/syndio-tutorial.md), [Yello Enterprise](../saas-apps/yello-enterprise-tutorial.md), [Timeclock 365 SAML](../saas-apps/timeclock-365-saml-tutorial.md), [Nalco E-data](https://www.ecolab.com/), [Vacancy Filler](https://app.vacancy-filler.co.uk/VFMVC/Account/Login), [Synerise AI Growth Ecosystem](../saas-apps/synerise-ai-growth-ecosystem-tutorial.md), [Imperva Data Security](../saas-apps/imperva-data-security-tutorial.md), [Illusive Networks](../saas-apps/illusive-networks-tutorial.md), [Proware](../saas-apps/proware-tutorial.md), [Splan Visitor](../saas-apps/splan-visitor-tutorial.md), [Aruba User Experience Insight](../saas-apps/aruba-user-experience-insight-tutorial.md), [Contentsquare SSO](../saas-apps/contentsquare-sso-tutorial.md), [Perimeter 81](../saas-apps/perimeter-81-tutorial.md), [Burp Suite Enterprise Edition](https://docs.microsoft.com/azure/active-directory/saas-apps/burp-suite-enterprise-edition-tutorial)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí: https://aka.ms/AzureADAppRequest. 

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Versión preliminar pública: el administrador de segundo nivel se puede establecer como aprobador alternativo

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 
Ahora, en la administración de derechos, se encuentra disponible una opción adicional al seleccionar aprobadores. Si selecciona "Administrador como aprobador" para el primer aprobador, tendrá otra opción disponible para elegir en el campo de aprobador alternativo, "Administrador de segundo nivel como aprobador alternativo". Si selecciona esta opción, debe agregar un aprobador de reserva al que reenviar la solicitud en caso de que el sistema no encuentre el administrador de segundo nivel. [Más información](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)
 
---

### <a name="general-availability---navigate-to-teams-directly-from-my-access-portal"></a>Disponibilidad general: navegación a Teams directamente desde el portal Mi acceso

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos
 
Ahora puede iniciar Teams directamente desde el portal Mi acceso. 

Para ello, inicie sesión en Mi acceso (https://myaccess.microsoft.com/) ), diríjase a "Paquetes de acceso" y, a continuación, vaya a la pestaña "Activos" para ver todos los paquetes de acceso a los que ya tiene acceso. Al expandir el paquete de acceso seleccionado y mantener el ratón sobre Teams, puede iniciarla al hacer clic en el botón "Abrir". [Más información](../governance/entitlement-management-request-access.md).
 
---

### <a name="improved-logging--end-user-prompts-for-risky-guest-users"></a>Mensajes de registro y usuario final mejorados para los usuarios invitados de riesgo

**Tipo:** Característica modificada  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 

Se han actualizado los mensajes de registro y usuario final para los usuarios invitados de riesgo. Más información en [Protección de identidades y usuarios de Colaboración B2B](../identity-protection/concept-identity-protection-b2b.md).
 
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
- [PaperCut Cloud Print Management](../saas-apps/papercut-cloud-print-management-provisioning-tutorial.md)
- [Parsable](../saas-apps/parsable-provisioning-tutorial.md)
- [Shopify Plus](../saas-apps/shopify-plus-provisioning-tutorial.md)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---december-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: diciembre de 2020

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En diciembre de 2020 se han agregado las siguientes 18 aplicaciones nuevas a la galería de aplicaciones con compatibilidad con la federación:

[AwareGo](../saas-apps/awarego-tutorial.md), [HowNow SSO](https://gethownow.com/), [ZyLAB ONE Legal Hold](https://www.zylab.com/en/product/legal-hold), [Guider](http://www.guider-ai.com/), [Softcrisis](https://www.softcrisis.se/sv/), [Pims 365](http://www.omega365.com/pims), [InformaCast](../saas-apps/informacast-tutorial.md), [RetrieverMediaDatabase](../saas-apps/retrievermediadatabase-tutorial.md), [vonage](../saas-apps/vonage-tutorial.md), [Count Me In - Operations Dashboard](../saas-apps/count-me-in-operations-dashboard-tutorial.md), [ProProfs Knowledge Base](../saas-apps/proprofs-knowledge-base-tutorial.md), [RightCrowd Workforce Management](../saas-apps/rightcrowd-workforce-management-tutorial.md), [JLL TRIRIGA](../saas-apps/jll-tririga-tutorial.md), [Shutterstock](../saas-apps/shutterstock-tutorial.md), [FortiWeb Web Application Firewall](../saas-apps/linkedin-talent-solutions-tutorial.md), [LinkedIn Talent Solutions](../saas-apps/linkedin-talent-solutions-tutorial.md), [Equinix Federation App](../saas-apps/equinix-federation-app-tutorial.md) y [KFAdvance](../saas-apps/kfadvance-tutorial.md)

La documentación de todas las aplicaciones está disponible en https://aka.ms/AppsTutorial.

Para mostrar su aplicación en la galería de aplicaciones de Azure AD, lea los detalles aquí https://aka.ms/AzureADAppRequest.

---

### <a name="navigate-to-teams-directly-from-my-access-portal"></a>Navegación a Teams directamente desde el portal Mi acceso

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso a usuarios **Funcionalidad del producto:** Administración de derechos

Ahora puede iniciar Teams directamente desde el portal Mi acceso. Para ello, inicie sesión en [Mi acceso](https://myaccess.microsoft.com/), diríjase a **Paquetes de acceso** y, a continuación, vaya a la pestaña **Activos** para ver todos los paquetes de acceso a los que ya tiene acceso. Al expandir el paquete de acceso y mantener el ratón sobre Teams, puede iniciarla al hacer clic en el botón **Abrir**. 

Para obtener más información sobre el uso del portal Mi acceso, visite [Solicitud de acceso a un paquete de acceso en la administración de derechos de Azure AD](../governance/entitlement-management-request-access.md#sign-in-to-the-my-access-portal).

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Versión preliminar pública: el administrador de segundo nivel se puede establecer como aprobador alternativo

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de acceso de usuarios  
**Funcionalidad del producto:** Administración de derechos

Ahora está disponible una opción adicional en el proceso de aprobación de la administración de derechos. Si selecciona Administrador como aprobador para el primer aprobador, tendrá otra opción disponible para elegir en el campo de aprobador alternativo, Administrador de segundo nivel como aprobador alternativo. Cuando selecciona esta opción, debe agregar un aprobador de reserva al que reenviar la solicitud en caso de que el sistema no encuentre el administrador de segundo nivel.

Para obtener más información, diríjase a [Cambio de la configuración de aprobación para un paquete de acceso de administración de derechos de Azure AD](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers).

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
 
 [Los roles de RBAC personalizados para la administración delegada de aplicaciones empresariales](../roles/custom-available-permissions.md) ahora están en versión preliminar pública. Estos nuevos permisos se basan en los roles personalizados para la administración del registro de aplicaciones, lo que permite un control exhaustivo sobre el acceso que tienen los administradores. Con el tiempo, se publicarán permisos adicionales para la administración delegada de Azure AD.

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

### <a name="public-preview---azure-ad-application-proxy-natively-supports-single-sign-on-access-to-applications-that-use-headers-for-authentication"></a>Versión preliminar pública: Application Proxy de Azure AD admite de manera nativa el acceso de inicio sesión único a las aplicaciones que usan encabezados para la autenticación

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

Para más información, consulte [Qué es el aprovisionamiento automatizado de usuarios de aplicaciones SaaS en Azure AD](../app-provisioning/user-provisioning.md).
 
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

Consulte [Tipo de recurso bitlockerRecoveryKey](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) para conocer las actualizaciones de la documentación para reflejar estos cambios.

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
 


