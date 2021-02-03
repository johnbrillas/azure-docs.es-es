---
title: Optimización de trabajos de Spark para mejorar el rendimiento en Azure HDInsight
description: Se muestran estrategias comunes para obtener el mejor rendimiento de los clústeres de Apache Spark en Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/21/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 567fc2637538408d9727d07d3185a5b0e3cf20c5
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929941"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>Optimización de trabajos de Apache Spark en HDInsight

En este artículo se proporciona información general sobre las estrategias para optimizar trabajos Apache Spark en Azure HDInsight.

## <a name="overview"></a>Información general

El rendimiento de los trabajos de Apache Spark depende de varios factores. Estos factores de rendimiento incluyen: cómo se almacenan los datos, cómo se configura el clúster y las operaciones que se usan al procesar los datos.

Entre las dificultades más comunes que pueden surgir se incluyen las restricciones de memoria debido al tamaño incorrecto de los ejecutores, las operaciones de larga duración y las tareas que dan lugar a operaciones cartesianas.

También hay muchas optimizaciones que pueden ayudarle a superar estas dificultades, como el almacenamiento en caché y la posibilidad de sesgar los datos.

En cada uno de los siguientes artículos, puede encontrar información sobre diferentes aspectos de la optimización de Spark.

* [Optimización del almacenamiento de datos para Apache Spark](optimize-data-storage.md)
* [Optimización del procesamiento de datos para Apache Spark](optimize-data-processing.md)
* [Optimización del uso de la memoria para Apache Spark](optimize-memory-usage.md)
* [Optimización de la configuración de clústeres de HDInsight para Apache Spark](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Pasos siguientes

* [Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight](apache-spark-job-debugging.md)
* [Administración de recursos de un clúster Apache Spark en Azure HDInsight](apache-spark-resource-manager.md)
* [Configuración de opciones de Apache Spark](apache-spark-settings.md)
