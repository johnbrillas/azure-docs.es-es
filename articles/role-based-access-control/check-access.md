---
title: 'Inicio rápido: Comprobación del acceso de un usuario a los recursos de Azure (Azure RBAC)'
description: En este inicio rápido, aprenderá a comprobar su acceso, o el de otro usuario a los recursos de Azure mediante Azure Portal y el control de acceso basado en rol (RBAC) de Azure.
services: role-based-access-control
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: quickstart
ms.workload: identity
ms.date: 12/09/2020
ms.author: rolyon
ms.custom: contperf-fy21q2
ms.openlocfilehash: 5e4f3314ba580dddbd995855bc0f0512b7597107
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115753"
---
# <a name="quickstart-check-access-for-a-user-to-azure-resources"></a>Inicio rápido: Comprobación del acceso de un usuario a los recursos de Azure

A veces, necesita comprobar qué acceso tiene un usuario a un conjunto de recursos de Azure. Para realizar dicha comprobación, debe enumerar sus asignaciones. Una forma rápida de comprobar el acceso de un usuario es utilizar la característica **Comprobar acceso** de la página **Control de acceso (IAM)** .

## <a name="step-1-open-the-azure-resources"></a>Paso 1: Apertura de los recursos de Azure

Para comprobar el acceso de un usuario, primero debe abrir los recursos de Azure cuyo acceso desee comprobar. Los recursos de Azure se organizan en niveles, que normalmente se denominan el *ámbito*. En Azure, puede especificar un ámbito a cuatro niveles, de ancho a estrecho: grupo de administración, suscripción, grupo de recursos y recurso.

![Niveles de ámbito de los nombres de Azure RBAC](../../includes/role-based-access-control/media/scope-levels.png)

Siga estos pasos para abrir el conjunto de recursos de Azure para el que desea comprobar el acceso.

1. Abra [Azure Portal](https://portal.azure.com).

1. Abra el conjunto de recursos de Azure, como **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso concreto.

1. Haga clic en el recurso específico de ese ámbito.

    A continuación se muestra un grupo de recursos de ejemplo.

    ![Información general del grupo de recursos](./media/shared/rg-overview.png)

## <a name="step-2-check-access-for-a-user"></a>Paso 2: Comprobación de acceso de un usuario

Siga estos pasos para comprobar el acceso de un solo usuario, grupo, entidad de servicio o identidad administrada a los recursos de Azure seleccionados anteriormente.

1. Haga clic en **Control de acceso (IAM).**

    A continuación se muestra un ejemplo de la página Control de acceso (IAM) de un grupo de recursos.

    ![Control de acceso del grupo de recursos: pestaña Comprobar acceso](./media/shared/rg-access-control.png)

1. En la pestaña **Comprobar acceso** de la lista **Buscar**, seleccione el usuario, el grupo, la entidad de servicio o la identidad administrada para los que desea comprobar el acceso.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/shared/rg-check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    En este panel, puede ver el acceso para la entidad de seguridad seleccionada en este ámbito y heredada a este ámbito. No se muestran las asignaciones en ámbitos secundarios. Aparecerán las siguientes asignaciones:

    - Asignaciones de roles agregadas con Azure RBAC.
    - Asignaciones de denegación agregadas mediante Azure Blueprints o aplicaciones administradas de Azure.
    - Asignaciones de coadministradores y administradores de servicios clásicos para implementaciones clásicas. 

    ![Panel de asignaciones de roles y denegaciones para un usuario](./media/shared/rg-check-access-assignments-user.png)

## <a name="step-3-check-your-access"></a>Paso 3: Compruebe el acceso

Siga estos pasos para comprobar el acceso a los recursos de Azure seleccionados anteriormente.

1. Haga clic en **Control de acceso (IAM).**

1. En la pestaña **Comprobar acceso**, haga clic en el botón **View my access** (Ver mi acceso).

    Aparece un panel de asignaciones que enumera el acceso a este ámbito y heredado en este ámbito. No se muestran las asignaciones en ámbitos secundarios.

    ![Panel de asignaciones de roles y denegaciones](./media/check-access/rg-check-access-assignments.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Enumeración de asignaciones de roles de Azure mediante Azure Portal](role-assignments-list-portal.md)
