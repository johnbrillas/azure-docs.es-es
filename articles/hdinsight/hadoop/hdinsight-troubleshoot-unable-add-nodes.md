---
title: No se pueden agregar nodos a un clúster Azure HDInsight
description: Solución de los problemas por los que no se pueden agregar nodos al clúster de Apache Hadoop en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/31/2019
ms.openlocfilehash: b11d1edef2f3a6fa0fb39c76d1f25ec05ff15d07
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944333"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Escenario: No se pueden agregar nodos a un clúster Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se pueden agregar nodos al clúster de Azure HDInsight.

## <a name="cause"></a>Causa

Los motivos pueden variar.

## <a name="resolution"></a>Solución

Mediante la característica [Tamaño del clúster](../hdinsight-scaling-best-practices.md), calcule el número de núcleos adicionales necesarios para el clúster. La cifra se basa en el número total de núcleos de los nuevos nodos de trabajo. Luego, pruebe uno o varios de los pasos siguientes:

* Compruebe si hay algún núcleo disponible en la ubicación del clúster.

* Compruebe los núcleos disponibles en otras ubicaciones. Considere la posibilidad de volver a crear el clúster en otra ubicación con suficientes núcleos disponibles.

* Si desea aumentar la cuota de núcleos para una determinada ubicación, registre una incidencia de soporte técnico para solicitar el aumento de la cuota de núcleos de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]