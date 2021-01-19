---
title: Conector de Administración de servicios de TI en Log Analytics
description: En este artículo se proporciona información general sobre el Conector de Administración de servicios de TI (ITSMC) e información sobre cómo usarlo para supervisar y administrar los elementos de trabajo de ITSM en Log Analytics y resolver rápidamente cualquier problema.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.custom: references_regions
ms.openlocfilehash: 717a1bc4361ba4a7366f4864c1fe44f93b6f4b5e
ms.sourcegitcommit: 02b1179dff399c1aa3210b5b73bf805791d45ca2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98127862"
---
# <a name="connect-azure-to-itsm-tools-by-using-it-service-management-connector"></a>Conexión de Azure a las herramientas de ITSM mediante el Conector de Administración de servicios de TI

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

En este artículo se proporciona información sobre cómo configurar el Conector de Administración de servicios de TI (ITSMC) en Log Analytics para administrar de forma centralizada los elementos de trabajo.

## <a name="add-it-service-management-connector"></a>Adición del Conector de Administración de servicios de TI

Para poder crear una conexión, debe agregar ITSMC.

1. En Azure Portal, seleccione **Crear un recurso**:

   ![Captura de pantalla en la que se muestra el elemento de menú Crear un recurso.](media/itsmc-overview/azure-add-new-resource.png)

2. Busque el **Conector de Administración de servicios de TI** en Azure Marketplace. Seleccione **Crear**:

   ![Captura de pantalla en la que muestra el botón Crear en Azure Marketplace.](media/itsmc-overview/add-itsmc-solution.png)

3. En la sección **Área de trabajo de LA**, seleccione el área de trabajo de Azure Log Analytics donde quiera instalar ITSMC.
   >[!NOTE]
   >
   > * ITSMC solo se puede instalar en áreas de trabajo de Log Analytics en las siguientes regiones: Este de EE. UU., Oeste de EE. UU. 2, Centro-sur de EE. UU., Centro-oeste de EE. UU., US Gov Arizona, US Gov Virginia, Centro de Canadá, Oeste de Europa, Sur de Reino Unido, Sudeste Asiático, Este de Japón, Centro de la India y Sudeste de Australia.

