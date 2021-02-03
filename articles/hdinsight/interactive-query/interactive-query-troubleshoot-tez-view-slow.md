---
title: Apache Ambari Tez View se carga lentamente en Azure HDInsight
description: Apache Ambari Tez View puede cargarse lentamente o no cargarse en absoluto en Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 4fe66b3104be0351a9b0e1df6b6545f71ff276ab
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98930763"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>Escenario: Apache Ambari Tez View se carga lentamente en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Apache Ambari Tez View puede cargarse lentamente o no cargarse en absoluto. Al cargar Ambari Tez View, es posible que los procesos de nodos principales dejen de responder.

## <a name="cause"></a>Causa

El acceso a API de ATS para Yarn a veces puede tener un rendimiento deficiente en los clústeres creados antes de octubre de 2017, cuando el clúster tiene un gran número de trabajos de Hive ejecutados en él.

## <a name="resolution"></a>Solución

Se trata de un problema que se ha corregido en octubre de 2017. Si se vuelve a crear el clúster, el problema dejará de aparecer.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]