---
title: Investigación de errores mediante el panel de ITSMC
description: Aprenda a usar el panel de Conector de Administración de servicios de TI para investigar los errores.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 5e1acf422abf487edda3e871fa99d07212c59b3a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102039536"
---
# <a name="investigate-errors-by-using-the-itsmc-dashboard"></a>Investigación de errores mediante el panel de ITSMC

Este artículo contiene información sobre el panel de Conector de Administración de servicios de TI (ITSMC). El panel le ayuda a investigar el estado del conector.

## <a name="view-errors"></a>Visualización de errores

Para ver los errores en el panel:

1. Seleccione **Todos los recursos** y busque **ServiceDesk(*nombre de su área de trabajo*)** .

   ![Captura de pantalla que muestra los recursos de lo servicios de Azure.](media/itsmc-definition/create-new-connection-from-resource.png)

2. En **Orígenes de datos del área de trabajo** en el panel de la izquierda, seleccione **Conexiones de ITSM**:

   ![Captura de pantalla que muestra la selección de conexiones de ITSM en Orígenes de datos del área de trabajo.](media/itsmc-overview/add-new-itsm-connection.png)

3. En **Resumen**, en el área del **Conector de Administración de servicios de TI**, seleccione **Ver resumen**:

   ![Captura de pantalla que muestra el botón Ver resumen.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. Cuando aparezca un gráfico en el área del **Conector de Administración de servicios de TI**, selecciónelo:

   ![Captura de pantalla que muestra la selección de un gráfico.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Aparecerá el panel. Úselo para revisar el estado y los errores del conector.
   
   ![Captura de pantalla que muestra el estado del conector en el panel.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="understand-dashboard-elements"></a>Descripción de los elementos del panel

El panel contiene información sobre las alertas que se enviaron a la herramienta ITSM mediante este conector. Asimismo, el panel se divide en cuatro partes.

### <a name="created-work-items"></a>Elementos de trabajo creados 

En el área **ELEMENTOS DE TRABAJO CREADOS**, el gráfico y la tabla que están debajo contienen el recuento de los elementos de trabajo por tipo. Si selecciona el gráfico o la tabla, podrá ver más detalles sobre los elementos de trabajo.

![Captura de pantalla que muestra el elemento de trabajo creado.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)

### <a name="affected-computers"></a>Equipos afectados 

En el área **EQUIPOS AFECTADOS**, en la tabla se enumeran los equipos y sus elementos de trabajo asociados. Al seleccionar las filas de las tablas, puede obtener más detalles sobre los equipos.

La tabla contiene un número limitado de filas. Si quiere ver todas las filas, seleccione **Ver todo**.

![Captura de pantalla que muestra los equipos afectados.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)

### <a name="connector-status"></a>Estado del conector 

En el área **ESTADO DEL CONECTOR**, el gráfico y la tabla que están debajo contienen mensajes sobre el estado del conector. Al seleccionar el gráfico o las filas de la tabla, puede obtener más detalles sobre los mensajes.

La tabla contiene un número limitado de filas. Si quiere ver todas las filas, seleccione **Ver todo**.

Para obtener más información sobre los mensajes de la tabla, consulte [este artículo](itsmc-dashboard-errors.md).

![Captura de pantalla que muestra el estado del conector.](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)

### <a name="alert-rules"></a>Las reglas de alertas 

En el área **REGLAS DE ALERTAS**, las tablas contienen información sobre el número de reglas de alertas que se detectaron. Al seleccionar las filas de la tabla, puede obtener más detalles sobre las reglas detectadas.
    
La tabla contiene un número limitado de filas. Si quiere ver todas las filas, seleccione **Ver todo**.

![Captura de pantalla que muestra las reglas de alertas.](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)
