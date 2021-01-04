---
title: Visualización de varios recursos en el Explorador de métricas
description: Obtenga información sobre cómo visualizar varios recursos en el Explorador de métricas de Azure Monitor
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.subservice: metrics
ms.openlocfilehash: 724809dbce3ca1b5a36f4da0ba5c03d0f78897f5
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577582"
---
# <a name="viewing-multiple-resources-in-metrics-explorer"></a>Visualización de varios recursos en el Explorador de métricas

El selector del ámbito de recursos le permite ver las métricas en varios recursos que se encuentren en la misma suscripción y región. A continuación se proporcionan instrucciones sobre cómo ver varios recursos en el Explorador de métricas de Azure Monitor. 

## <a name="selecting-a-resource"></a>Seleccionar un recurso 

Seleccione **Métricas** en el menú de **Azure Monitor** o en la sección **Supervisión** del menú de un recurso. Haga clic en el botón "Seleccionar un ámbito" para abrir el selector del ámbito de recursos, que le permitirá seleccionar los recursos de los que quiera ver las métricas. El ámbito ya aparece rellenado al abrir el explorador de métricas desde el menú de un recurso. 

![Captura de pantalla del selector del ámbito de recursos resaltado en rojo](./media/metrics-charts/019.png)

## <a name="selecting-multiple-resources"></a>Selección de varios recursos 

Algunos tipos de recursos tienen habilitada la capacidad de consultar métricas en varios recursos, siempre y cuando se encuentren en la misma suscripción y ubicación. Estos tipos de recursos se pueden encontrar en la parte superior de la lista desplegable "Tipos de recursos". 

![Captura de pantalla que muestra una lista desplegable de los recursos compatibles con varios recursos. ](./media/metrics-charts/020.png)

> [!WARNING] 
> Debe tener el permiso de tipo Lector de supervisión en el nivel de suscripción para visualizar las métricas de varios recursos, grupos de recursos o de una suscripción. Para ello, siga las instrucciones de [este documento](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Para visualizar las métricas en varios recursos, seleccione varios recursos en el selector del ámbito de recursos. 

![Captura de pantalla que muestra cómo seleccionar varios recursos.](./media/metrics-charts/021.png)

> [!NOTE]
> Solo se pueden seleccionar varios recursos dentro del mismo tipo de recurso, ubicación y suscripción. Tenga en cuenta que los recursos que no cumplan este criterio no se podrán seleccionar. 

Cuando haya terminado de seleccionar los recursos, haga clic en el botón "Aplicar" para guardar la selección. 

## <a name="selecting-a-resource-group-or-subscription"></a>Selección de un grupo de recursos o suscripción 

> [!WARNING]
> Debe tener el permiso de tipo Lector de supervisión en el nivel de suscripción para visualizar las métricas de varios recursos, grupos de recursos o de una suscripción. 

En el caso de los tipos compatibles con varios recursos, también puede consultar las métricas en una suscripción o en varios grupos de recursos. Para comenzar, seleccione una suscripción o uno o varios grupos de recursos: 

![Captura de pantalla que muestra cómo realizar consultas en varios grupos de recursos. ](./media/metrics-charts/022.png)

A continuación, debe seleccionar un tipo de recurso y una ubicación para poder seguir aplicando el nuevo ámbito. 

![Captura de pantalla que muestra los grupos de recursos seleccionados. ](./media/metrics-charts/023.png)

También puede expandir los ámbitos seleccionados para comprobar los recursos a los que se aplicará esta opción.

![Captura de pantalla que muestra los recursos seleccionados dentro de los grupos. ](./media/metrics-charts/024.png)

Una vez que haya terminado de seleccionar los ámbitos, haga clic en "Aplicar" para guardar lo que haya seleccionado. 

## <a name="splitting-and-filtering-by-resource-group-or-resources"></a>División y filtrado por grupo de recursos o por recursos

Después de trazar los recursos, puede usar la herramienta de división y filtrado para obtener más información sobre los datos. 

La opción de división le permite visualizar el modo en que los distintos segmentos de la métrica se comparan entre sí. Por ejemplo, cuando se traza una métrica de varios recursos, puede usar la herramienta "Aplicar división" para dividir en función del id. del recurso o del grupo de recursos. Esto le permitirá comparar fácilmente una sola métrica entre varios recursos o grupos de recursos.  

Por ejemplo, a continuación se muestra un gráfico con un porcentaje de la CPU en 9 VM. Gracias a la división en función del id. de recurso, puede ver fácilmente el porcentaje de CPU que se diferencia por VM. 

![Captura de pantalla que muestra cómo puede usar la opción de división para ver el porcentaje de CPU por VM.](./media/metrics-charts/026.png)

Además de la división, puede usar la característica de filtrado para mostrar solo los grupos de recursos que quiera ver.  Por ejemplo, si quiere ver el porcentaje de la CPU de las VM de un determinado grupo de recursos, puede usar la herramienta "Agregar filtro" para filtrar por grupo de recursos. En este ejemplo, filtramos por el valor TailspinToys, que quitará las métricas asociadas a los recursos de TailspinToysDemo. 

![Captura de pantalla que muestra cómo se puede filtrar por grupo de recursos.](./media/metrics-charts/027.png)

## <a name="pinning-your-multi-resource-charts"></a>Anclaje de los gráficos de varios recursos 

> [!WARNING] 
> Debe tener el permiso de tipo Lector de supervisión en el nivel de suscripción para visualizar las métricas de varios recursos, grupos de recursos o de una suscripción. Para ello, siga las instrucciones de [este documento](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 

Para anclar el gráfico de varios recursos, siga las instrucciones que tiene [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts#create-alert-rules). 

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Explorador de métricas](metrics-troubleshoot.md)
* [Vea una lista de métricas disponibles para servicios de Azure](metrics-supported.md)
* [Vea ejemplos de gráficos configurados](metric-chart-samples.md)

