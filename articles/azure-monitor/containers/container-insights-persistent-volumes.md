---
title: Configuración de la supervisión de VP con Container insights | Microsoft Docs
description: En este artículo se describe cómo puede configurar la supervisión de clústeres de Kubernetes con volúmenes persistentes mediante Container Insights.
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 0afbeab49a6909a0011cd75a0419f7325ca68132
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713735"
---
# <a name="configure-pv-monitoring-with-container-insights"></a>Configuración de la supervisión de VP con Container insights

A partir de la versión del agente *ciprod10052020*, el agente integrado de Container Insights ahora admite la supervisión del uso de VP (volúmenes persistentes).

## <a name="pv-metrics"></a>Métricas de VP

Container Insights inicia automáticamente la supervisión de los VP mediante la recopilación de las métricas siguientes a intervalos de 60 segundos y su almacenamiento en la tabla **InsightMetrics**.

|Nombre de métrica |Dimensión de métrica (etiquetas) |Descripción |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|Espacio usado en bytes para un volumen persistente específico con una notificación utilizada por un pod concreto. `pvCapacityBytes` se pliega como una dimensión en el campo Etiquetas para reducir el costo de ingesta de datos y simplificar las consultas.|

## <a name="monitor-persistent-volumes"></a>Supervisión de volúmenes persistentes

Container Insights incluye gráficos preconfigurados para esta métrica en un libro para cada clúster. Puede encontrar los gráficos en la pestaña Volumen persistente del libro **Detalles de la carga de trabajo** directamente desde un clúster de AKS. Para ello, seleccione Libros en el panel de la izquierda y en la lista desplegable **Ver libros** de Conclusión. También puede habilitar una alerta recomendada para el uso de VP, así como consultar estas métricas en Log Analytics.  

![Ejemplo de libro de cargas de trabajo de VP de Azure Monitor](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las métricas de VP recopiladas [aquí](./container-insights-agent-config.md).
