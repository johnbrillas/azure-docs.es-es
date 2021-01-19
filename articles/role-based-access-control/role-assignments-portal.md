---
title: 'Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante Azure Portal y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 01/11/2021
ms.author: rolyon
ms.openlocfilehash: f1753e7bc50fa9ff2c5512696a37dae7578f23b4
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98117451"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] En este artículo se describe cómo asignar roles con Azure Portal.

Si tiene que asignar roles de Administrador en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC de Azure, para conceder acceso a un recurso de Azure, se agrega una asignación de roles. Siga estos pasos para asignar un rol. Para obtener información general de alto nivel de los pasos, consulte [Pasos para agregar una asignación de roles](role-assignments-steps.md).

### <a name="step-1-identify-the-needed-scope"></a>Paso 1: identificar el ámbito necesario

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)]

[!INCLUDE [Scope for Azure RBAC least privilege](../../includes/role-based-access-control/scope-least.md)] Para obtener más información sobre el ámbito, vea [Comprensión del ámbito](scope-overview.md).

![Niveles de ámbito de los nombres de Azure RBAC](../../includes/role-based-access-control/media/scope-levels.png)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el cuadro de búsqueda de la parte superior, busque el ámbito al que quiere conceder acceso. Por ejemplo, busque **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso específico.

    ![Buscar grupo de recursos en Azure Portal](./media/shared/rg-portal-search.png)

1. Haga clic en el recurso específico de ese ámbito.

    A continuación se muestra un grupo de recursos de ejemplo.

    ![Información general del grupo de recursos](./media/shared/rg-overview.png)

### <a name="step-2-open-the-add-role-assignment-pane"></a>Paso 2: abrir el panel Agregar asignación de roles

**Control de acceso (IAM)** es la página que se usa normalmente para asignar roles y conceder acceso a los recursos de Azure. También se conoce como administración de identidad y acceso (IAM) y aparece en varias ubicaciones de Azure Portal.

1. Haga clic en **Control de acceso (IAM).**

    A continuación se muestra un ejemplo de la página Control de acceso (IAM) de un grupo de recursos.

    ![Página Control de acceso (IAM) para un grupo de recursos](./media/shared/rg-access-control.png)

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar** > **Agregar asignación de roles**.
   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar asignación de roles](./media/shared/add-role-assignment-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/shared/add-role-assignment.png)

### <a name="step-3-select-the-appropriate-role"></a>Paso 3: seleccionar el rol apropiado

1. En la lista **Role**, busque o desplácese para buscar el rol que quiere asignar.

    Para ayudarle a determinar el rol adecuado, puede mantener el puntero sobre el icono de información para mostrar una descripción del rol. Para más información, puede ver el artículo [Roles integrados de Azure](built-in-roles.md).

   ![Seleccionar el rol en Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment-role.png)

1. Haga clic para seleccionar el rol.

### <a name="step-4-select-who-needs-access"></a>Paso 4: elegir quién necesita acceso

1. En la lista **Asignar acceso a** , seleccione el tipo de entidad de seguridad a la que quiere asignar acceso.

    | Tipo | Descripción |
    | --- | --- |
    | **Usuario, grupo o entidad de servicio** | Si quiere asignar el rol a un usuario, grupo o entidad de servicio (aplicación), seleccione este tipo. |
    | **Identidad administrada asignada por el usuario** | Si quiere asignar el rol a una [identidad administrada asignada por el usuario](../active-directory/managed-identities-azure-resources/overview.md), seleccione este tipo. |
    | *Identidad administrada asignada por el sistema* | Si quiere asignar el rol a una [identidad administrada asignada por el sistema](../active-directory/managed-identities-azure-resources/overview.md), seleccione la instancia de servicio de Azure en la que se encuentra la identidad administrada. |

   ![Seleccionar el tipo de entidad de seguridad en Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment-type.png)

1. Si ha seleccionado una identidad administrada asignada por el usuario o una identidad administrada asignada por el sistema, seleccione la **suscripción** en la que se encuentra la identidad administrada.

1. En la sección **Seleccionar**, escriba una cadena o desplácese por la lista para buscar la entidad de seguridad.

   ![Seleccionar el usuario en Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment-user.png)

1. Una vez que haya encontrado la entidad de seguridad, haga clic en esta para seleccionarla.

### <a name="step-5-assign-role"></a>Paso 5: Asignación de un rol

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la entidad de seguridad en el ámbito seleccionado.

1. En la pestaña **Asignaciones de roles**, compruebe que ve la asignación de roles en la lista.

    ![Agregar asignación de roles guardado](./media/role-assignments-portal/rg-role-assignments.png)

## <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol

En RBAC de Azure, para quitar el acceso de un recurso de Azure, se quita una asignación de roles. Siga estos pasos para eliminar una asignación de roles.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere quitar el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. En la lista de asignaciones de roles, agregue una marca de verificación a la entidad de seguridad con la asignación de roles que desee quitar.

   ![Asignación de roles seleccionada que se va a quitar](./media/role-assignments-portal/rg-role-assignments-select.png)

1. Haga clic en **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí**.

    Si ve un mensaje que indica que las asignaciones de roles heredadas no se pueden quitar, significa que intenta quitar una asignación de roles en un ámbito secundario. Debe abrir el control de acceso (IAM) en el ámbito en el que se asignó el rol e intentarlo de nuevo. Una forma rápida de abrir el control de acceso (IAM) en el ámbito correcto es examinar la columna **Ámbito** y hacer clic en el vínculo situado junto a **(Heredado)** .

   ![Quitar el mensaje de asignación de roles de las asignaciones de roles heredadas](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un usuario como administrador de una suscripción de Azure](role-assignments-portal-subscription-admin.md)
- [Adición de una asignación de roles para una identidad administrada](role-assignments-portal-managed-identity.md)
- [Solución de problemas de Azure RBAC](troubleshooting.md)
