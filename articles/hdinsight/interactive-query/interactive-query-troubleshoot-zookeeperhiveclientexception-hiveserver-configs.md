---
title: 'Error del intérprete de Zeppelin para Apache Hive: Azure HDInsight'
description: El intérprete JDBC de Zeppelin para Apache Hive apunta a la dirección URL equivocada en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 2d6e660459d9f350cc5c63fb8d312bcbcb300a64
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98930750"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Escenario: El intérprete de Zeppelin para Apache Hive genera un error de Zookeeper en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

En un clúster de LLAP de Apache Hive, el intérprete de Zeppelin devuelve el siguiente mensaje de error al intentar ejecutar una consulta:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Causa

El intérprete JDBC de Hive de Zeppelin apunta a la dirección URL equivocada.

## <a name="resolution"></a>Solución

1. Navegue hasta el resumen de componente de Hive y copie la "Dirección URL de JDBC de Hive" en el portapapeles.

1. Vaya a `https://clustername.azurehdinsight.net/zeppelin/#/interpreter`.

1. Edite la configuración de JDBC: actualice el valor de Hive.url a la dirección URL de JDBC de Hive copiada en el paso 1.

1. Guarde y vuelva a intentar la consulta

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]