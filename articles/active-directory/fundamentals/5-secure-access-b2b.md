---
title: Transición a la colaboración controlada con Colaboración B2B de Azure Active Directory
description: Realice la transición a la colaboración controlada con Colaboración B2B de Azure AD.
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
ms.openlocfilehash: 4206ba7617032e34310682d1468e6b1b661b8c8a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101648594"
---
# <a name="transition-to-governed-collaboration-with-azure-active-directory-b2b-collaboration"></a>Transición a la colaboración controlada con Colaboración B2B de Azure Active Directory 

Tener la colaboración bajo control es clave para proteger el acceso externo a los recursos. Antes de continuar con este artículo, asegúrese de que:

* [Ha determinado su posición de seguridad](1-secure-access-posture.md)

* [Ha detectado su estado actual](2-secure-access-current-state.md)

* [Ha creado un plan de seguridad](3-secure-access-plan.md)

* [Ha comprendido cómo funcionan conjuntamente los grupos y la seguridad](4-secure-access-groups.md)

Una vez que hecho todo esto, estará listo realizar la transición a la colaboración controlada. Este artículo le ayudará a trasladar toda la colaboración externa a la [Colaboración B2B de Azure Active Directory](../external-identities/what-is-b2b.md) (B2B de Azure AD). B2B de Azure AD es una característica de [Azure AD External Identities](../external-identities/compare-with-b2c.md).

## <a name="control-who-your-organization-collaborates-with"></a>Control de las personas que colaboran con su organización

Debe decidir si desea limitar las organizaciones con las que los usuarios pueden colaborar y qué usuarios de su organización pueden iniciar la colaboración. La mayoría de las organizaciones adoptan el enfoque de permitir que las unidades de negocio decidan con quién colaboran y delegar la aprobación y la supervisión según sea necesario. Por ejemplo, algunas organizaciones gubernamentales, educativas y de servicios financieros no permiten la colaboración abierta. Puede que desee usar las características de Azure AD para establecer el ámbito de la colaboración, como se describe en el resto de esta sección.

### <a name="determine-collaboration-partners"></a>Determinación de asociados de colaboración

En primer lugar, asegúrese de que ha documentado las organizaciones con las que está colaborando actualmente y los dominios de los usuarios de esas organizaciones. Un asociado de colaboración puede tener varios dominios. Por ejemplo, un asociado puede tener varias unidades de negocio con dominios independientes.

A continuación, determine si desea habilitar la colaboración futura con 

* cualquier dominio (más inclusivo)

* todos los dominios excepto los denegados explícitamente 

* solo dominios específicos (más restrictivo)

> [!NOTE]
> Cuanto más restrictiva sea la configuración de colaboración, más probable es que los usuarios vayan fuera del marco de colaboración aprobado. Recomendamos habilitar la colaboración más amplia que permitan sus necesidades de seguridad y revisar estrechamente esa colaboración en lugar de ser demasiado restrictivo. 

Además, tenga en cuenta que la limitación a un solo dominio puede impedir accidentalmente la colaboración autorizada con las organizaciones, que tienen otros dominios no relacionados para sus usuarios. Por ejemplo, si hace negocios con una organización Contoso, el punto inicial de contacto con Contoso podría ser uno de los empleados con sede en Estados Unidos que tenga un correo electrónico con un dominio ".com". Sin embargo, si solo permite el dominio ".com", puede omitir accidentalmente los empleados canadienses que tengan el dominio ".ca". 

Hay circunstancias en las que desearía permitir solo asociados de colaboración concretos. Por ejemplo, un sistema de universidad solo desea permitir el acceso de su propio profesorado a un inquilino de recursos. O un conglomerado de empresas puede que solo desee permitir que determinadas subsidiarias colaboren entre sí para lograr el cumplimiento con un marco requerido.

#### <a name="using-allow-and-deny-lists"></a>Uso de las listas de permitidos y denegados

Puede usar una lista de permitidos o denegados para [restringir las invitaciones a usuarios B2B](../external-identities/allow-deny-list.md) procedentes de determinadas organizaciones. Solo se puede usar una lista de permitidos o una lista de denegados, no ambas.

* Una [lista de permitidos](../external-identities/allow-deny-list.md) limita la colaboración solo a los dominios enumerados; todos los demás dominios se encuentran efectivamente en la lista de denegados.

