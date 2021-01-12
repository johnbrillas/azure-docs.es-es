---
title: Quitar máquinas virtuales de Azure Automation Update Management
description: En este artículo se explica cómo quitar máquinas administradas con Update Management.
services: automation
ms.topic: conceptual
ms.date: 01/05/2021
ms.custom: mvc
ms.openlocfilehash: d0399aed9be8d81abb2aa55190225570ddcc1a4e
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97913199"
---
# <a name="remove-vms-from-update-management"></a>Eliminación de una máquina virtual desde Update Management

Cuando haya terminado de administrar las actualizaciones en las máquinas virtuales de su entorno, puede dejar de administrar las máquinas virtuales con la característica [Update Management](overview.md). Para dejar de administrarlos, modificará la consulta de búsqueda guardada `MicrosoftDefaultComputerGroup` en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation.

## <a name="sign-into-the-azure-portal"></a>Inicie sesión en Azure Portal.

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="to-remove-your-vms"></a>Para quitar las máquinas virtuales

1. En Azure Portal, inicie **Cloud Shell** en la navegación superior de Azure Portal. Si no está familiarizado con Azure Cloud Shell, consulte [Introducción a Azure Cloud Shell](../../cloud-shell/overview.md).

2. Use el siguiente comando para identificar el UUID de un equipo que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En Azure Portal, vaya a **Areas de trabajo de Log Analytics**. Seleccione el área de trabajo de la lista.

4. En el área de trabajo de Log Analytics, seleccione **Configuración avanzada** y elija **Grupos de equipos** en el menú de la izquierda.

5. En **Grupos de equipos**, en el panel de la derecha, seleccione **Grupos guardados**.

6. En la tabla, en la consulta de búsqueda guardada **Updates:MicrosoftDefaultComputerGroup**, haga clic en el icono **Ver miembros** para ejecutar y ver sus miembros.

7. En el editor de consultas, revise la consulta y busque el UUID de la máquina virtual. Quite el UUID de la máquina virtual y repita los pasos para las demás máquinas virtuales que desee quitar.

8. Guarde la búsqueda guardada cuando haya terminado de modificarla; para ello, seleccione **Guardar** en la barra superior. Cuando se le pida, especifique lo siguiente:

    * **Nombre**: MicrosoftDefaultComputerGroup
    * **Guardar como**: Función
    * **Alias**: Updates__MicrosoftDefaultComputerGroup
    * **Categoría**: Actualizaciones

>[!NOTE]
>Las máquinas todavía se muestran después de anular la inscripción, ya que se informa de todas las máquinas que se han evaluado en las últimas 24 horas. Después de quitar la máquina, debe esperar 24 horas antes de que ya no aparezcan en la lista.

## <a name="next-steps"></a>Pasos siguientes

Para volver a habilitar la administración de la máquina virtual, consulte [Examinen de Azure Portal para habilitar Update Management](enable-from-portal.md) o [Habilitación de Update Management desde una máquina virtual de Azure](enable-from-vm.md).