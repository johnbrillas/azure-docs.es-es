---
title: Protección del acceso externo a Microsoft Teams, SharePoint y OneDrive con Azure Active Directory
description: Proteja el acceso a los servicios de Microsoft 365 como parte de la seguridad de acceso externo general.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f373c849cd542769fec7a10f20dad04e742c9591
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102565145"
---
# <a name="secure-external-access-to-microsoft-teams-sharepoint-and-onedrive-for-business"></a>Protección del acceso externo a Microsoft Teams, SharePoint y OneDrive con para la Empresa 

Microsoft Teams, SharePoint y OneDrive para la Empresa son tres de las formas más usadas de colaborar y compartir contenido con usuarios externos. Si los métodos “aprobados” son demasiado restrictivos, los usuarios saldrán de los métodos aprobados enviando contenido por correo electrónico o configurando aplicaciones y procesos externos inseguros, como una instancia de OneDrive o DropBox personal. El objetivo es equilibrar las necesidades de seguridad con la facilidad de colaboración.

En este artículo se explica cómo determinar y configurar la colaboración externa para satisfacer sus objetivos empresariales con Microsoft Teams y SharePoint.

## <a name="governance-begins-in-azure-active-directory"></a>La gobernanza comienza en Azure Active Directory

El uso compartido en Microsoft 365 está en parte gobernado por [External Identities | Configuración de colaboración externa](https://aad.portal.azure.com/) en Azure Active Directory (Azure AD). Si el uso compartido externo está deshabilitado o restringido en Azure AD, invalida todas las configuraciones definidas en Microsoft 365. Una excepción a esta afirmación es que si la integración de B2B de Azure AD no está habilitada, SharePoint y OneDrive se pueden configurar para admitir el uso compartido ad hoc a través de códigos de acceso de un solo uso (OTP).

![Captura de pantalla de la configuración de colaboración externa](media/secure-external-access/9-external-collaboration-settings.png)

### <a name="guest-user-access"></a>Acceso de usuarios invitados

Existen tres opciones para el acceso de los usuarios invitados, que controlan lo que estos pueden ver después de ser invitados. 

Para evitar que los usuarios invitados vean detalles de otros usuarios invitados y puedan enumerar la pertenencia a grupos, elija Los usuarios invitados tienen acceso limitado a las propiedades y pertenencias de los objetos de directorio.

### <a name="guest-invite-settings"></a>Configuración de la invitación de usuarios

Esta configuración determina quién puede invitar a invitados y cómo se puede invitar a esos invitados. Esta configuración solo está habilitada si también lo está la integración con B2B.

Se recomienda permitir que los administradores y usuarios del rol Invitador de usuarios invitados puedan invitar. Esta configuración permite definir procesos de colaboración controlados, como en el ejemplo siguiente.

* El propietario del equipo envía un vale para que se le asigne el rol Invitador de usuarios invitados y

   * Se convierte en el responsable de todas las invitaciones de invitado.

   * Acepta no agregar directamente usuarios a la instancia de SharePoint subyacente.

   * Es responsable de realizar revisiones de acceso periódicas y revocar el acceso según corresponda.

* TI central, hace lo siguiente:

   * Habilita el uso compartido externo mediante la concesión del rol solicitado al finalizar el entrenamiento.

   * Asigna una licencia P2 de Azure AD al propietario del grupo de Microsoft 365 para habilitar las revisiones de acceso.
   * Crea una revisión de acceso al grupo de Microsoft 365.

   * Confirma que se están produciendo revisiones de acceso.

   * Quita los usuarios que se agregan directamente a la instancia de SharePoint subyacente.

 Establezca **Habilitar el código de acceso de un solo uso de correo electrónico para invitados (versión preliminar) y Habilitación del registro de invitados de autoservicio mediante flujos de usuario (versión preliminar)** en **Sí**. Esta configuración aprovecha la integración con la configuración de colaboración externa de Azure AD.

### <a name="collaboration-restrictions"></a>Restricciones de colaboración

Existen tres opciones en las restricciones de colaboración. Los requisitos empresariales determinan cuál de ellas elegir.

* **Permitir que se envíen invitaciones a cualquier dominio** significa que se puede invitar a cualquier usuario a colaborar.

* **Denegar invitaciones a los dominios especificados** significa que cualquier usuario ajeno a esos dominios especificados puede ser invitado para colaborar.

* **Permitir invitaciones solo a los dominios especificados** significa que no se puede invitar a ningún usuario ajeno a esos dominios especificados. 

## <a name="govern-access-in-teams"></a>Control del acceso en Teams

[Teams diferencia entre los usuarios externos (cualquiera que no pertenece a la organización) y los usuarios invitados (aquellos con cuentas de invitado)](/microsoftteams/communicate-with-users-from-other-organizations?WT.mc_id=TeamsAdminCenterCSH%e2%80%8b). Puede administrar la configuración de colaboración en el [portal de configuración de Teams](https://admin.teams.microsoft.com/company-wide-settings/external-communications) en la configuración de toda la organización. 

> [!NOTE]
> La configuración de colaboración de identidades externas en Azure Active Directory controla los permisos efectivos. Puede aumentar las restricciones en Teams, pero no reducirlas de las que se establecen en Azure AD.

* **Configuración del acceso externo**. De forma predeterminada, Teams permite el acceso externo, lo que significa que la organización puede comunicarse con todos los dominios externos. Si desea restringir o permitir dominios específicos solo para Teams, puede hacerlo aquí.

* **Acceso de invitado**. El acceso de invitado controla lo que los usuarios invitados pueden hacer en Teams.

Para más información sobre cómo administrar el acceso externo en Teams, consulte los siguientes recursos.

* [Administración del acceso externo en Microsoft Teams](/microsoftteams/manage-external-access)

* [Modelos de identidad de Microsoft 365 y Azure Active Directory](/microsoft-365/enterprise/about-microsoft-365-identity)

* [Modelos de identidad y autenticación para Microsoft Teams](/MicrosoftTeams/identify-models-authentication)

* [Etiquetas de confidencialidad para Microsoft Teams](/MicrosoftTeams/sensitivity-labels)

## <a name="govern-access-in-sharepoint-and-onedrive"></a>Control del acceso en SharePoint y OneDrive

Los administradores de SharePoint tienen muchas configuraciones disponibles para la colaboración. La configuración de toda la organización se administra desde el centro de administración de SharePoint. La configuración se puede ajustar para cada sitio de SharePoint. Se recomienda que la configuración de toda la organización tenga los niveles de seguridad mínimos necesarios y que aumente la seguridad en sitios específicos según sea necesario. Por ejemplo, para un proyecto de alto riesgo, puede que desee restringir a los usuarios a determinados dominios y deshabilitar la capacidad de los miembros para invitar a los invitados.

### <a name="integrating-sharepoint-and-one-drive-with-azure-ad-b2b"></a>Integración de SharePoint y OneDrive con B2B de Azure AD

Como parte de la estrategia global para controlar la colaboración externa, se recomienda [habilitar la versión preliminar de la integración de SharePoint y OneDrive con B2B de Azure AD](/sharepoint/sharepoint-azureb2b-integration-preview).

B2B de Azure AD proporciona autenticación y administración de usuarios invitados. Con la integración de SharePoint y OneDrive, los [códigos de acceso de un solo uso de B2B de Azure AD](../external-identities/one-time-passcode.md) se usan para el uso compartido externo de archivos, carpetas, elementos de lista, bibliotecas de documentos y sitios. Esta característica proporciona una experiencia actualizada de la [experiencia de protección de destinatario de uso compartido externo](/sharepoint/what-s-new-in-sharing-in-targeted-release) existente.

> [!NOTE]
> Si habilita la versión preliminar para la integración de B2B de Azure AD, el uso compartido de SharePoint y OneDrive estará sujeto a la configuración de relaciones organizativas de Azure AD, como **Los miembros pueden invitar** y **Los invitados pueden invitar**.

### <a name="sharing-policies"></a>Uso compartido de directivas

El *uso compartido externo* puede establecerse tanto para SharePoint como para OneDrive. Las restricciones de OneDrive no pueden ser más permisivas que la configuración de SharePoint.

 ![Captura de pantalla de la configuración de uso compartido externo en SharePoint y OneDrive](media/secure-external-access/9-sharepoint-settings.png)

La integración de SharePoint con B2B de Azure AD cambia el modo en que los controles interactúan con las cuentas.

* **Cualquiera**. No recomendado

   * Independientemente del estado de la integración, la habilitación de vínculos Cualquiera significa que no se aplicará ninguna directiva de Azure cuando se use este tipo de vínculo. 

   * En un escenario de colaboración controlada, no habilite esta funcionalidad. 
   > [!NOTE]
   > Es posible que encuentre un escenario en el que necesite habilitar esta configuración para un sitio específico, en cuyo caso la habilitaría aquí y establecería la restricción mayor en sitios individuales.

* **Invitados nuevos y existentes**. Opción recomendada si tiene habilitada la integración.

   * **Con la integración de B2B de Azure AD** habilitada, los invitados nuevos y existentes tendrán una cuenta de invitado de B2B de Azure AD que se puede administrar con directivas de Azure AD.

   * **Sin la integración de B2B de Azure AD** habilitada, los nuevos invitados no tendrán una cuenta de B2B de Azure AD creada y no se podrán administrar desde Azure AD. El hecho de que los invitados existentes tengan una cuenta de B2B de Azure AD depende de cómo se creó el invitado.

* **Invitados existentes**. Opción recomendada si no tiene habilitada la integración.

   * Con esta opción habilitada, los usuarios solo pueden compartir con otros usuarios que ya estén en el directorio.

* **Solo los miembros de su organización**. No se recomienda cuando necesita colaborar con usuarios externos.

   * Independientemente del estado de la integración, los usuarios solo podrán compartir con los usuarios de su organización. 

* **Limitar el uso compartido externo por dominio**. De forma predeterminada, SharePoint permite el acceso externo, lo que significa que se permite el uso compartido con todos los dominios externos. Si desea restringir o permitir dominios específicos solo para SharePoint, puede hacerlo aquí.

* **Allow only users in specific security groups to share externally** (Permitir que solo los usuarios de grupos de seguridad específicos compartan externamente).  Esta opción de configuración restringe quién puede compartir contenido en SharePoint y OneDrive, mientras que la configuración de Azure AD se aplica a todas las aplicaciones. Restringir quién puede compartir puede ser útil si desea que sus usuarios se formen en el uso compartido seguro y, al finalizar, agregarlos a un grupo de seguridad de uso compartido aprobado. Si se selecciona esta opción de configuración y los usuarios no tienen una manera de acceder a ser un "compartidor aprobado", en su lugar, pueden encontrar formas no aprobadas de compartir. 

* **Permitir a los invitados compartir elementos que no son de su propiedad**. Se recomienda mantener esta opción de configuración deshabilitada.

* **People who use a verification code must reauthenticate after this many days**  (Las personas que usan un código de verificación deben volver a autenticarse después de este número de días) (el valor predeterminado es 30). Se recomienda habilitar esta opción de configuración.

### <a name="access-controls"></a>Controles de acceso

La configuración de los controles de acceso afectará a todos los usuarios de la organización. Dado que es posible que no pueda controlar si los usuarios externos tienen dispositivos compatibles, no abordaremos estos controles aquí. 

* **Cierre de sesión inactiva**. Se recomienda habilitar este control, que le permite advertir a los usuarios y cerrar la sesión de los mismos en dispositivos no administrados después de un período de inactividad. Puede configurar el período de inactividad y la advertencia. 

* **Ubicación de red**. Establecer este control significa que solo puede permitir el acceso procedente de las direcciones IP que posee su organización. En escenarios de colaboración externa, establezca esta opción de configuración solamente si todos los asociados externos van a acceder a los recursos desde dentro de la red o a través de su VPN.

### <a name="file-and-folder-links"></a>Vínculos de archivos y carpetas

En el centro de administración de SharePoint, también puede establecer el modo en que se comparten los vínculos de archivos y carpetas. También puede configurar estas opciones para cada sitio. 

 ![Captura de pantalla de configuración de vínculos de archivos y carpetas](media/secure-external-access/9-file-folder-links.png)

Si ha habilitado la integración con B2B de Azure AD, el uso compartido de archivos y carpetas con los que se encuentran fuera de la organización dará lugar a la creación de un usuario de B2B cuando se compartan los archivos y las carpetas.

Se recomienda establecer el tipo de vínculo predeterminado en **Solo los miembros de su organización** y los permisos predeterminados en **Editar**. De esta forma garantiza que los elementos se compartan con cuidado. Después, puede personalizar esta opción de configuración para los valores predeterminados por sitio que cumplan las necesidades específicas de colaboración.

### <a name="anyone-links"></a>Vínculos Cualquiera

No se recomienda habilitar vínculos Cualquiera. Si lo hace, se recomienda establecer una expiración y considerar la posibilidad de restringirlos para ver los permisos. Si selecciona los permisos Solo vista para archivos o carpetas, los usuarios no podrán cambiar los vínculos Cualquiera para incluir privilegios de edición.

Para obtener más información sobre cómo controlar el acceso externo a SharePoint, vea lo siguiente:

* [Información general sobre el uso compartido externo de SharePoint](/sharepoint/external-sharing-overview)

* [Integración de SharePoint y OneDrive con B2B de Azure AD](/sharepoint/sharepoint-azureb2b-integration-preview)

#### <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre cómo proteger el acceso externo a los recursos. Se recomienda realizar las acciones en el orden indicado.

1. [Determinación de la posición de seguridad para el acceso externo](1-secure-access-posture.md)

2. [Detección del estado actual](2-secure-access-current-state.md)

3. [Creación de un plan de gobernanza](3-secure-access-plan.md)

4. [Uso de grupos con fines de seguridad](4-secure-access-groups.md)

5. [Transición a B2B de Azure AD](5-secure-access-b2b.md)

6. [Acceso seguro mediante la administración de derechos](6-secure-access-entitlement-managment.md)

7. [Acceso seguro mediante directivas de acceso condicional](7-secure-access-conditional-access.md)

8. [Acceso seguro mediante etiquetas de confidencialidad](8-secure-access-sensitivity-labels.md)

9. [Acceso seguro a Microsoft Teams, OneDrive y SharePoint](9-secure-access-teams-sharepoint.md) (usted está aquí).