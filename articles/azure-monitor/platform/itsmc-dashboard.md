---
title: Investigación de errores mediante el panel
description: Este documento contiene información sobre la investigación de errores mediante el panel
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 9a43318ed935dd1e71b6da75c9f078fbebbd29eb
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599024"
---
# <a name="error-investigation-using-the-dashboard"></a>Investigación de errores mediante el panel

Esta página contiene información sobre el panel del conector de ITSM. Este panel le ayudará a investigar el estado del conector de ITSM.

## <a name="how-to-view-the-dashboard"></a>Visualización del panel

Para ver los errores en el panel, siga estos pasos:

1. En **Todos los recursos**, busque **ServiceDesk(*nombre de su área de trabajo*)** :

   ![Captura de pantalla en la que se muestran los recursos recientes en Azure Portal.](media/itsmc-definition/create-new-connection-from-resource.png)

2. En **Orígenes de datos del área de trabajo** en el panel de la izquierda, seleccione **Conexiones de ITSM**:

   ![Captura de pantalla en la que se muestra el elemento de menú Conexiones de ITSM.](media/itsmc-overview/add-new-itsm-connection.png)

3. En **Resumen**, en el cuadro **Conector de Administración de servicios de TI** de la izquierda, seleccione **Ver resumen**:

    ![Captura de pantalla que muestra Ver resumen.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. En **Resumen**, en el cuadro **Conector de Administración de servicios de TI** de la izquierda, haga clic en el grafo:

    ![Captura de pantalla que muestra el clic en el grafo.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Con este panel podrá revisar el estado y los errores del conector.
    ![Captura de pantalla que muestra el estado del conector.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="dashboard-elements"></a>Elementos del panel

El panel contiene información sobre las alertas que se enviaron a la herramienta ITSM mediante este conector.
Asimismo, el panel se divide en cuatro partes:

1. Elemento de trabajo creado: el gráfico y la tabla siguiente contienen el recuento de los elementos de trabajo por tipo. Si hace clic en el gráfico o en la tabla, podrá ver más detalles sobre los elementos de trabajo.
    ![Captura de pantalla que muestra el elemento de trabajo creado.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)
2. Equipos afectados: las tablas contienen detalles sobre los elementos de configuración que crearon elementos de configuración.
    Al hacer clic en las filas de las tablas, puede obtener más detalles sobre los elementos de configuración.
    La tabla contiene un número limitado de filas, pero si desea ver toda la lista, puede hacer clic en "Ver todo".
    ![Captura de pantalla que muestra los equipos afectados.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)
3. Estado del conector: el gráfico y la tabla siguiente contienen mensajes sobre el estado del conector. Al hacer clic en el gráfico de las filas de la tabla, puede obtener más detalles sobre los mensajes del estado del conector.
    La tabla contiene un número limitado de filas, pero si desea ver toda la lista, puede hacer clic en "Ver todo".

    Puede ver los detalles de los mensajes de la tabla [aquí](itsmc-dashboard-errors.md).

    ![Captura de pantalla que muestra el estado del conector.](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)
4. Reglas de alertas: las tablas contienen información sobre el número de reglas de alerta que se detectaron.
    Al hacer clic en las filas de las tablas, puede obtener más detalles sobre las reglas que se detectaron.
    La tabla contiene un número limitado de filas, pero si desea ver toda la lista, puede hacer clic en "Ver todo".
    ![Captura de pantalla que muestra las reglas de alertas.](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)