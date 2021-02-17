---
title: Asignación de un usuario como administrador de una suscripción de Azure - Azure RBAC
description: Aprenda a convertir a un usuario en administrador de una suscripción de Azure mediante Azure Portal y el control de acceso basado en rol (Azure RBAC).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 01/11/2021
ms.author: rolyon
ms.openlocfilehash: dec5888127ed1fc291bec244a44cfb71e343e3bb
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100556841"
---
# <a name="assign-a-user-as-an-administrator-of-an-azure-subscription"></a>Asignación de un usuario como administrador de una suscripción de Azure

Para que un usuario sea administrador de una suscripción de Azure, asígnele el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. El rol Propietario da al usuario acceso total a todos los recursos de la suscripción, incluido el derecho a conceder acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="step-1-open-the-subscription"></a>Paso 1: Abrir la suscripción

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque las suscripciones en el cuadro de búsqueda.

    ![Buscar grupo de recursos en Azure Portal](./media/shared/sub-portal-search.png)

1. Haga clic en la suscripción que desee usar.

    A continuación se muestra una suscripción de ejemplo.

    ![Información general del grupo de recursos](./media/shared/sub-overview.png)

## <a name="step-2-open-the-add-role-assignment-pane"></a>Paso 2: Abrir el panel Agregar asignación de roles

**Control de acceso (IAM)** es la página que se usa normalmente para asignar roles y conceder acceso a los recursos de Azure. También se conoce como administración de identidad y acceso (IAM) y aparece en varias ubicaciones de Azure Portal.

1. Haga clic en **Control de acceso (IAM)**.

    A continuación se muestra un ejemplo de la página Control de acceso (IAM) de una suscripción.

    ![Página Control de acceso (IAM) para un grupo de recursos](./media/shared/sub-access-control.png)

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar** > **Agregar asignación de roles**.
   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar asignación de roles](./media/shared/add-role-assignment-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/shared/add-role-assignment.png)

## <a name="step-3-select-the-owner-role"></a>Paso 3: Seleccionar el rol Propietario

El rol [Propietario](built-in-roles.md#owner) permite conceder acceso total para administrar todos los recursos, incluida la posibilidad de asignar roles en Azure RBAC. Debe tener un máximo de 3 propietarios de suscripción para reducir el riesgo de vulneración por parte de un propietario en peligro.

- En la lista **Rol**, seleccione el rol **Propietario**.

   ![Seleccione el rol Propietario en el panel Agregar asignación de roles](./media/role-assignments-portal-subscription-admin/add-role-assignment-role-owner.png)

## <a name="step-4-select-who-needs-access"></a>Paso 4: Determinar quién necesita acceso

1. En la lista **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**.

1. En la sección **Seleccionar**, busque el usuario escribiendo una cadena o desplazándose por la lista.

   ![Seleccione el usuario en Agregar asignación de roles](./media/role-assignments-portal-subscription-admin/add-role-assignment-user-admin.png)

1. Una vez que haya encontrado el usuario, haga clic en él para seleccionarlo.

## <a name="step-5-assign-role"></a>Paso 5: Asignación de un rol

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol al usuario en el ámbito seleccionado.

1. En la pestaña **Asignaciones de roles**, compruebe que ve la asignación del rol en la lista.

    ![Agregar asignación de roles guardado](./media/role-assignments-portal-subscription-admin/sub-role-assignments-owner.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure mediante Azure Portal](role-assignments-portal.md)
- [Enumeración de asignaciones de roles de Azure mediante Azure Portal](role-assignments-list-portal.md)
- [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)
