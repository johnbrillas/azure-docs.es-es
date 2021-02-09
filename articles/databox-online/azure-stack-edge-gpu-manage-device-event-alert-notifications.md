---
title: Administración de notificaciones de alertas de eventos de dispositivos en recursos de Azure Stack Edge Pro | Microsoft Docs
description: En este artículo se explica cómo usar Azure Portal para administrar recursos compartidos en Azure Stack Edge Pro.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: a18937d84b05159626708d577982405d79161a1a
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99499484"
---
# <a name="manage-device-event-alert-notifications-on-azure-stack-edge-pro-resources"></a>Administración de notificaciones de alertas de eventos de dispositivos en recursos de Azure Stack Edge Pro

En este artículo se describe cómo crear reglas de acción en Azure Portal para desencadenar o suprimir notificaciones de alertas para los eventos de dispositivo que se producen dentro de un grupo de recursos, una suscripción de Azure o un recurso de Azure Stack Edge individual. Este artículo se aplica a todos los modelos de Azure Stack Edge.  

## <a name="about-action-rules"></a>Acerca de las reglas de acción

Una regla de acción puede desencadenar o suprimir notificaciones de alerta. La regla de acción se agrega a un *grupo de acciones* (un conjunto de preferencias de notificación que se usa para notificar a los usuarios que necesitan actuar en las alertas desencadenadas en distintos contextos de un recurso o conjunto de recursos).

Para más información sobre las reglas de acción, consulte [Configuración de una regla de acción](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule). Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones en Azure Portal](/blob/master/articles/azure-monitor/platform/action-groups).

> [!NOTE]
> La característica de reglas de acción está en versión preliminar. Es posible que algunas pantallas y algunos pasos cambien cuando se refina el proceso.


## <a name="create-an-action-rule"></a>Creación de una regla de acción

Siga estos pasos en Azure Portal para crear una regla de acción para un dispositivo de Azure Stack Edge.

> [!NOTE]
> Con estos pasos se crea una regla de acción que envía notificaciones a un grupo de acciones. Para más información sobre la creación de una regla de acción para suprimir notificaciones, consulte [Configuración de una regla de acción](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule).

1. Vaya al recurso de Azure Stack Edge en Azure Portal y, después, vaya a **Supervisión > Alertas**. Seleccione **Administrar acciones**.

   ![Supervisión de alertas, vista Administrar acciones](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/action-rules-open-view-01.png)

2. Seleccione **Reglas de acción (versión preliminar)** y, después, seleccione **+Nueva regla de acción**.

   ![Administrar acciones, opción Reglas de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/action-rules-open-view-02.png)

3. En la pantalla **Crear regla de acción**, use **Ámbito** para seleccionar una suscripción de Azure, un grupo de recursos o un recurso de destino. La regla de acción actuará en todas las alertas generadas dentro de ese ámbito.

   1. Elija **Seleccionar** por **Ámbito**.

      ![Seleccionar un ámbito para una nueva regla de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-01.png)

   2. Seleccione la **suscripción** para la regla de acción y, opcionalmente, filtre por un tipo de **recurso**. Para filtrar a los recursos de Azure Stack Edge, seleccione **Dispositivos Data Box Edge (dataBoxEdge)** .

      En el área **Recurso** se enumeran los recursos disponibles en función de lo que haya seleccionado.
  
      ![Recursos disponibles en la pantalla Seleccionar ámbito](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-02.png)

   3. Active la casilla de cada recurso al que desea aplicar la regla. Puede seleccionar la suscripción, los grupos de recursos o los recursos individuales. A continuación, seleccione **Done** (Listo).

      ![Configuración de ejemplo de un ámbito de regla de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-03.png)

      La pantalla **Crear regla de acción** muestra el ámbito seleccionado.

      ![Ámbito completado para una regla de acción de Azure Stack Edge](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-scope-04.png)

