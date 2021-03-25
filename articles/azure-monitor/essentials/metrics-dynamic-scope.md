---
title: Visualización de varios recursos en el Explorador de métricas de Azure
description: Obtenga información sobre cómo visualizar varios recursos mediante el Explorador de métricas de Azure.
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.openlocfilehash: 85fa252f999407d5cd7f75e954554e3b14357071
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037326"
---
# <a name="view-multiple-resources-in-the-azure-metrics-explorer"></a>Visualización de varios recursos en el Explorador de métricas de Azure

El selector del ámbito de recursos le permite ver las métricas en varios recursos que se encuentren en la misma suscripción y región. En este artículo se explica cómo ver varios recursos mediante la característica Explorador de métricas de Azure de Azure Monitor. 

## <a name="select-a-resource"></a>Selección de un recurso 

Seleccione **Métricas** en el menú de **Azure Monitor** o en la sección **Supervisión** del menú de un recurso. A continuación, elija **Seleccionar un ámbito** para abrir el selector de ámbitos. 

Use el selector de ámbitos para seleccionar los recursos cuyas métricas desea ver. El ámbito se debe rellenar al abrir el Explorador de métricas de Azure desde el menú de un recurso. 

![Captura de pantalla que muestra cómo abrir el selector de ámbitos de recursos.](./media/metrics-dynamic-scope/019.png)

## <a name="select-multiple-resources"></a>Seleccionar varios recursos 

Algunos tipos de recursos pueden consultar las métricas de varios recursos. Las métricas deben estar en la misma suscripción y ubicación. Busque estos tipos de recursos en la parte superior del menú **Tipos de recursos**.

![Captura de pantalla que muestra un menú de recursos que son compatibles con varios recursos.](./media/metrics-dynamic-scope/020.png)

> [!WARNING] 
> Debe tener el permiso de tipo Lector de supervisión en el nivel de suscripción para visualizar las métricas de varios recursos, grupos de recursos o de una suscripción. Para más información, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

Para visualizar las métricas en varios recursos, seleccione varios recursos en el selector del ámbito de recursos. 

![Captura de pantalla que muestra cómo seleccionar varios recursos.](./media/metrics-dynamic-scope/021.png)

> [!NOTE]
> Los recursos que seleccione deben estar en el mismo tipo de recurso, ubicación y suscripción. Los recursos que no se ajustan a estos criterios no se pueden seleccionar. 

Cuando termine, elija **Aplicar** para guardar las selecciones. 

## <a name="select-a-resource-group-or-subscription"></a>Selección de un grupo de recursos o suscripción 

> [!WARNING]
> Debe tener el permiso de tipo Lector de supervisión en el nivel de suscripción para visualizar las métricas de varios recursos, grupos de recursos o de una suscripción. 

En el caso de los tipos que son compatibles con varios recursos, puede consultar las métricas en una suscripción o en varios grupos de recursos. Para comenzar, seleccione una suscripción o uno o varios grupos de recursos: 

![Captura de pantalla que muestra cómo realizar consultas en varios grupos de recursos.](./media/metrics-dynamic-scope/022.png)

Seleccione un tipo de recurso y una ubicación. 

![Captura de pantalla que muestra los grupos de recursos seleccionados.](./media/metrics-dynamic-scope/023.png)

Puede expandir los ámbitos seleccionados para comprobar los recursos a los que se aplican las selecciones.

![Captura de pantalla que muestra los recursos seleccionados dentro de los grupos.](./media/metrics-dynamic-scope/024.png)

Cuando termine de seleccionar ámbitos, seleccione **Aplicar**. 

## <a name="split-and-filter-by-resource-group-or-resources"></a>División y filtrado por grupo de recursos o recursos

Después de trazar los recursos, puede usar la división y el filtrado para obtener más información sobre los datos. 

La opción de división le permite visualizar el modo en que los distintos segmentos de la métrica se comparan entre sí. Por ejemplo, cuando se traza una métrica de varios recursos, puede elegir **Aplicar división** para dividir en función del id. de recurso o del grupo de recursos. La división le permite comparar una sola métrica entre varios recursos o grupos de recursos.  

Por ejemplo, en el siguiente gráfico se muestra el porcentaje de CPU en nueve máquinas virtuales. Al dividir por id. de recurso, verá cómo difiere el porcentaje de CPU según la máquina virtual. 

![Captura de pantalla que muestra cómo usar la división para ver el porcentaje de CPU entre máquinas virtuales.](./media/metrics-dynamic-scope/026.png)

Junto con la división, puede usar el filtrado para mostrar solo los grupos de recursos que desea ver.  Por ejemplo, para ver el porcentaje de CPU de las VM de un determinado grupo de recursos, puede seleccionar **Agregar filtro** para filtrar por grupo de recursos. 

En este ejemplo, se filtra por TailspinToysDemo. En este caso, el filtro quita las métricas asociadas a los recursos en TailspinToys. 

![Captura de pantalla que muestra cómo filtrar por grupo de recursos.](./media/metrics-dynamic-scope/027.png)

## <a name="pin-multiple-resource-charts"></a>Anclar gráficos de varios recursos 

Los gráficos de varios recursos que visualizan las métricas de grupos de recursos y suscripciones requieren que el usuario tenga permiso de tipo *Lector de supervisión* en el nivel de suscripción. Asegúrese de que todos los usuarios de los paneles a los que ancla gráficos de varios recursos tienen permisos suficientes. Para más información, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

Para anclar el gráfico de varios recursos a un panel, consulte [Anclar a paneles](../essentials/metrics-charts.md#pinning-to-dashboards). 

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Explorador de métricas](../essentials/metrics-troubleshoot.md)
* [Vea una lista de métricas disponibles para servicios de Azure](./metrics-supported.md)
* [Vea ejemplos de gráficos configurados](../essentials/metric-chart-samples.md)