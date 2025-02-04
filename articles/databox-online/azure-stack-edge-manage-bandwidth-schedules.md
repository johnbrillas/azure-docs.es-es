---
title: Administración de programaciones de ancho de banda en Azure Stack Edge Pro | Microsoft Docs
description: En este artículo se explica cómo puede usar Azure Portal para administrar las programaciones de ancho de banda en Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: e73a02c93807072e30c8ce2a1a7feb30e9d3c8c6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91978975"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge-pro"></a>Uso de Azure Portal para administrar las programaciones de ancho de banda en Azure Stack Edge Pro  

En este artículo se explica cómo administrar los usuarios en Azure Stack Edge Pro. Las programaciones del ancho de banda le permiten configurar el uso del ancho de banda de red en programaciones en varias horas del día. Dichas programaciones se pueden aplicar a las operaciones de carga y descarga desde el dispositivo a la nube.

Puede agregar, modificar o eliminar las programaciones de ancho de banda de Azure Stack Edge Pro en Azure Portal.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Adición de una programación
> * Modificar una programación
> * Eliminación de una programación


## <a name="add-a-schedule"></a>Adición de una programación

Para agregar una programación siga estos pasos en Azure Portal.

1. En la instancia de Azure Portal del recurso de Azure Stack Edge, vaya a **Ancho de banda**.
2. En el panel derecho, seleccione **+ Agregar programación**.

    ![Seleccionar ancho de banda](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. En **Agregar programación**: 

   1. Especifique los valores de **Día de inicio**, **Día de finalización**, **Hora de inicio** y **Hora de finalización** de la programación.
   2. Seleccione la opción **Todo el día** si esta programación se debe ejecutar todo el día.
   3. **Velocidad de ancho de banda** es el ancho de banda, en Megabits por segundo (Mbps), que utiliza el dispositivo en operaciones que afectan a la nube (tanto cargas como descargas). Proporcione un número entre 20 y 1.000.000.007 para este campo.
   4. Seleccione el ancho de banda **Ilimitado** si no desea regular la carga y la descarga de la fecha.
   5. Seleccione **Agregar**.

      ![Agregar programación](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Se crea una programación con los parámetros especificados. Esta programación se muestra en la lista de programaciones de ancho de banda del portal.

    ![Lista actualizada de las programaciones de ancho de banda](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Edición de programación

Siga estos pasos para editar una programación del ancho de banda.

1. En Azure Portal, vaya al recurso de Azure Stack Edge y luego a **Ancho de banda**. 
2. En la lista de programaciones de ancho de banda, seleccione y haga clic en la programación que desee modificar.
    ![Seleccionar programación de ancho de banda](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. Realice los cambios deseados y guárdelos.

    ![Modificación de un usuario](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Después de que se modifique la programación, se actualiza la lista de programaciones para reflejar la programación modificada.

    ![Modificación de un usuario 2](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Eliminación de una programación

Siga estos pasos para eliminar una programación de ancho de banda asociada a un dispositivo Azure Stack Edge Pro.

1. En Azure Portal, vaya al recurso de Azure Stack Edge y luego a **Ancho de banda**.  

2. En la lista de programaciones de ancho de banda, seleccione la programación que desea eliminar. En **Editar programación**, seleccione **Eliminar**. Cuando se le pida confirmación, seleccione **Sí**.

   ![Eliminar un usuario](media/azure-stack-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Después de que se elimina la programación se actualiza la lista de programaciones.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar los recursos compartidos](azure-stack-edge-manage-shares.md).
