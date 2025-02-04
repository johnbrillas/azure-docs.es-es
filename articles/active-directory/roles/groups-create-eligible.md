---
title: Creación de un grupo para asignar roles en Azure Active Directory | Microsoft Docs
description: Aprenda a crear un grupo al que se pueden asignar roles en Azure AD. Administre roles de Azure en Azure Portal, PowerShell o Graph API.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bf0a9ca193be9fd61d0b284338c0f581c9f91e3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103012061"
---
# <a name="create-a-role-assignable-group-in-azure-active-directory"></a>Creación de un grupo al que se pueden asignar roles en Azure Active Directory

Solo puede asignar un rol a un grupo que se ha creado con la propiedad "isAssignableToRole" establecida en True o que se ha creado en el portal de Azure AD con la opción **Los roles de Azure AD se pueden asignar a un grupo** activada. Este atributo de grupo convierte el grupo en uno de los que se puede asignar a un rol en Azure Active Directory (Azure AD). En este artículo se describe cómo crear este tipo especial de grupo. **Nota:** Un grupo con la propiedad isAssignableToRole establecida en true no puede ser de tipo de pertenencia dinámica. Para más información, consulte [Uso de grupos para administrar asignaciones de roles en Azure AD](groups-concept.md).

## <a name="using-azure-ad-admin-center"></a>Mediante el centro de administración de Azure AD

1. Inicie sesión en el[centro de administración de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione **Grupos** > **Todos los grupos** > **Nuevo grupo**.

    [![Apertura de Azure Active Directory para crear un nuevo grupo.](./media/groups-create-eligible/new-group.png "Apertura de Azure Active Directory para crear un nuevo grupo.")](./media/groups-create-eligible/new-group.png#<lightbox>)

1. En la pestaña **Nuevo grupo**, proporcione el tipo, el nombre y la descripción del grupo.
1. Active **Los roles de Azure AD se pueden asignar a un grupo**. Este modificador solo es visible para los administradores de roles con privilegios y los administradores globales, ya que estos son los dos únicos roles que pueden establecer el modificador.

    [![Hacer que el nuevo grupo sea válido para la asignación de roles](./media/groups-create-eligible/eligible-switch.png "Hacer que el nuevo grupo sea válido para la asignación de roles")](./media/groups-create-eligible/eligible-switch.png#<lightbox>)

1. Seleccione los miembros y propietarios del grupo. También tiene la opción de asignar roles al grupo, pero no es necesario asignar un rol aquí.

    [![Adición de miembros al grupo al que se pueden asignar roles y asignación de roles.](./media/groups-create-eligible/specify-members.png "Adición de miembros al grupo al que se pueden asignar roles y asignación de roles.")](./media/groups-create-eligible/specify-members.png#<lightbox>)

1. Una vez especificados los miembros y los propietarios, seleccione **Crear**.

    [![El botón Crear se encuentra en la parte inferior de la página.](./media/groups-create-eligible/create-button.png "El botón Crear se encuentra en la parte inferior de la página.")](./media/groups-create-eligible/create-button.png#<lightbox>)

El grupo se crea con los roles que le haya asignado.

## <a name="using-powershell"></a>Usar PowerShell

### <a name="install-the-azure-ad-preview-module"></a>Instalación del módulo en versión preliminar de Azure AD

```powershell
Install-Module -Name AzureADPreview
Import-Module -Name AzureADPreview
```

Para comprobar que el módulo está listo para su uso, ejecute el siguiente comando:

```powershell
Get-Module -Name AzureADPreview
```

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Creación de un grupo que se puede asignar a un rol

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

Para este tipo de grupo, `isPublic` siempre será False e `isSecurityEnabled` siempre será True.

### <a name="copy-one-groups-users-and-service-principals-into-a-role-assignable-group"></a>Copia de los usuarios y las entidades de servicio de un grupo a un grupo al que se pueden asignar roles

```powershell
#Basic set up
Install-Module -Name AzureADPreview
Import-Module -Name AzureADPreview
Get-Module -Name AzureADPreview

#Connect to Azure AD. Sign in as Privileged Role Administrator or Global Administrator. Only these two roles can create a role-assignable group.
Connect-AzureAD

#Input variabled: Existing group
$idOfExistingGroup = "14044411-d170-4cb0-99db-263ca3740a0c"

#Input variables: New role-assignable group
$groupName = "Contoso_Bellevue_Admins"
$groupDescription = "This group is assigned to Helpdesk Administrator built-in role in Azure AD."
$mailNickname = "contosobellevueadmins"

#Create new security group which is a role assignable group. For creating a Microsoft 365 group, set GroupTypes="Unified" and MailEnabled=$true
$roleAssignablegroup = New-AzureADMSGroup -DisplayName $groupName -Description $groupDescription -MailEnabled $false -MailNickname $mailNickname -SecurityEnabled $true -IsAssignableToRole $true

#Get details of existing group
$existingGroup = Get-AzureADMSGroup -Id $idOfExistingGroup
$membersOfExistingGroup = Get-AzureADGroupMember -ObjectId $existingGroup.Id

#Copy users and service principals from existing group to new group
foreach($member in $membersOfExistingGroup){
if($member.ObjectType -eq 'User' -or $member.ObjectType -eq 'ServicePrincipal'){
Add-AzureADGroupMember -ObjectId $roleAssignablegroup.Id -RefObjectId $member.ObjectId
}
}
```

## <a name="using-microsoft-graph-api"></a>Uso de Microsoft Graph API

### <a name="create-a-role-assignable-group-in-azure-ad"></a>Creación de un grupo al que se pueden asignar roles en Azure AD

```http
POST https://graph.microsoft.com/beta/groups
{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

Para este tipo de grupo, `isPublic` siempre será False e `isSecurityEnabled` siempre será True.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un rol a un grupo en la nube](groups-assign-role.md)
- [Uso de grupos en la nube para administrar asignaciones de roles](groups-concept.md)
- [Solución de problemas de roles asignados a grupos en la nube](groups-faq-troubleshooting.md)
