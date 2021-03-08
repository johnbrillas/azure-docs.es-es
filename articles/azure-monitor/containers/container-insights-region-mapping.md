---
title: Asignaciones de región de Container Insights
description: Se describen las asignaciones de región compatibles entre Container Insights, el área de trabajo de Log Analytics y las métricas personalizadas.
ms.topic: conceptual
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: f9e910b1352109608becb82609e85e26d27d2cd1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101728882"
---
# <a name="region-mappings-supported-by-container-insights"></a>Asignaciones de región admitidas por Container Insights

 Cuando se habilita Container Insights, solo se admiten determinadas regiones para vincular un área de trabajo de Log Analytics y un clúster de AKS, y recopilar las métricas personalizadas enviadas a Azure Monitor.

## <a name="log-analytics-workspace-supported-mappings"></a>Asignaciones admitidas por el área de trabajo de Log Analytics

Las regiones de AKS compatibles se indican en [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service). El área de trabajo de Log Analytics debe estar en la misma región, excepto en las regiones enumeradas en la tabla siguiente. Consulte [Notas de la versión de AKS](https://github.com/Azure/AKS/releases) para ver las actualizaciones.


|**Región de clúster de AKS** | **Región del área de trabajo de Log Analytics** |
|-----------------------|------------------------------------|
|**África** | |
|SouthAfricaNorth |Oeste de Europa |
|SouthAfricaWest |Oeste de Europa |
|**Australia** | |
|AustraliaCentral2 |AustraliaCentral |
|**Brasil** | |
|BrazilSouth | SouthCentralUS |
|**Canadá** ||
|CanadaEast |CanadaCentral |
|**Europa** | |
|FranceSouth |FranceCentral |
|UKWest |UKSouth |
|**India** | |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japón** | |
|JapanWest |JapanEast |
|**Corea** | |
|KoreaSouth |KoreaCentral |
|**EE. UU.** | |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|


<sup>1</sup> Debido a restricciones de capacidad, la región no está disponible cuando se crean recursos nuevos. Esto incluye un área de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

## <a name="custom-metrics-supported-regions"></a>Regiones compatibles con las métricas personalizadas

Las métricas recopiladas de los pods y los nodos de clústeres de Azure Kubernetes Service (AKS) se admiten para publicar como métricas personalizadas solo en las [regiones de Azure](../essentials/metrics-custom-overview.md#supported-regions) siguientes.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a supervisar el clúster de AKS, revise [Habilitar Azure Monitor para contenedores](container-insights-onboard.md), a fin de entender los requisitos y los métodos disponibles para habilitar la supervisión.  
