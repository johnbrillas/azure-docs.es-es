---
title: Creación de un plan de seguridad para el acceso externo a Azure Active Directory
description: Elabore un plan la seguridad para el acceso externo a los recursos de su organización.
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
ms.openlocfilehash: fd04d24e0e46b053381858be037a009b1987f8e9
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97932527"
---
# <a name="3-create-a-security-plan-for-external-access"></a>3. Creación de un plan de seguridad para el acceso externo 

Ahora que ha [determinado la posición de seguridad deseada para el acceso externo](1-secure-access-posture.md) y [ha descubierto su estado de colaboración actual](2-secure-access-current-state.md), puede crear un plan de gobernanza y seguridad para usuarios externos. 

Este plan debe documentar lo siguiente:

* Las aplicaciones y otros recursos que se deben agrupar para el acceso.

* Las condiciones de inicio de sesión adecuadas para los usuarios externos. Por ejemplo, el estado del dispositivo, la ubicación de inicio de sesión, los requisitos de las aplicaciones cliente y los riesgos para el usuario.

* Directivas empresariales sobre cuándo revisar y retirar el acceso. 

* Las poblaciones de usuarios que se van a agrupar y tratar de manera parecida.

Una vez que se han documentado estas áreas, puede usar directivas de administración de identidades y acceso de Microsoft o cualquier otro proveedor de identidades (IdP) para implementar este plan.

## <a name="document-resources-to-be-grouped-for-access"></a>Documentación de los recursos que se van a agrupar para el acceso

Hay varias maneras de agrupar los recursos para el acceso. 

* Microsoft Teams agrupa los archivos, los hilos de conversación y otros recursos en un solo lugar. Deberá formular una estrategia de acceso externo para Microsoft Teams. Consulte [Acceso seguro a Teams, OneDrive y SharePoint](9-secure-access-teams-sharepoint.md).

* Los paquetes de acceso de administración de derechos le permiten crear un único paquete de aplicaciones y otros recursos a los que puede conceder acceso. 

* Se pueden aplicar directivas de acceso condicional hasta a 250 aplicaciones con los mismos requisitos de acceso.

Con independencia de cómo administre el acceso, debe documentar qué aplicaciones deben agruparse juntas. Tenga en cuenta lo siguiente:

* **Perfil de riesgo**. ¿Cuál es el riesgo para su negocio si un actor malintencionado obtuviera acceso a una aplicación? Considere la posibilidad de codificar cada aplicación con un riesgo alto, medio o bajo. Tenga cuidado al agrupar las aplicaciones de alto riesgo con las de bajo riesgo. 

   * Documente también las aplicaciones que nunca se deben compartir con usuarios externos.

* **Marcos de cumplimiento**. ¿Qué ocurre si una aplicación debe satisfacer algún marco de cumplimiento? ¿Cuáles son los requisitos de acceso y revisión?

* **Aplicaciones para roles de trabajo o departamentos específicos**. ¿Hay aplicaciones que se deban agrupar porque todos los usuarios de un rol de trabajo o departamento concretos van a necesitar acceso?

* **Aplicaciones centradas en la colaboración**. ¿A qué aplicaciones centradas en la colaboración deben poder acceder los usuarios externos? Es posible que sea necesario tener acceso a Microsoft Teams y SharePoint. En el caso de aplicaciones de productividad dentro de Office 365, como Word y Excel, ¿los usuarios externos traerán sus propias licencias, o bien habrá que concederles licencias y proporcionarles acceso?

Para cada agrupación de aplicaciones y recursos que quiera que esté accesible para los usuarios externos, documente lo siguiente:

* Un nombre descriptivo para el grupo, por ejemplo *Finanzas_Acceso_Externo_Alto_Riesgo*. 

* Lista completa de todas las aplicaciones y recursos del grupo.

* Propietarios de aplicaciones y recursos e información de contacto.

* Si el acceso está controlado por TI o se ha delegado en el propietario de la empresa.

* Los requisitos previos del acceso, por ejemplo, la realización de una comprobación en segundo plano o un entrenamiento.

