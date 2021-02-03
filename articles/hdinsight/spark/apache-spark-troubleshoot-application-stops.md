---
title: La aplicación Apache Spark Streaming se detiene después de 24 días en Azure HDInsight
description: Una aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días y no hay errores en los archivos de registro.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: b702cbf915e4991df4c202564677ea7e0a02f9c4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929469"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Escenario: La aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Una aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días y no hay errores en los archivos de registro.

## <a name="cause"></a>Causa

El valor `livy.server.session.timeout` controla el tiempo que Apache Livy debe esperar hasta que se complete una sesión. Cuando la duración de la sesión alcanza el valor de `session.timeout`, la sesión de Livy y la aplicación se terminan automáticamente.

## <a name="resolution"></a>Solución

Para los trabajos de larga duración, aumente el valor `livy.server.session.timeout` con la interfaz de usuario de Ambari. Puede acceder a la configuración de Livy desde la interfaz de usuario de Ambari mediante la dirección URL `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`.

Reemplace `<yourclustername>` por el nombre de su clúster de HDInsight que se muestra en el portal.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]