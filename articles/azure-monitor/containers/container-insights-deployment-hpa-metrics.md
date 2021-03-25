---
title: Implementación y métricas de HPA con Container Insights | Microsoft Docs
description: En este artículo se describe qué métricas de implementación y HPA (Horizontal Pod Autoscaler) se recopilan con Container Insights.
ms.topic: conceptual
ms.date: 08/09/2020
ms.openlocfilehash: c8bb100b756ea92d73e1c3a698f119b4f8157930
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101717713"
---
# <a name="deployment--hpa-metrics-with-container-insights"></a>Implementación y métricas de HPA con Container Insights

A partir de la versión de agente *ciprod08072020*, el agente integrado de Container Insights ahora recopila métricas para las implementaciones y las instancias de HPA.

## <a name="deployment-metrics"></a>Métricas de implementación

Container Insights inicia automáticamente la supervisión de las implementaciones mediante la recopilación de las métricas siguientes a intervalos de 60 segundos y su almacenamiento en la tabla **InsightMetrics**:

|Nombre de métrica |Dimensión de métrica (etiquetas) |Descripción |
|------------|------------------------|------------|
|kube_deployment_status_replicas_ready |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, deployment, deploymentStrategy, k8sNamespace, spec_replicas, status_replicas_available, status_replicas_updated (status.updatedReplicas) | Número total de pods preparados que esta implementación tiene como destino (status.readyReplicas). A continuación se muestran las dimensiones de esta métrica. <ul> <li> deployment: nombre de la implementación. </li> <li> k8sNamespace: espacio de nombres de Kubernetes para la implementación. </li> <li> deploymentStrategy: estrategia de implementación que se usa para reemplazar los pods por otros nuevos (spec.strategy.type).</li><li> creationTime: marca de tiempo de creación de la implementación. </li> <li> spec_replicas: número de pods deseados (spec.replicas). </li> <li>status_replicas_available: número total de pods disponibles (preparados durante al menos minReadySeconds) que esta implementación tiene como destino (status.availableReplicas).</li><li>status_replicas_updated: número total de pods no terminados que esta implementación tiene como destino y que tienen la especificación de plantilla deseada (status.updatedReplicas). </li></ul>|

## <a name="hpa-metrics"></a>Métricas de HPA

Container Insights inicia automáticamente la supervisión de las instancias de HPA mediante la recopilación de las métricas siguientes a intervalos de 60 s y su almacenamiento en la tabla **InsightMetrics**:

|Nombre de métrica |Dimensión de métrica (etiquetas) |Descripción |
|------------|------------------------|------------|
|kube_hpa_status_current_replicas |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, hpa, k8sNamespace, lastScaleTime, spec_max_replicas, spec_min_replicas, status_desired_replicas, targetKind, targetName | Número actual de réplicas de pods administradas por este escalador automático (status.currentReplicas). A continuación se muestran las dimensiones de esta métrica. <ul> <li> hpa: nombre de la instancia de HPA. </li> <li> k8sNamespace: espacio de nombres de Kubernetes para la instancia de HPA. </li> <li> lastScaleTime: última vez que la instancia de HPA escaló el número de pods (status.lastScaleTime).</li><li> creationTime: marca de tiempo de creación de la instancia de HPA. </li> <li> spec_max_replicas: límite superior del número de pods que puede establecer el escalador automático (spec.maxReplicas). </li> <li> spec_min_replicas: límite inferior del número de réplicas al que se puede reducir verticalmente el escalador automático (spec.minReplicas). </li><li>status_desired_replicas: número deseado de réplicas de pods administradas por este escalador automático (status.desiredReplicas).</li><li>targetKind: tipo del destino de la instancia de HPA (spec.scaleTargetRef.kind). </li><li>targetName: nombre del destino de la instancia de HPA (spec.scaleTargetRef.name). </li></ul>|

## <a name="deployment--hpa-charts"></a>Gráficos de implementación y HPA 

Container Insights incluye gráficos preconfigurados para las métricas indicadas anteriormente en la tabla como libro para cada clúster. Puede encontrar el libro de implementaciones y HPA **Implementaciones y HPA** directamente de un clúster de AKS. Para ello, seleccione **Libros** en el panel izquierdo y después selecciónelo en la lista desplegable **Ver libros** de Conclusión.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Métricas de estado de Kube en Kubernetes](https://github.com/kubernetes/kube-state-metrics/tree/master/docs) para obtener más información sobre las métricas de estado de Kube.