* Cualquier requisito de cumplimiento para el acceso a los recursos.

* Cualquier otro reto adicional, por ejemplo, exigir la autenticación multifactor con recursos específicos.

* La frecuencia con que se revisará el acceso, quién lo hará y dónde se documentará.

Este tipo de plan de gobernanza también se puede y debería elaborarse para el acceso interno.

## <a name="document-sign-in-conditions-for-external-users"></a>Documentación de las condiciones de inicio de sesión para los usuarios externos

Como parte del plan, debe determinar los requisitos de inicio de sesión de los usuarios externos cuando accedan a los recursos. Estos requisitos suelen basarse en el perfil de riesgo de los recursos y en la evaluación de los riesgos del inicio de sesión de los usuarios.

Las condiciones de inicio de sesión se configuran en [Acceso condicional de Azure AD](../conditional-access/overview.md) y constan de una condición y un resultado. Por ejemplo, cuándo exigir la autenticación multifactor.

**Condiciones de inicio de sesión basadas en el riesgo para los recursos.**

| Perfil de riesgo de aplicación| Para desencadenar la autenticación multifactor, tenga en cuenta estas directivas |
| - |-|
| Riesgo bajo| Exigir MFA para conjuntos de aplicaciones específicos |
| Riesgo medio| Exigir MFA cuando existan otros riesgos |
| Riesgo alto| Exigir MFA siempre para usuarios externos |