* Una [lista de denegados](../external-identities/allow-deny-list.md) permite la colaboración con cualquier dominio que no esté en la lista de denegados.

> [!IMPORTANT]
> Estas listas no se aplican a los usuarios que ya están en el directorio. Tampoco se aplican a listas de permitidos y denegados de OneDrive para la Empresa y SharePoint, que son independientes.

Algunas organizaciones usan una lista de dominios de "actor incorrecto" conocidos que el proveedor de seguridad administrada proporciona para la lista de denegados. Por ejemplo, si la organización lleva a cabo negocios legítimamente con Contoso y usa un dominio .com, puede que haya una organización no relacionada que haya estado usando el dominio .org de Contoso y que intente un ataque de suplantación de identidad (phishing) para suplantar a los empleados de Contoso. 

## <a name="control-how-external-users-gain-access"></a>Control del modo de acceso de los usuarios externos

Hay muchas maneras de colaborar con asociados externos mediante B2B de Azure AD. Para comenzar la colaboración, usted invita o permite a su asociado acceder a sus recursos. Los usuarios pueden acceder respondiendo para:

* Canjear [una invitación enviada a través de un correo electrónico](../external-identities/redemption-experience.md) o [un vínculo directo para compartir](../external-identities/redemption-experience.md) un recurso.

* Solicitar acceso [a través de una aplicación](../external-identities/self-service-sign-up-overview.md) que usted crea

* Solicitar acceso a través del portal [Mi acceso](../governance/entitlement-management-request-access.md)

Al habilitar B2B de Azure AD, habilita la capacidad de invitar a usuarios invitados a través de vínculos directos e invitaciones por correo electrónico de forma predeterminada. Las invitaciones a través de correo electrónico OTP y un portal de autoservicio se encuentran actualmente en versión preliminar y deben estar habilitadas en External Identities | Configuración de colaboración externa en el portal de Azure AD.

### <a name="control-who-can-invite-guest-users"></a>Control de quién puede invitar a usuarios invitados

Determine quién puede invitar a los usuarios invitados a acceder a los recursos.

* La configuración más restrictiva consiste en permitir que solo los administradores y los usuarios a los que se les haya concedido el [rol Invitador de usuarios invitados](../external-identities/delegate-invitations.md) inviten a los invitados.

* Si los requisitos de seguridad lo permiten, se recomienda permitir que todos los usuarios con un tipo de usuario de miembro inviten a los invitados.

* Determine si desea que los usuarios con un tipo de usuario de invitado, que es el tipo de cuenta predeterminado para usuarios de B2B de Azure AD, puedan invitar a otros invitados. 

![Captura de pantalla de la configuración de invitación de invitados.](media/secure-external-access/5-guest-invite-settings.png)

 

### <a name="collect-additional-information-about-external-users"></a>Recopilación de información adicional sobre usuarios externos

Si usa la administración de derechos de Azure AD, puede configurar preguntas para que los usuarios externos respondan. A continuación, se mostrarán a las preguntas a los aprobadores para ayudarles a tomar una decisión. Puede configurar diferentes conjuntos de preguntas para cada [directiva de paquete de acceso](../governance/entitlement-management-access-package-approval-policy.md) de forma que los aprobadores puedan tener información pertinente para el acceso que están aprobando. Por ejemplo, si un paquete de acceso está pensado para el acceso del proveedor, se le puede pedir al solicitante el número de contrato de proveedor. Un paquete de acceso diferente diseñado para los proveedores puede solicitar el país de origen.

Si usa un portal de autoservicio, puede usar [conectores de API](../external-identities/api-connectors-overview.md) para recopilar atributos adicionales sobre los usuarios a medida que se suscriben. Después, puede usar esos atributos para asignar el acceso. Por ejemplo, si durante el proceso de registro recopila su identificador de proveedor, puede usar ese atributo para asignarlo dinámicamente a un grupo o un paquete de acceso para ese proveedor. Puede crear atributos personalizados en Azure Portal y usarlos en los flujos de usuario de registro de autoservicio. También puede leer y escribir estos atributos mediante [Microsoft Graph API](../../active-directory-b2c/microsoft-graph-operations.md). 

### <a name="troubleshoot-invitation-redemption-to-azure-ad-users"></a>Solución de problemas de canje de invitación para usuarios de Azure AD

