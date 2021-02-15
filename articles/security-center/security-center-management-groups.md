---
title: Obtención de visibilidad de todos los inquilinos en Azure Security Center | Microsoft Docs
description: En este artículo se explica cómo administrar la posición de seguridad a escala mediante la aplicación de directivas a todas las suscripciones vinculadas al inquilino de Azure Active Directory.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: 13cbc2e9451221fef951eb6fac4c6b2772275122
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99556430"
---
# <a name="organize-management-groups-subscriptions-and-tenant-wide-visibility"></a>Organización de grupos de administración, suscripciones y visibilidad de todo el inquilino

En este artículo se explica cómo administrar la posición de seguridad de la organización a escala mediante la aplicación de directivas de seguridad a todas las suscripciones vinculadas al inquilino de Azure Active Directory.

Para obtener visibilidad en la posición de seguridad de todas las suscripciones registradas en el inquilino de Azure AD, se necesita asignar un rol de Azure con permisos de lectura suficientes en el grupo de administración raíz.


## <a name="organize-your-subscriptions-into-management-groups"></a>Organización de las suscripciones en grupos de administración

### <a name="introduction-to-management-groups"></a>Introducción a los grupos de administración

Los grupos de administración de Azure permiten administrar de un modo eficaz el acceso, las directivas y los informes en grupos de suscripciones, además de administrar todas las inversiones en Azure mediante acciones en el grupo de administración raíz. Puede organizar las suscripciones en grupos de administración y aplicar las directivas de gobernanza a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las directivas que se aplican al grupo de administración. 

A cada inquilino de Azure AD se le da un grupo de administración de nivel superior único llamado **grupo de administración raíz**. Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo permite que las directivas globales y las asignaciones de roles de Azure se apliquen en el nivel de directorio. 

