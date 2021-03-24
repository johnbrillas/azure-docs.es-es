---
title: 'Asignación de roles de Azure a una identidad administrada (versión preliminar): RBAC de Azure'
description: Para obtener información sobre cómo asignar roles de Azure, empiece con la identidad administrada y seleccione el ámbito y el rol mediante Azure Portal y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 02/15/2021
ms.author: rolyon
ms.openlocfilehash: 57c8c00a64996bc6223fbe7e514db9db38ccdcc2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100556860"
---
# <a name="assign-azure-roles-to-a-managed-identity-preview"></a>Asignación de roles de Azure a una identidad administrada (versión preliminar)

Puede asignar un rol a una identidad administrada mediante la página **Control de acceso (IAM)** como se describe en [Asignación de roles de Azure mediante Azure Portal](role-assignments-portal.md). Cuando se usa la página Control de acceso (IAM), se empieza con el ámbito y, a continuación, se selecciona la identidad administrada y el rol. En este artículo se describe una manera alternativa de asignar roles para una identidad administrada. Con estos pasos, empieza con la identidad administrada y luego selecciona el ámbito y el rol.

> [!IMPORTANT]
> La asignación de un rol a una identidad administrada mediante estos pasos alternativos se encuentra actualmente en versión preliminar.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

Siga estos pasos para asignar un rol a una identidad administrada asignada por el sistema a partir de la identidad administrada.

1. En Azure Portal, abra una identidad administrada asignada por el sistema.

1. En el menú izquierdo, haga clic en **Identidad**.

    ![Identidad administrada asignada por el sistema](./media/shared/identity-system-assigned.png)

1. En **Permisos**, haga clic en **Asignaciones de roles de Azure**.

    Si los roles ya están asignados a la identidad administrada asignada por el sistema seleccionada, verá la lista de asignaciones de roles. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/shared/role-assignments-system-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción**.

1. Haga clic en **Agregar asignación de roles (versión preliminar)** .

1. Use las listas desplegables para seleccionar el conjunto de recursos a los que se aplica la asignación de roles, como **Suscripción**, **Grupo de recursos** o recurso.

    Si no tiene permisos de escritura de asignación de roles para el ámbito seleccionado, se mostrará un mensaje alineado. 

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

   ![Panel para agregar la asignación de roles para la identidad administrada asignada por el sistema](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la identidad administrada en el ámbito seleccionado.

## <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

Siga estos pasos para asignar un rol a una identidad administrada asignada por el usuario a partir de la identidad administrada.

1. En Azure Portal, abra una identidad administrada asignada por el usuario.

1. En el menú de la izquierda, haga clic en **Asignaciones de roles de Azure**.

    Si los roles ya están asignados a la identidad administrada asignada por el usuario seleccionada, verá la lista de asignaciones de roles. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el usuario](./media/shared/role-assignments-user-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción**.

1. Haga clic en **Agregar asignación de roles (versión preliminar)** .

1. Use las listas desplegables para seleccionar el conjunto de recursos a los que se aplica la asignación de roles, como **Suscripción**, **Grupo de recursos** o recurso.

    Si no tiene permisos de escritura de asignación de roles para el ámbito seleccionado, se mostrará un mensaje alineado. 

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

   ![Panel para agregar la asignación de roles para la identidad administrada asignada por el usuario](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la identidad administrada en el ámbito seleccionado.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué son las identidades administradas de recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md)
- [Asignación de roles de Azure mediante Azure Portal](role-assignments-portal.md)
- [Enumeración de asignaciones de roles de Azure mediante Azure Portal](role-assignments-list-portal.md)