En la actualidad, puede [exigir la autenticación multifactor para los usuarios de B2B del inquilino](https://docs.microsoft.com/azure/active-directory/external-identities/b2b-tutorial-require-mfa). 

**Condiciones de inicio de sesión basadas en el usuario y en el dispositivo**.

| Riesgo para el usuario o el inicio de sesión| Tenga en cuenta estas directivas |
| - | - |
| Dispositivo| Requerir dispositivos compatibles |
| Aplicaciones móviles| Exigir aplicaciones aprobadas |
| Identity Protection muestra un riesgo alto| Exigir que el usuario cambie la contraseña |
| Ubicación de red| Exigir el inicio de sesión con un intervalo de direcciones IP específico para proyectos muy confidenciales |

En la actualidad, para usar el estado del dispositivo como entrada para una directiva, el dispositivo debe estar registrado o unido al inquilino. 

Se pueden usar [directivas basadas en riesgos de Identity Protection](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk). Sin embargo, los problemas se deben mitigar en el inquilino de inicio del usuario.

En las [ubicaciones de red](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-location), puede restringir el acceso a cualquier intervalo de direcciones IP que posea. Puede hacer esto si solo quiere que los asociados externos accedan a una aplicación mientras están en el sitio de su organización.

[Más información sobre las directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).

## <a name="document-access-review-policies"></a>Documentación de las directivas de revisión de acceso

Documente las directivas empresariales para cuando necesite revisar el acceso a los recursos y cuando necesite retirar el acceso a la cuenta para los usuarios externos. Algunas de estas decisiones podrían incluir:

* Requisitos detallados de cualquier marco de cumplimiento

* Directivas y procesos empresariales internos

* Comportamiento del usuario

A pesar de que las directivas estarán adaptadas en gran medida a sus necesidades, tenga en cuenta lo siguiente:

* **Revisiones de acceso de administración de derechos**. Use la funcionalidad de administración de derechos para:

   * [Hacer caducar automáticamente los paquetes de acceso](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-lifecycle-policy) y, por tanto, el acceso de los usuarios externos a los recursos incluidos.

   * Establecer una [frecuencia de revisión requerida](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-reviews-create) para las revisiones de acceso.

   * Si va a usar [organizaciones conectadas](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-organization) para agrupar a todos los usuarios de un solo asociado, programe revisiones periódicas con el propietario de la empresa y el representante asociado.

* **Grupos de Microsoft 365**. Establezca una [directiva de expiración del grupo](https://docs.microsoft.com/microsoft-365/solutions/microsoft-365-groups-expiration-policy?view=o365-worldwide) para los grupos de Microsoft 365 a los que se invita a los usuarios externos. 

* **Otras opciones**. Si los usuarios externos tienen acceso fuera de los paquetes de acceso de administración de derechos o de los grupos de Microsoft 365, configure el proceso de negocio para revisar cuándo se deben inactivar o eliminar las cuentas. Por ejemplo:

   * Retire la capacidad de inicio de sesión de cualquier cuenta en la que no se haya iniciado sesión durante 90 días.

   * Evalúe las necesidades de acceso y tome medidas al final de cada proyecto con los usuarios externos.

 

## <a name="determine-your-access-control-methods"></a>Determinación de los métodos de control de acceso

Ahora que sabe sobre qué quiere aplicar el control de acceso, cómo se deben agrupar esos recursos para el acceso común y las directivas necesarias de inicio de sesión y revisión de acceso, puede decidir cómo llevar a cabo el plan. 

Algunas funciones, por ejemplo, la [administración de derechos](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview), solo están disponibles con licencias de Azure AD Premium 2 (P2). Las licencias de Microsoft 365 E5 y Office 365 E5 incluyen licencias de Azure AD P2. 

Otras combinaciones de Microsoft 365, Office 365 y Azure AD también permiten alguna funcionalidad para administrar los usuarios externos. Para más información, consulte [Protección de la información](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-365-security-compliance-licensing-guidance).

> [!NOTE]
> Las licencias son por usuario. Por lo tanto, puede tener usuarios específicos, como administradores y propietarios de empresa con control de acceso delegado en el nivel Azure AD P2 o Microsoft 365 E5 sin habilitar dichas licencias para todos los usuarios. Los primeros 50 000 usuarios externos son gratuitos. Si no habilita las licencias P2 para los otros usuarios internos, no podrán usar la funcionalidad de administración de derechos, como los paquetes de acceso. 


## <a name="govern-access-with-azure-ad-p2-and-microsoft--office-365-e5"></a>Gobernanza del acceso con Azure AD P2 y Microsoft/Office 365 E5
Azure AD P2 y Microsoft 365 E5 poseen el conjunto completo de herramientas de seguridad y gobernanza.

### <a name="provisioning-signing-in-reviewing-access-and-deprovisioning-bolded-entries-are-preferred-methods"></a>Aprovisionamiento, inicio de sesión, revisión del acceso y desaprovisionamiento. Las entradas en negrita son los métodos preferidos

| Característica| Aprovisionamiento de usuarios externos| Exigir requisitos de inicio de sesión| Revisar acceso| Desaprovisionar el acceso |
| - | - | - | - | - |
| Colaboración B2B de Azure AD| Invitación por correo electrónico, OTP, autoservicio| | **Revisión periódica por asociado**| Quitar cuenta<br>Restringir el inicio de sesión |
| Administración de derechos| **Agregar usuarios mediante asignación o acceso de autoservicio**| | Revisiones de acceso|**Expiración o eliminación de un paquete de acceso**|
| Office 365 Groups| | | Revisar las pertenencias a grupos| Expiración o eliminación de grupos<br> Eliminación del grupo |
| Grupos de seguridad de Azure AD| | **Directivas de acceso condicional** (agregar usuarios externos a grupos de seguridad según sea necesario)| |  |



 ### <a name="access-to-resources-bolded-entries-are-preferred-methods"></a>Acceso a los recursos. Las entradas en negrita son los métodos preferidos

|Característica | Acceso a aplicaciones y recursos| Acceso a SharePoint y OneDrive| Acceso a Teams| Seguridad de documentos y correo electrónico |
| - |-|-|-|-|
| Administración de derechos| **Agregar usuarios mediante asignación o acceso de autoservicio**| **Paquetes de acceso**| **Paquetes de acceso**|  |
| Grupo de Office 365| | Acceso a los sitios (y al contenido asociado) incluidos con el grupo| Acceso a los equipos (y al contenido asociado) incluidos con el grupo|  |
| Etiquetas de confidencialidad| | **Clasificar y restringir el acceso de forma manual y automática**| **Clasificar y restringir el acceso de forma manual y automática**| **Clasificar y restringir el acceso de forma manual y automática** |
| Grupos de seguridad de Azure AD| **Directivas de acceso condicional de acceso no incluidas en los paquetes de acceso**| | |  |


### <a name="entitlement-management"></a>Administración de derechos 

Los [paquetes de acceso de administración de derechos](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-create) permiten el aprovisionamiento y el desaprovisionamiento del acceso a grupos y equipos, aplicaciones y sitios de SharePoint. Puede definir a qué organizaciones conectadas se les permite el acceso, si se permiten solicitudes de autoservicio y qué flujos de trabajo de aprobación son necesarios (si los hay) para conceder el acceso. Para garantizar que el acceso no se mantenga más tiempo del necesario, puede definir directivas de expiración y revisiones de acceso para cada paquete de acceso. 

 

## <a name="govern-access-with-azure-ad-p1-and-microsoft--office-365-e3"></a>Regulación del acceso con Azure AD P1 y Microsoft/Office 365 E3
Puede lograr una gobernanza sólida con Azure AD P1 y Microsoft 365 E3.

### <a name="provisioning-signing-in-reviewing-access-and-deprovisioning"></a>Aprovisionamiento, inicio de sesión, revisión de el acceso y desaprovisionamiento


|Característica | Aprovisionamiento de usuarios externos| Exigir requisitos de inicio de sesión| Revisar acceso| Desaprovisionar el acceso |
| - |-|-|-|-|
| Colaboración B2B de Azure AD| **Invitación por correo electrónico, OTP, autoservicio**| Federación de B2B directa| **Revisión periódica por asociado**| Quitar cuenta<br>Restringir el inicio de sesión |
| Grupos de Microsoft u Office 365| | | | Expiración o eliminación de grupos.<br>Eliminación del grupo. |
| Grupos de seguridad| | **Agregar usuarios externos a grupos de seguridad (organización, equipo, proyecto, etc.)**| |  |
| Directivas de acceso condicional| | **Directivas de acceso condicional de inicio de sesión para usuarios externos**| |  |


 ### <a name="access-to-resources"></a>Acceso a los recursos.

|Característica | Acceso a aplicaciones y recursos| Acceso a SharePoint y OneDrive| Acceso a Teams| Seguridad de documentos y correo electrónico |
| - |-|-|-|-|
| Grupos de Microsoft u Office 365| | **Acceso a los sitios incluidos con el grupo (y al contenido asociado)**|**Acceso a los equipos incluidos con el grupo de Microsoft 365 (y al contenido asociado)**|  |
| Etiquetas de confidencialidad| | Clasificar y restringir el acceso manualmente| Clasificar y restringir el acceso manualmente| Clasificar manualmente para restringir y cifrar |
| Directivas de acceso condicional| Directivas de acceso condicional del control de acceso| | |  |
| Otros métodos| | Restrinja el acceso al sitio de SharePoint de forma pormenorizada con grupos de seguridad.<br>No permita el uso compartido directo.| **Restringir las invitaciones externas desde equipos**|  |


### <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre cómo proteger el acceso externo a los recursos. Se recomienda realizar las acciones en el orden indicado.

1. [Determinación de la posición de seguridad para el acceso externo](1-secure-access-posture.md)

2. [Detección del estado actual](2-secure-access-current-state.md)

3. [Creación de un plan de gobernanza](3-secure-access-plan.md) (usted está aquí).

4. [Uso de grupos con fines de seguridad](4-secure-access-groups.md)

5. [Transición a B2B de Azure AD](5-secure-access-b2b.md)

6. [Acceso seguro mediante la administración de derechos](6-secure-access-entitlement-managment.md)

7. [Acceso seguro mediante directivas de acceso condicional](7-secure-access-conditional-access.md)

8. [Acceso seguro mediante etiquetas de confidencialidad](8-secure-access-sensitivity-labels.md)

9. [Acceso seguro a Microsoft Teams, OneDrive y SharePoint](9-secure-access-teams-sharepoint.md)