4. Use las opciones de **Filtrar** para restringir la aplicación de la regla al subconjunto de alertas del ámbito seleccionado.

   1. Seleccione **Agregar** para abrir el panel **Agregar filtros**.

      ![Opción para agregar filtros a una regla de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-filter-01.png)

   2. En **Filtros**, agregue los filtros que desee aplicar. Para cada filtro, seleccione el tipo de filtro, el **operador** y el **valor**.
   
      Para obtener una lista de opciones de filtro, consulte [Criterios de filtro](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#filter-criteria).

      Los siguientes filtros de ejemplo se aplican a todas las alertas con los niveles de gravedad 2, 3 y 4 que el servicio Monitor genera para los recursos de Azure Stack Edge.

      Cuando termine de agregar filtros, seleccione **Listo**.
   
      ![Filtro de ejemplo para una regla de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-filter-02.png)

5. En la pantalla **Crear regla de acción**, seleccione **Grupo de acciones** para crear una regla que envíe notificaciones. A continuación, en **Acciones**, elija **Seleccionar**.

   ![Opción de Grupo de acciones para crear una regla de acción que envíe notificaciones](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-action-group-01.png)

   > [!NOTE]
   > Para crear una regla que suprima las notificaciones, debe elegir **Supresión**. Para más información, consulte [Configuración de una regla de acción](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule).

6. Seleccione el grupo de acciones que desea usar con esta regla de acción. Luego, elija **Seleccionar**. La nueva regla de acción se agregará a las preferencias de notificación del grupo de acciones seleccionado.

   Si necesita crear un grupo de acciones, seleccione **+Crear grupo de acciones** y siga los pasos descritos en [Creación de un grupo de acciones con Azure Portal](/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal).

   ![Seleccione un grupo de acciones para usarlo con la regla y elija Seleccionar.](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-action-group-02.png)

7. Asigne a la nueva regla de acción un **nombre** y una **descripción**, y asigne la regla a un grupo de recursos.

9. La nueva regla se encuentra habilitada de forma predeterminada. Si no desea empezar a usar la regla inmediatamente, seleccione **No** en **Enable rule update creation**(Habilitar creación de actualización de regla).

10. Cuando finalice la configuración, seleccione **Crear**.

    ![Configuración completada de una regla de acción que enviará notificaciones de alerta](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-completed-settings.png)

    Se abre la pantalla **Reglas de acción de (versión preliminar)** , pero es posible que no vea la nueva regla de acción inmediatamente. Lo que verá es **Todos**  los grupos de recursos.

11. Para ver la nueva regla de acción, seleccione el grupo de recursos de la regla.

    ![Pantalla Reglas de acción en la que se muestra la nueva regla](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/new-action-rule-displayed.png)


## <a name="view-notifications"></a>Ver notificaciones

Las notificaciones salen cuando un evento nuevo desencadena una alerta para un recurso que se encuentra dentro del ámbito de una regla de acción.

El grupo de acciones de una regla establece quién recibe una notificación y el tipo de notificación que se envía: correo electrónico, un mensaje SMS o ambos.

Es posible que tarde unos minutos en recibir notificaciones después de que se desencadene una alerta.

La notificación por correo electrónico será similar a esta.

![Notificación por correo electrónico de ejemplo de una regla de acción](media/azure-stack-edge-gpu-manage-device-event-alert-notifications/sample-action-rule-email-notification.png)


## <a name="next-steps"></a>Pasos siguientes

<!-- - See [Create and manage action groups in the Azure portal](/blob/master/articles/azure-monitor/platform/action-groups) for guidance on creating a new action group.
- See [Configure an action rule](/azure/azure-monitor/platform/alerts-action-rules?tabs=portal#configuring-an-action-rule) for more info about creating action rules that send or suppress alert notifications. -2 bullets referenced above. Making room for local tasks in "Next Steps." --> 
- Para obtener información sobre cómo revisar eventos de dispositivo, el estado de hardware y los gráficos de métricas, consulte [Supervisión de Azure Stack Edge Pro](azure-stack-edge-monitor.md). 
- Para obtener información sobre cómo optimizar Azure Monitor para los dispositivos con GPU de Azure Stack Edge Pro, consulte [Uso de Azure Monitor](azure-stack-edge-gpu-enable-azure-monitor.md).
- Para obtener información sobre la administración de alertas individuales, consulte [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](/../azure-monitor/platform/alerts-metric.md).