Hay tres casos en los que los usuarios invitados de un asociado de colaboración que utilizan Azure AD tendrán problemas para canjear una invitación.

* Si usa una lista de permitidos y el dominio del usuario no se incluye en dicha lista.

* Si el inquilino principal del asociado de colaboración tiene restricciones de inquilino que impiden la colaboración con usuarios externos.

* Si el usuario no forma parte del inquilino de Azure AD del asociado. Por ejemplo, si hay usuarios en contoso.com que solo están en Active Directory (u otro servicio IdP local), solo podrán canjear invitaciones a través del proceso de OTP de correo electrónico. Para más información, consulte el [flujo de canje de invitación](../external-identities/redemption-experience.md).

## <a name="control-what-external-users-can-access"></a>Control de los recursos a los que pueden acceder los usuarios externos

La mayoría de las organizaciones no son monolíticas. Es decir, hay algunos recursos que se pueden compartir sin problemas con usuarios externos y otros a los que no querrá que los usuarios externos accedan. Por lo tanto, debe controlar a qué acceden los usuarios externos. Considere la posibilidad de usar la [administración de derechos y los paquetes de acceso para controlar el acceso](6-secure-access-entitlement-managment.md) a recursos específicos.

De forma predeterminada, los usuarios invitados pueden ver información y atributos sobre los miembros de inquilino y otros asociados, incluidas las pertenencias a grupos. Considere si sus requisitos de seguridad exigen limitar el acceso de los usuarios externos a esta información.

![Captura de pantalla de la configuración de la colaboración externa.](media/secure-external-access/5-external-collaboration-settings.png)

Se recomiendan las siguientes restricciones para los usuarios invitados.

* **Limitar el acceso de invitado a los grupos de exploración y otras propiedades del directorio**

   * Use la configuración de colaboración externa para restringir la capacidad de invitado para leer los grupos de los que no son miembros.

* **Bloquear el acceso a aplicaciones solo para empleados**.

   * Cree una directiva de acceso condicional para bloquear el acceso a las aplicaciones integradas en Azure AD que solo son adecuadas para los usuarios que no son invitados. 

* **Bloquear el acceso a Azure Portal. Puede hacer excepciones raras, pero necesarias**. 

   * Cree una directiva de acceso condicional que incluya a todos los usuarios invitados y externos y luego [implemente una directiva para bloquear el acceso](../../role-based-access-control/conditional-access-azure-management.md).

 

## <a name="remove-users-who-no-longer-need-access"></a>Eliminación de usuarios que ya no necesitan acceso

Evalúe el acceso actual para que pueda [revisar y quitar usuarios que ya no necesitan acceso](../governance/access-reviews-external-users.md). Incluya usuarios externos en el inquilino como invitados y aquellos con cuentas de miembro. 

Algunas organizaciones agregaron usuarios externos como proveedores, asociados y contratistas como miembros. Estos miembros pueden tener un atributo específico, o nombres de usuario que comienzan por, por ejemplo,

* p- para proveedores

* a- para asociados

* c- para contratistas

Evalúe cualquier usuario externo con cuentas de miembro para determinar si sigue necesitando acceso. En caso afirmativo, realice la transición de estos usuarios a B2B de Azure AD tal y como se describe en la sección siguiente.

También puede tener usuarios invitados que no fueron invitados a través de la Administración de derechos o B2B de Azure AD.

Para encontrar estos usuarios, puede:

* [Buscar usuarios invitados que no han sido invitados mediante la Administración de derechos](../governance/access-reviews-external-users.md).

   * Proporcionamos un [script de PowerShell de EJEMPLO.](https://github.com/microsoft/access-reviews-samples/tree/master/ExternalIdentityUse)

Realice la transición de estos usuarios a usuarios de B2B de Azure AD tal y como se describe en la sección siguiente.

## <a name="transition-your-current-external-users-to-b2b"></a>Transición de los usuarios externos actuales a B2B

Si no ha usado B2B de Azure AD, es probable que tenga usuarios no empleados en el inquilino. Se recomienda realizar la transición de estas cuentas a cuentas de usuario externas de B2B de Azure AD y luego cambiar el tipo de usuario a invitado. Esto le permite aprovechar las numerosas maneras en que Azure AD y Microsoft 365 le permiten tratar a los usuarios externos de forma diferente. Algunas de estas maneras son:

* Incluir fácilmente a los usuarios invitados en directivas de acceso condicional o excluirlos de estas

* Incluir fácilmente a los usuarios invitados en los paquetes de acceso y las revisiones de acceso o excluirlos de estos

* Incluir fácilmente el acceso externo a Teams, SharePoint y a otros recursos o excluir dicho acceso de estos.

Para realizar la transición de estos usuarios internos manteniendo al mismo tiempo su acceso actual, UPN y pertenencias a grupos, consulte [Invitar a usuarios internos a la colaboración B2B](../external-identities/invite-internal-users.md).

## <a name="decommission-undesired-collaboration-methods"></a>Retirada de métodos de colaboración no deseados

Para completar la transición a la colaboración controlada, debe retirar los métodos de colaboración no deseados. La retirada se basa en el grado de control que desea que TI ejerza sobre la colaboración y su posición de seguridad. Para información sobre TI y el control de usuario final, consulte [Determinación de la posición de seguridad para el acceso externo](1-secure-access-posture.md).

A continuación se indican los vehículos de colaboración que puede querer evaluar.

### <a name="direct-invitation-through-microsoft-teams"></a>Invitación directa a través de Microsoft Teams

De forma predeterminada, Teams permite el acceso externo, lo que significa que la organización puede comunicarse con todos los dominios externos. Si desea restringir o permitir dominios específicos solo para Teams, puede hacerlo en el [portal de administración de Teams](https://admin.teams.microsoft.com/company-wide-settings/external-communications). 


### <a name="direct-sharing-through-sharepoint-and-onedrive"></a>Uso compartido directo a través de SharePoint y OneDrive

El uso compartido directo a través de SharePoint y OneDrive puede agregar usuarios fuera del proceso de Administración de derechos. Para obtener una visión detallada de estas configuraciones, vea [Administración del acceso con Microsoft Teams, SharePoint y OneDrive para la Empresa](9-secure-access-teams-sharepoint.md). Si lo desea, también puede [bloquear el uso de OneDrive personal del usuario](/office365/troubleshoot/group-policy/block-onedrive-use-from-office).

### <a name="sending-documents-through-email"></a>Envío de documentos por correo electrónico

Los usuarios enviarán documentos por correo electrónico a usuarios externos. Tenga en cuenta cómo desea restringir y cifrar el acceso a estos documentos mediante el uso de etiquetas de confidencialidad. Para más información, consulte la información sobre la administración del acceso con etiquetas de confidencialidad.

### <a name="unsanctioned-collaboration-tools"></a>Herramientas de colaboración no autorizadas

El panorama de las herramientas de colaboración es vasto. Es probable que los usuarios usen muchas herramientas fuera de sus obligaciones oficiales, incluidas plataformas como Google Docs, DropBox, Slack o Zoom. Es posible bloquear el uso de estas herramientas desde una red corporativa en el nivel de firewall y con la administración de aplicaciones móviles para dispositivos administrados por la organización. Sin embargo, esto también bloqueará las instancias autorizadas de estas plataformas y no bloquearía el acceso desde dispositivos no administrados. Bloquee las plataformas que no desea usar si es necesario y cree directivas comerciales para el uso no autorizado para las plataformas que necesita usar. 

Para más información sobre la administración de aplicaciones no autorizadas, consulte:

* [Control de aplicaciones conectadas](/cloud-app-security/governance-actions)

* [Autorización y no autorización de una aplicación.](/cloud-app-security/governance-discovery)

 
### <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre cómo proteger el acceso externo a los recursos. Se recomienda realizar las acciones en el orden indicado.

1. [Determinación de la posición de seguridad para el acceso externo](1-secure-access-posture.md)

2. [Detección del estado actual](2-secure-access-current-state.md)

3. [Creación de un plan de gobernanza](3-secure-access-plan.md)

4. [Uso de grupos con fines de seguridad](4-secure-access-groups.md)

5. [Transición a B2B de Azure AD](5-secure-access-b2b.md) (usted está aquí).

6. [Acceso seguro mediante la Administración de derechos](6-secure-access-entitlement-managment.md)

7. [Acceso seguro mediante directivas de acceso condicional](7-secure-access-conditional-access.md)

8. [Acceso seguro mediante etiquetas de confidencialidad](8-secure-access-sensitivity-labels.md)

9. [Acceso seguro a Microsoft Teams, OneDrive y SharePoint](9-secure-access-teams-sharepoint.md)