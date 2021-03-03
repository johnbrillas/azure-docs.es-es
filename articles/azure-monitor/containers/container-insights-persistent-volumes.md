---
title: Configuración de la supervisión de VP con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede configurar la supervisión de clústeres de Kubernetes con volúmenes persistentes mediante Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: d7da6bc88e7c8526e3940714502d3c92d2f37dd8
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100604837"
---
# <a name="configure-pv-monitoring-with-azure-monitor-for-containers"></a>Configuración de la supervisión de VP con Azure Monitor para contenedores

A partir de la versión del agente *ciprod10052020*, el agente integrado de Azure Monitor para contenedores ahora admite la supervisión del uso de VP (volúmenes persistentes).

## <a name="pv-metrics"></a>Métricas de VP

Azure Monitor para contenedores inicia automáticamente la supervisión de VP mediante la recopilación de las métricas siguientes a intervalos de 60 segundos y las almacena en la tabla **InsightMetrics**.

|Nombre de métrica |Dimensión de métrica (etiquetas) |Descripción |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|Espacio usado en bytes para un volumen persistente específico con una notificación utilizada por un pod concreto. `pvCapacityBytes` se pliega como una dimensión en el campo Etiquetas para reducir el costo de ingesta de datos y simplificar las consultas.|

## <a name="monitor-persistent-volumes"></a>Supervisión de volúmenes persistentes

Azure Monitor para contenedores incluye gráficos preconfigurados para esta métrica en un libro para cada clúster. Puede encontrar los gráficos en la pestaña Volumen persistente del libro **Detalles de la carga de trabajo** directamente desde un clúster de AKS. Para ello, seleccione Libros en el panel de la izquierda y en la lista desplegable **Ver libros** de Conclusión. También puede habilitar una alerta recomendada para el uso de VP, así como consultar estas métricas en Log Analytics.  

![Ejemplo de libro de cargas de trabajo de VP de Azure Monitor](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las métricas de VP recopiladas [aquí](./container-insights-agent-config.md).