4. En la sección **Área de trabajo de Log Analytics**, seleccione el grupo de recursos donde quiera crear el recurso de ITSMC:

   ![Captura de pantalla en la que se muestra la sección Área de trabajo de Log Analytics.](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Como parte de la transición continuada de Microsoft Operations Management Suite (OMS) a Azure Monitor, las áreas de trabajo de OMS ahora se conocen como *áreas de trabajo de Log Analytics*.

5. Seleccione **Aceptar**.

Cuando se implemente el recurso de ITSMC, aparecerá una notificación en la esquina superior derecha de la ventana.

## <a name="create-an-itsm-connection"></a>Cree una conexión de ITSM

Después de instalar ITSMC, puede crear una conexión.

Para crear una conexión, debe preparar la herramienta de ITSM para permitir la conexión desde ITSMC.  

En función del producto de ITSM al que se va a conectar, seleccione alguno de los siguientes vínculos para obtener instrucciones:

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

Una vez que haya preparado las herramientas de ITSM, siga estos pasos para crear una conexión:

1. En **Todos los recursos**, busque **ServiceDesk(*nombre de su área de trabajo*)** :

   ![Captura de pantalla en la que se muestran los recursos recientes en Azure Portal.](media/itsmc-definition/create-new-connection-from-resource.png)

1. En **Orígenes de datos del área de trabajo** en el panel de la izquierda, seleccione **Conexiones de ITSM**:

   ![Captura de pantalla en la que se muestra el elemento de menú Conexiones de ITSM.](media/itsmc-overview/add-new-itsm-connection.png)
1. Seleccione **Agregar conexión**.

1. Especifique la configuración de conexión tal y como se describe, según los productos o servicios de ITSM:

    - [ServiceNow](./itsmc-connections-servicenow.md)
    - [System Center Service Manager](./itsmc-connections-scsm.md)
    - [Cherwell](./itsmc-connections-cherwell.md)
    - [Provance](./itsmc-connections-provance.md)

   > [!NOTE]
   >
   > De forma predeterminada, ITSMC actualiza los datos de configuración de la conexión una vez cada 24 horas. Para actualizar los datos de la conexión al instante para reflejar cualquier modificación o actualizaciones de plantilla que se realicen, seleccione el botón **Sincronizar** que se muestra en la hoja de la conexión:
   >
   > ![Captura de pantalla en la que se muestra el botón Sincronizar en la hoja conexión.](media/itsmc-overview/itsmc-connections-refresh.png)

## <a name="use-itsmc"></a>Uso de ITSMC

   Puede usar ITSMC para crear alertas de Azure Monitor en la herramienta ITSM.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Creación de elementos de trabajo de ITSM a partir de alertas de Azure

Después de crear la conexión de ITSM, puede crear elementos de trabajo en la herramienta ITSM en función de las alertas de Azure. Para crear los elementos de trabajo, usará la acción de ITSM en grupos de acciones.

Los grupos de acciones proporcionan una manera modular y reutilizable de desencadenar acciones para las alertas de Azure. Puede usar los grupos de acciones con alertas de métricas, de registros de actividad y de Azure Log Analytics en Azure Portal.

> [!NOTE]
> Después de crear la conexión de ITSM, debe esperar 30 minutos para que finalice el proceso de sincronización.

### <a name="template-definitions"></a>Definiciones de plantillas

   Existen tipos de elementos de trabajo que pueden utilizar las plantillas que haya definido la herramienta ITSM.
Con el uso de plantillas, puede definir los campos que se rellenarán automáticamente en función de los valores fijos que se definen como parte del grupo de acciones. Las plantillas se definen en la herramienta ITSM.
Puede definir qué plantilla le gustaría usar como parte de la definición del grupo de acciones.

Use el procedimiento siguiente para crear grupos de acciones:

1. En Azure Portal, seleccione **Alertas**.
2. En el menú de la parte superior de la pantalla, seleccione **Administrar acciones**:

    ![Captura de pantalla en la que se muestra el elemento de menú Administrar acciones.](media/itsmc-overview/action-groups-selection-big.png)

   Aparece la ventana **Crear grupo de acciones**.

3. Seleccione las opciones de **Suscripción** y **Grupo de recursos** donde quiere crear el grupo de acciones. Escriba lo que corresponda en **Nombre del grupo de acciones** y **Nombre para mostrar** para el grupo de acciones. Seleccione **Siguiente: notificaciones**.

    ![Captura de pantalla en la que se muestra la ventana Crear grupo de acciones.](media/itsmc-overview/action-groups-details.png)

4. En la lista de notificaciones, seleccione **Siguiente: acciones**.
5. En la lista de acciones, seleccione **ITSM** en la lista **Tipo de acción**. Proporcione un **Nombre** para la acción. Seleccione el botón del lápiz que representa **Editar detalles**.

    ![Captura de pantalla que muestra la definición de un grupo de acciones.](media/itsmc-definition/action-group-pen.png)

6. En la lista **Suscripción**, seleccione la suscripción en la que se encuentra el área de trabajo de Log Analytics. En la lista **Conexión**, seleccione el nombre del conector de ITSM. Irá seguido del nombre del área de trabajo. Por ejemplo, MiConectorITSM(MiÁreaDeTrabajo).

7. Seleccione un tipo de **Elemento de trabajo**.

8. Si desea rellenar los campos predeterminados con valores fijos, seleccione **Usar la plantilla personalizada**. De lo contrario, elija una [plantilla](#template-definitions) existente en la lista **Plantilla** y escriba los valores fijos en los campos de la plantilla.

9. En la última sección de la definición del grupo de acciones ITSM puede definir cuántos elementos de trabajo se crearán para cada alerta.Esta sección solo es pertinente para las alertas de búsqueda de registros.

    >[!NOTE]
    >
    > * This section is relevant only for Log Search Alerts.
    > * Para todos los demás tipos de alerta, se creará un elemento de trabajo por cada alerta.

    * En caso de que seleccione en la lista desplegable "Elemento de trabajo" el valor "Incidente" o "Alerta": ![Captura de pantalla que muestra la ventana Incidente de ITSM.](media/itsmc-overview/itsm-action-configuration.png)
        * Si activa la casilla **"Crear elementos de trabajo individuales para cada elemento de configuración"** , cada elemento de configuración en cada alerta creará un elemento de trabajo. Como resultado de varias alertas para los mismos elementos de configuración afectados, habrá más de un elemento de trabajo para cada elemento de configuración.

             Por ejemplo:
             1) Alerta 1 con tres elementos de configuración: A, B, C: crearán tres elementos de trabajo.
             2) Alerta 2 con un elemento de configuración: A: creará un elemento de trabajo.

        * Si desactiva la casilla **"Crear elementos de trabajo individuales para cada elemento de configuración"** , el conector de ITSM creará un único elemento de trabajo para cada regla de alerta y le anexará todos los elementos de configuración afectados. Si el anterior está cerrado, se creará un nuevo elemento de trabajo.

        >[!NOTE]
        > En este caso, algunas de las alertas desencadenadas no generarán elementos de trabajo nuevos en la herramienta ITSM.

        Por ejemplo:
         1) Alerta 1 con tres elementos de configuración: A, B, C: crearán un elemento de trabajo.
         2) Alerta 2 para la misma regla de alerta que en el paso A con un elemento de configuración: D: el elemento D se adjuntará a la lista de elementos de configuración afectados, en el elemento de trabajo creado en el paso A.
         3) Alerta 3 para una regla de alerta diferente con un elemento de configuración: E: creará un elemento de trabajo.

    * En caso de que seleccione en la lista desplegable "Elemento de trabajo" el elemento "Evento": ![Captura de pantalla en la que se muestra la ventana Evento de ITSM.](media/itsmc-overview/itsm-action-configuration-event.png)

        * Si selecciona **"Crear elementos de trabajo individuales para cada entrada de registro (no se rellena el campo de elemento de configuración. Puede dar como resultado un gran número de elementos de trabajo)."** en la selección de botones de radio, se creará un elemento de trabajo por cada fila de los resultados de búsqueda de la consulta de alerta de búsqueda de registros. La propiedad description de la carga útil del elemento de trabajo contendrá la fila de los resultados de la búsqueda.
        * Si selecciona **"Crear elementos de trabajo individuales para cada elemento de configuración"** en la selección de botones de radio, cada elemento de configuración en cada alerta creará un nuevo de elemento de trabajo. Puede haber más de un elemento de trabajo por cada elemento de configuración en el sistema ITSM. Será lo mismo que activar la casilla en la sección Incidente o alerta.

10. Seleccione **Aceptar**.

Al crear o editar una regla de alerta de Azure, use un grupo de acciones, que tiene una acción de ITSM. Cuando se desencadena la alerta, se crea o actualiza el elemento de trabajo en la herramienta ITSM.

> [!NOTE]
>
>- Para obtener información sobre los precios de la acción de ITSM, vea la [página de precios](https://azure.microsoft.com/pricing/details/monitor/) de los grupos de acciones.
>
>
>- El campo de descripción breve de la definición de la regla de alerta está limitado a 40 caracteres cuando se envía mediante la acción de ITSM.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)
