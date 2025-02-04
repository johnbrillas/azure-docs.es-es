---
title: Administración de programaciones de ancho de banda en Azure Stack Edge Pro con GPU | Microsoft Docs
description: En este artículo se explica cómo usar Azure Portal para administrar las programaciones de ancho de banda en Azure Stack Edge Pro con GPU.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 7b091ff1ec825ac2292345183eb77bc37f02f6e0
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102637355"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge-pro-gpu"></a>Uso de Azure Portal para administrar las programaciones de ancho de banda en Azure Stack Edge Pro con GPU 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En este artículo se describe cómo administrar las programaciones de ancho de banda en Azure Stack Edge Pro. Las programaciones del ancho de banda le permiten configurar el uso del ancho de banda de red en programaciones en varias horas del día. Dichas programaciones se pueden aplicar a las operaciones de carga y descarga desde el dispositivo a la nube.

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

    ![Seleccionar ancho de banda](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-1.png)

3. En **Agregar programación**:

   1. Especifique los valores de **Día de inicio**, **Día de finalización**, **Hora de inicio** y **Hora de finalización** de la programación.
   2. Seleccione la opción **Todo el día** si esta programación se debe ejecutar todo el día.
   3. **Velocidad de ancho de banda** es el ancho de banda, en Megabits por segundo (Mbps), que utiliza el dispositivo en operaciones que afectan a la nube (tanto cargas como descargas). Proporcione un número entre 64 y 2 147 483 647 para este campo.
   4. Seleccione **Ancho de banda ilimitado** si no quiere regular la carga y la descarga de la fecha.
   5. Seleccione **Agregar**.

      ![Agregar programación](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-2.png)

3. Se crea una programación con los parámetros especificados. Esta programación se muestra en la lista de programaciones de ancho de banda del portal.

    ![Lista actualizada de las programaciones de ancho de banda](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Edición de programación

Siga estos pasos para editar una programación del ancho de banda.

1. En Azure Portal, vaya al recurso de Azure Stack Edge y luego a **Ancho de banda**.
2. En la lista de programaciones de ancho de banda, seleccione la programación que quiere modificar.

   ![Seleccionar programación de ancho de banda](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-1.png)

3. Realice los cambios deseados y guárdelos.

    ![Modificación de un usuario](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-2.png)

4. Después de que se modifique la programación, se actualiza la lista de programaciones para reflejar la programación modificada.

    ![Modificación de un usuario 2](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Eliminación de una programación

Siga estos pasos para eliminar una programación de ancho de banda asociada a un dispositivo Azure Stack Edge Pro.

1. En Azure Portal, vaya al recurso de Azure Stack Edge y luego a **Ancho de banda**.  

2. En la lista de programaciones de ancho de banda, seleccione la programación que desea eliminar. En **Editar programación**, seleccione **Eliminar**. Cuando se le pida confirmación, seleccione **Sí**.

   ![Eliminar un usuario](media/azure-stack-edge-gpu-manage-bandwidth-schedules/delete-schedule-2.png)

3. Después de que se elimina la programación se actualiza la lista de programaciones.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar los recursos compartidos](azure-stack-edge-gpu-manage-shares.md).
