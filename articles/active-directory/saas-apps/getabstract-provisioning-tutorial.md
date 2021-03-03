---
title: 'Tutorial: Configuración de getAbstract para el aprovisionamiento automático de usuarios mediante Azure Active Directory | Microsoft Docs'
description: Aprenda a aprovisionar y desaprovisionar automáticamente las cuentas de usuario de Azure AD en getabstract.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: bd8898f9-7a01-4e85-9dd4-61ae4b01ab5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2021
ms.author: Zhchia
ms.openlocfilehash: 25253e9a302a34fb473da63ad4cad562d6302a8a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651756"
---
# <a name="tutorial-configure-getabstract-for-automatic-user-provisioning"></a>Tutorial: Configuración de getAbstract para el aprovisionamiento automático de usuarios

En este tutorial, se describen los pasos que debe realizar en getAbstract y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se ha configurado, Azure AD aprovisiona y desaprovisiona usuarios y grupos de forma automática en [getAbstract](https://www.getabstract.com) mediante el servicio de aprovisionamiento de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en getAbstract.
> * Eliminar usuarios en getAbstract cuando ya no necesitan acceso.
> * Mantener los atributos de usuario sincronizados entre Azure AD y getAbstract.
> * Aprovisionar grupos y pertenencias a grupos en getAbstract.
> * [Inicio de sesión único](getabstract-tutorial.md) en getAbstract (recomendado)

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md) 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Un inquilino de getAbstract (licencia corporativa de getAbstract).
* SSO habilitado en el inquilino de Azure AD y el inquilino de getAbstract.
* Aprobación y habilitación de SCIM para getAbstract (enviar correo electrónico a b2b.itsupport@getabstract.com).

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos se van a [asignar entre Azure AD y getAbstract](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-getabstract-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de getAbstract para admitir el aprovisionamiento con Azure AD
1. Inicie sesión en getAbstract.
2. Haga clic en el icono de configuración que se encuentra en la esquina superior derecha y haga clic en la opción **My Central Admin** (Mi administración central).
 
    ![My Central Admin de getAbstract](media/getabstract-provisioning-tutorial/my-account.png)

3. Busque y haga clic en la opción **SCIM Admin** (Administración de SCIM).
 
    ![Administrador de SCIM de getAbstract](media/getabstract-provisioning-tutorial/scim-admin.png) 

4. Haga clic en el botón **Ir**. 

    ![Id. de cliente de SCIM de getAbstract](media/getabstract-provisioning-tutorial/scim-client-go.png)

5. Haga clic en el botón **Generar nuevo token**.

    ![Token 1 de SCIM de getAbstract](media/getabstract-provisioning-tutorial/scim-generate-token-step-2.png)

6. Si está seguro, haga clic en el botón **Generar nuevo token**. En caso contrario, haga clic en el botón **Cancelar**.

    ![Token 2 de SCIM de getAbstract](media/getabstract-provisioning-tutorial/scim-generate-token-step-1.png)

7. Por último, puede hacer clic en el icono Copiar al Portapapeles o seleccionar el token completo y copiarlo. Tenga también en cuenta que la dirección URL base o del inquilino es `https://www.getabstract.com/api/scim/v2`. Estos valores se escriben en los campos **Token secreto*** y **URL de inquilino*** de la pestaña Aprovisionamiento de la aplicación getAbstract en Azure Portal.

    ![Token 3 de SCIM de getAbstract](media/getabstract-provisioning-tutorial/scim-generate-token-step-3.png)


## <a name="step-3-add-getabstract-from-the-azure-ad-application-gallery"></a>Paso 3. Adición de getAbstract desde la galería de aplicaciones de Azure AD

Agregue getAbstract desde la galería de aplicaciones de Azure AD para empezar a administrar el aprovisionamiento en getAbstract. Si ya ha configurado getAbstract para el inicio de sesión único anteriormente, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a getAbstract, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-getabstract"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en getAbstract 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en TestApp en función de las asignaciones de grupos o usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-getabstract-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para getAbstract en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **getAbstract**.

    ![Vínculo a getAbstract en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento automático](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, especifique la URL del inquilino y el token secreto de getAbstract. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a getAbstract. Si la conexión no funciona, asegúrese de que la cuenta de getAbstract tiene permisos de administrador y pruebe de nuevo.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to getAbstract** (Sincronizar usuarios de Azure Active Directory con getAbstract).

9. Revise los atributos de usuario que se sincronizan entre Azure AD y getAbstract en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de getAbstract en operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de getAbstract admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|Compatible con el filtrado|
   |---|---|---|
   |userName|String|&check;|
   |active|Boolean|
   |emails[type eq "work"].value|String|
   |name.givenName|String|
   |name.familyName|String|
   |externalId|String|
   |preferredLanguage|String|

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to getAbstract** (Sincronizar grupos de Azure Active Directory con getAbstract).

11. Revise los atributos de grupo que se sincronizan entre Azure AD y getAbstract en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer coincidencias con los grupos de getAbstract con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    |Atributo|Tipo|Compatible con el filtrado|
    |---|---|---|
    |DisplayName|String|&check;|
    |externalId|String|
    |members|Referencia|
12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para getAbstract, cambie el valor de **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Para definir los usuarios o grupos que desea aprovisionar en getAbstract, seleccione los valores deseados en **Ámbito**, en la sección **Configuración**.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

* Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
* Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
* Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)