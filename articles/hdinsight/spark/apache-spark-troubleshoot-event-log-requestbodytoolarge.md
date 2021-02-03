---
title: 'Error de RequestBodyTooLarge de la aplicación de Apache Spark: Azure HDInsight'
description: NativeAzureFileSystem ... RequestBodyTooLarge aparece en el registro de la aplicación de streaming de Apache Spark en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: 73ae646cb083841ee1d55b6c7ce6af7180cef08e
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929438"
---
# <a name="nativeazurefilesystemrequestbodytoolarge-appear-in-apache-spark-streaming-app-log-in-hdinsight"></a>"NativeAzureFileSystem...RequestBodyTooLarge" aparecen en el registro de las aplicaciones de Apache Spark Streaming en HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El error `NativeAzureFileSystem ... RequestBodyTooLarge` aparece en el registro de controladores para una aplicación de streaming de Apache Spark.

## <a name="cause"></a>Causa

El archivo de registro de eventos de Spark probablemente alcanza el límite de longitud de archivos de WASB.

En Spark 2.3, cada aplicación de Spark genera un archivo de registro de eventos de Spark. El archivo del registro de eventos de Spark correspondiente a una aplicación de streaming de Spark sigue creciendo mientras la aplicación se está ejecutando. En la actualidad, un archivo en WASB tiene un límite de bloque de 50 000 y el tamaño de bloque predeterminado es de 4 MB. Por lo tanto, en la configuración predeterminada, el tamaño máximo del archivo es de 195 GB. Pero Azure Storage ha aumentado el tamaño máximo de bloque a 100 MB, lo que establece el límite de un solo archivo en 4,75 TB. Para más información, consulte [Objetivos de escalabilidad y rendimiento de Blob Storage](../../storage/blobs/scalability-targets.md).

## <a name="resolution"></a>Solución

Hay tres soluciones disponibles para este error:

* Aumente el tamaño del bloque hasta 100 MB. En la interfaz de usuario de Ambari, modifique la propiedad de configuración `fs.azure.write.request.size` de HDFS (o créela en la sección `Custom core-site`). Establezca la propiedad en un valor mayor, por ejemplo: 33554432. Guarde la configuración actualizada y reinicie los componentes afectados.

* Detenga y vuelva a enviar periódicamente el trabajo de streaming de Spark.

* Use HDFS para almacenar los registros de eventos de Spark. Si usa HDFS en el almacenamiento puede provocar la pérdida de datos de eventos de Spark durante el escalado de clústeres o las actualizaciones de Azure.

    1. Realice cambios en `spark.eventlog.dir` y `spark.history.fs.logDirectory` a través de la interfaz de usuario de Ambari:

        ```
        spark.eventlog.dir = hdfs://mycluster/hdp/spark2-events
        spark.history.fs.logDirectory = "hdfs://mycluster/hdp/spark2-events"
        ```

    1. Cree directorios en HDFS:

        ```
        hadoop fs -mkdir -p hdfs://mycluster/hdp/spark2-events
        hadoop fs -chown -R spark:hadoop hdfs://mycluster/hdp
        hadoop fs -chmod -R 777 hdfs://mycluster/hdp/spark2-events
        hadoop fs -chmod -R o+t hdfs://mycluster/hdp/spark2-events
        ```

    1. Reinicie todos los servicios afectados a través de la interfaz de usuario de Ambari.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]