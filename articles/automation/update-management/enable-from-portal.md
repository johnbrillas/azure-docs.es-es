---
title: Habilitación de Update Management de Azure Automation desde Azure Portal
description: En este artículo se describe cómo habilitar Update Management desde Azure Portal.
services: automation
ms.subservice: update-management
ms.date: 01/07/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 089c5fea6ac4a6fc4fb25af2d631335ef51cf4cc
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99054913"
---
# <a name="enable-update-management-from-the-azure-portal"></a>Habilitación de Update Management desde Azure Portal

En este artículo se describe cómo se puede habilitar la característica [Update Management](overview.md) en máquinas virtuales examinando Azure Portal. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual de Azure existente mediante Update Management.

El número de grupos de recursos que puede usar para administrar las máquinas virtuales está limitado por los [límites de implementación de Resource Manager](../../azure-resource-manager/templates/deploy-to-resource-group.md). Las implementaciones de Resource Manager, que no se deben confundir con las implementaciones de actualización, están limitadas a cinco grupos de recursos por implementación. Dos de esos grupos de recursos están reservados para configurar el área de trabajo de Log Analytics, la cuenta de Automation y los recursos relacionados. Esto le deja con tres grupos de recursos que seleccionar para su administración mediante Update Management. Este límite solo se aplica a la instalación simultánea, no al número de grupos de recursos que una característica de Automation puede administrar.

> [!NOTE]
> Al habilitar Update Management, solo se admiten determinadas regiones para vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](../automation-security-overview.md) para administrar máquinas.
* Una [máquina virtual](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

1. En Azure Portal, vaya a **Máquinas virtuales**.

2. En la página **Máquinas virtuales**, use las casillas para elegir las máquinas virtuales que se van a agregar a Update Management. Puede agregar máquinas para un máximo de tres grupos de recursos distintos a la vez. Las máquinas virtuales de Azure pueden existir en cualquier región independientemente de la ubicación de la cuenta de Automation.

    ![Lista de máquinas virtuales](media/enable-from-portal/vmlist.png)

    > [!TIP]
    > Use los controles de filtro para seleccionar máquinas virtuales de distintas suscripciones, ubicaciones y grupos de recursos. Puede hacer clic en la casilla superior para seleccionar todas las máquinas virtuales de una lista.

    [ ![Habilitar Update Management](./media/enable-from-portal/onboard-feature.png)](./media/enable-from-portal/onboard-feature-expanded.png#lightbox)

3. Seleccione **Servicios** y **Update Management** para obtener la característica Update Management.

4. La lista de máquinas virtuales se filtra para mostrar solo las que se encuentran en la misma suscripción y ubicación. Si las máquinas virtuales están en más de tres grupos de recursos, se seleccionan los tres primeros.

5. De forma predeterminada, se seleccionan un área de trabajo de Log Analytics y una cuenta de Automation existentes. Si quiere usar un área de trabajo de Log Analytics y una cuenta de Automation diferentes, seleccione **PERSONALIZADO** para seleccionarlas en la página Configuración personalizada. Al elegir un área de trabajo de Log Analytics, se realiza una comprobación para determinar si está vinculada a una cuenta de Automation. Si se encuentra una cuenta de Automation vinculada, verá la siguiente pantalla. Cuando haya terminado, seleccione **Aceptar**.

    [ ![Seleccionar área de trabajo y cuenta](./media/enable-from-portal/select-workspace-and-account.png)](./media/enable-from-portal/select-workspace-and-account-expanded.png#lightbox)

6. Si el área de trabajo seleccionada no está vinculada a una cuenta de Automation, verá la siguiente pantalla. Seleccione una cuenta de Automation y haga clic en **Aceptar** cuando haya terminado.

    ![No hay área de trabajo](media/enable-from-portal/no-workspace.png)

7. Anule la selección de las casillas que haya junto a las máquinas virtuales que no quiera habilitar. Ya se ha anulado la selección de las máquinas virtuales que no se puedan habilitar.

8. Seleccione **Habilitar** para habilitar la característica. Una vez que haya habilitado Update Management, puede que tarde unos 15 minutos en poder ver la evaluación de la actualización desde la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

* Para usar Update Management para máquinas virtuales, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales](manage-updates-for-vm.md).
* Para solucionar los errores de Update Management generales, consulte [Solución de problemas de Update Management](../troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, consulte [Solución de problemas del agente de actualización de Windows](../troubleshoot/update-agent-issues.md).
* Para solucionar problemas relativos al agente de actualización de Linux, vea [Solución de problemas del agente de actualización de Linux](../troubleshoot/update-agent-issues-linux.md).