---
title: 'Optimización del uso de memoria en Apache Spark: Azure HDInsight'
description: Aprenda a optimizar el uso de la memoria en Apache Spark en Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/20/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 4e23c5977b2492d2ea8a7a8cc050c77c512c3e16
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104868382"
---
# <a name="memory-usage-optimization-for-apache-spark"></a>Optimización del uso de memoria para Apache Spark

En este artículo se describe cómo optimizar la administración de memoria del clúster de Apache Spark para obtener el mejor rendimiento en Azure HDInsight.

## <a name="overview"></a>Información general

Spark funciona mediante la colocación de datos en memoria, por lo que administrar los recursos de memoria es un aspecto importante de la optimización de la ejecución de trabajos de Spark.  Hay varias técnicas que puede aplicar para usar la memoria del clúster de manera eficaz.

* Elija particiones de datos más pequeñas y tenga en cuenta el tamaño de los datos, los tipos y la distribución en su estrategia de creación de particiones.
* Considere la [`Kryo data serialization`](https://github.com/EsotericSoftware/kryo) más reciente y más eficiente, en lugar de la serialización Java predeterminada.
* Elija el uso de YARN, dado que separa `spark-submit` por lote.
* Supervise y ajuste los valores de configuración de Spark.

Para que le sirva de referencia, en la imagen siguiente se muestran la estructura de memoria de Spark y algunos parámetros de memoria de ejecutores principales.

## <a name="spark-memory-considerations"></a>Consideraciones de memoria de Spark

Si usa Apache Hadoop YARN, YARN controla la memoria que usan todos los contenedores en cada nodo de Spark.  En el diagrama siguiente se muestran los objetos principales y sus relaciones.

:::image type="content" source="./media/apache-spark-perf/apache-yarn-spark-memory.png" alt-text="Administración de memoria de Spark para YARN" border="false":::

Para resolver los mensajes de falta de memoria, intente lo siguiente:

* Revise los órdenes aleatorios de administración de DAG. Reduzca en el lado de asignación, cree particiones previas (o depósitos) de los datos de origen, maximice los órdenes aleatorios únicos y reduzca la cantidad de datos enviados.
* Elija `ReduceByKey`, con su límite fijo de memoria, antes que `GroupByKey`, que proporciona agregaciones, ventanas y otras funciones, pero que tiene un límite de memoria no enlazado.
* Elija `TreeReduce`, que realiza más trabajo en los ejecutores o particiones, antes que `Reduce`, que hace todo el trabajo en el controlador.
* Use DataFrames en lugar de los objetos RDD de nivel inferior.
* Cree ComplexTypes que encapsulan acciones, como "N principales", diversas agregaciones u operaciones de ventanas.

Si necesita pasos de solución de problemas adicionales, consulte [Excepciones OutOfMemoryError para Apache Spark en Azure HDInsight](apache-spark-troubleshoot-outofmemory.md).

## <a name="next-steps"></a>Pasos siguientes

* [Optimización del procesamiento de datos para Apache Spark](optimize-cluster-configuration.md)
* [Optimización del almacenamiento de datos para Apache Spark](optimize-data-storage.md)
* [Optimización de la configuración de clústeres para Apache Spark](optimize-cluster-configuration.md)
