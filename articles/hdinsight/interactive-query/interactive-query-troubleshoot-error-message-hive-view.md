---
title: 'No aparece el mensaje de error en la vista de Apache Hive: Azure HDInsight'
description: Se produce un error de consulta en la vista de Apache Hive sin ningún detalle en el clúster de Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: c60e06e8f37e7aed0d0a0df661690a2b1f32dbd5
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98930998"
---
# <a name="scenario-query-error-message-not-displayed-in-apache-hive-view-in-azure-hdinsight"></a>Escenario: Mensaje de error de consulta no mostrado en la vista de Apache Hive en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El mensaje de error en la consulta de la vista de Apache Hive tendrá un aspecto similar al siguiente, sin más información:

```
"Failed to execute query. <a href="#/messages/1">(details)</a>"
```

## <a name="cause"></a>Causa

A veces, el mensaje de error en la consulta puede ser demasiado grande para mostrarse en la página principal de la vista de Hive.

## <a name="resolution"></a>Solución

Consulte la pestaña Notificaciones en la esquina superior derecha de la vista de Hive para ver el mensaje de error y StackTrace completos.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]