El grupo de administración raíz se crea de manera automática cuando realiza cualquiera de las siguientes acciones: 
- Abra **Grupos de administración** en [Azure Portal](https://portal.azure.com).
- Cree un grupo de administración mediante una llamada API.
- Crea un grupo de administración con PowerShell. Puede encontrar las instrucciones de PowerShell en [Creación de grupos de administración para la administración de los recursos y la organización](../governance/management-groups/create-management-group-portal.md).

Aunque los grupos de administración no son necesarios para incorporar Security Center, se recomienda crear al menos uno para que se cree el grupo de administración raíz. Una vez creado el grupo, todas las suscripciones del inquilino de Azure AD estarán vinculadas a él. 


Para más información sobre los grupos de administración, consulte el artículo [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md).

### <a name="view-and-create-management-groups-in-the-azure-portal"></a>Visualización y creación de grupos de administración en Azure Portal

1. En [Azure Portal](https://portal.azure.com), use el cuadro de búsqueda de la barra superior para encontrar y abrir **Grupos de administración**.

    :::image type="content" source="./media/security-center-management-groups/open-management-groups-service.png" alt-text="Acceso a los grupos de administración":::

    Aparece la lista de grupos de administración.

1. Para crear un grupo de administración, seleccione **Agregar grupo de administración**, escriba los detalles pertinentes y elija **Guardar**.

    :::image type="content" source="media/security-center-management-groups/add-management-group.png" alt-text="Incorporación de un grupo de administración a Azure":::

    - El **identificador de grupo de administración** es el identificador único de directorio que se usa para enviar comandos en este grupo de administración. Este identificador no es editable una vez creado, dado que se usa en todo el sistema de Azure para identificar este grupo. 
    - El campo de nombre para mostrar es el nombre que se muestra en Azure Portal. Un nombre para mostrar independiente es un campo opcional al crear el grupo de administración y se puede cambiar en cualquier momento.  


### <a name="add-subscriptions-to-a-management-group"></a>Incorporación de suscripciones a un grupo de administración
Puede agregar suscripciones al grupo de administración que ha creado.

1. En **Grupos de administración**, seleccione el grupo de administración de la suscripción.

    :::image type="content" source="./media/security-center-management-groups/management-group-subscriptions.png" alt-text="Selección de un grupo de administración de la suscripción":::

1. Cuando se abra la página del grupo, seleccione **Detalles**.

    :::image type="content" source="./media/security-center-management-groups/management-group-details-page.png" alt-text="Apertura de la página de detalles de un grupo de administración":::

1. En la página de detalles del grupo, seleccione **Agregar suscripción**, seleccione las suscripciones y elija **Guardar**. Repita estos pasos hasta que haya agregado todas las suscripciones del ámbito.

    :::image type="content" source="./media/security-center-management-groups/management-group-add-subscriptions.png" alt-text="Incorporación de una suscripción a un grupo de administración":::
   > [!IMPORTANT]
   > Los grupos de administración pueden contener tanto suscripciones como grupos de administración secundarios. Cuando asigna un rol de Azure a un usuario en el grupo de administración primario, las suscripciones del grupo de administración secundario heredan el acceso. Las directivas establecidas en el grupo de administración primario también son heredadas por los secundarios. 


## <a name="grant-tenant-wide-permissions-to-yourself"></a>Concesión de permisos de todo el inquilino a uno mismo

Un usuario con el rol de **administrador global** de Azure Active Directory (AD) puede tener responsabilidades en todo el inquilino, pero carecer de los permisos de Azure para ver la información de toda la organización en Azure Security Center. La elevación de permisos es necesaria porque las asignaciones de roles de Azure AD no conceden acceso a los recursos de Azure. 

> [!TIP]
> Aprenda más sobre la elevación de permisos del rol de administrador global en [Elevación de los privilegios de acceso para administrar todas las suscripciones y los grupos de administración de Azure](../role-based-access-control/elevate-access-global-admin.md).

Para asignarse a uno mismo permisos de nivel de inquilino:

1. Si su organización administra el acceso a los recursos con [Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-configure.md) o cualquier otra herramienta de PIM, el rol de administrador global debe estar activo para el usuario mediante el procedimiento que se describe a continuación.

1. Como usuario administrador global sin una asignación en el grupo de administración raíz del inquilino, abra la página **Información general** de Security Center y seleccione el vínculo de **visibilidad de todo el inquilino** en el mensaje emergente. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-banner.png" alt-text="Habilitación de los permisos de nivel de inquilino en Azure Security Center":::

1. Selección del nuevo rol de Azure que se va a asignar. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-form.png" alt-text="Formulario para definir los permisos de nivel de inquilino que se asignarán al usuario":::

    > [!TIP]
    > Por lo general, el rol de Administrador de seguridad es necesario para aplicar directivas en el nivel raíz, mientras que el de Lector de seguridad será suficiente para proporcionar visibilidad en el nivel de inquilino. Para más información acerca de los permisos concedidos por estos roles, consulte la [descripción del rol integrado de Administrador de seguridad](../role-based-access-control/built-in-roles.md#security-admin) o la [descripción del rol integrado de Lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader).
    >
    > Para conocer las diferencias entre estos roles específicos de Security Center, consulte la tabla de [Roles y acciones permitidas](security-center-permissions.md#roles-and-allowed-actions).

    Para conseguir la vista de toda la organización, se conceden roles en el nivel de grupo de administración raíz del inquilino.  

1. Cierre sesión en Azure Portal y vuelva a iniciarla.

1. Cuando tenga privilegios de acceso elevados, abra o actualice Azure Security Center para comprobar que tiene visibilidad en todas las suscripciones del inquilino de Azure AD. 


## <a name="request-tenant-wide-permissions-when-yours-are-insufficient"></a>Solicitud de permisos para todo el inquilino cuando el suyo sea insuficiente

Si inicia sesión en Security Center y ve un banner que le indica que la vista está limitada, puede hacer clic para enviar una solicitud al administrador global de su organización. En la solicitud, puede incluir el rol que le gustaría que se le asignara y el administrador global tomará una decisión sobre qué rol otorgar. 

La decisión del administrador global es aceptar o rechazar estas solicitudes. 

> [!IMPORTANT]
> Solamente puede enviar una solicitud cada siete días.

Para solicitar permisos elevados al administrador global:

1. En Azure Portal, abra Azure Security Center.

1. Si ve el banner "Está viendo información limitada"., Selecciónelo.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions.png" alt-text="Banner que informa a un usuario de que puede solicitar permisos para todo el inquilino.":::

1. En el formulario de solicitud detallado, seleccione el rol deseado y la justificación del motivo por el que necesita estos permisos.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions-details.png" alt-text="Página de detalles para solicitar permisos para todo el inquilino al administrador global de Azure":::

1. Seleccione **Solicitar acceso**.

    Se envía un correo electrónico al administrador global. El correo electrónico contiene un vínculo a Security Center donde pueden aprobar o rechazar la solicitud.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions-email.png" alt-text="Correo electrónico al administrador global para los nuevos permisos":::

    Una vez que el administrador global selecciona **Revisar la solicitud** y completa el proceso, la decisión se envía por correo electrónico al usuario que realiza la solicitud. 

## <a name="assign-azure-roles-to-other-users"></a>Asignación de roles de Azure a otros usuarios

### <a name="assign-azure-roles-to-users-through-the-azure-portal"></a>Asignación de roles de Azure a los usuarios mediante Azure Portal: 
1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
1. Para ver los grupos de administración, seleccione **Todos los servicios** en el menú principal de Azure y, a continuación, seleccione **Grupo de administración**.
1.  Seleccione un grupo de administración y elija **Detalles**.

    :::image type="content" source="./media/security-center-management-groups/management-group-details.PNG" alt-text="Captura de pantalla con los detalles de Grupos de administración":::

1. Seleccione **Control de acceso (IAM)** y, luego, **Asignaciones de roles**.
1. Seleccione **Agregar asignación de roles**.
1. Seleccione el rol para asignar y el usuario y, luego, elija **Guardar**.  
   
   ![Captura de pantalla de incorporación del rol de Lector de seguridad](./media/security-center-management-groups/asc-security-reader.png)

### <a name="assign-azure-roles-to-users-with-powershell"></a>Asignación de roles de Azure a los usuarios con PowerShell: 
1. Instale [Azure PowerShell](/powershell/azure/install-az-ps).
2. Ejecute los comandos siguientes: 

    ```azurepowershell
    # Login to Azure as a Global Administrator user
    Connect-AzAccount
    ```

3. Cuando se le solicite, inicie sesión con credenciales de administrador global. 

    ![Captura de pantalla de la solicitud de inicio de sesión](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Conceda permisos de rol de lector mediante la ejecución del comando siguiente:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Para eliminar el rol, use el siguiente comando: 

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

## <a name="remove-elevated-access"></a>Eliminación de privilegios de acceso elevados 

Después de asignar los roles de Azure a los usuarios, el administrador de inquilino debería eliminarse a sí mismo del rol de administrador de acceso de usuarios.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com) o en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com).

2. En la lista de navegación, seleccione **Azure Active Directory** y, luego, **Propiedades**.

3. En **Administración de acceso a recursos de Azure**, establezca el modificador en **No**.

4. Para guardar la configuración, seleccione **Guardar**.



## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a obtener la visibilidad de todos los inquilinos en Azure Security Center. Para obtener información relacionada, consulte:

- [Permisos en Azure Security Center](security-center-permissions.md)