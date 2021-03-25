---
title: Eliminación de una cuenta de ejecución de Azure Automation
description: En este artículo se describe cómo eliminar una cuenta de ejecución con PowerShell o desde Azure Portal.
services: automation
ms.subservice: process-automation
ms.date: 01/06/2021
ms.topic: conceptual
ms.openlocfilehash: 6924a9a9394d237b08db878ef910de6767ca9b39
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99056896"
---
# <a name="delete-an-azure-automation-run-as-account"></a>Eliminación de una cuenta de ejecución de Azure Automation

Las cuentas de ejecución de Azure Automation proporcionan autenticación para administrar recursos en el modelo de implementación clásico de Azure o Azure Resource Manager mediante runbooks de Automation y otras características de Automation. En este artículo se describe cómo eliminar una cuenta de ejecución o una cuenta de ejecución clásica. Al realizar esta acción, la cuenta de Automation se conserva. Después de eliminar la cuenta de ejecución, puede volver a crearla en Azure Portal o con el script de PowerShell proporcionado.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Eliminación de una cuenta de ejecución o de ejecución clásica

1. Abra la cuenta de Automation en Azure Portal.

2. En el panel izquierdo, seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

3. En la página de propiedades Cuentas de ejecución, seleccione la cuenta de ejecución o la cuenta de ejecución clásica que quiere eliminar.

4. En el panel Propiedades de la cuenta seleccionada, haga clic en **Eliminar**.

   ![Eliminación de una cuenta de ejecución](media/delete-run-as-account/automation-account-delete-run-as.png)

5. Mientras se está eliminando la cuenta, puede seguir el progreso desde el menú, en **Notificaciones**.

## <a name="next-steps"></a>Pasos siguientes

Para recrear la cuenta de ejecución o de ejecución clásica, consulte [Creación de cuentas de ejecución](create-run-as-account.md).