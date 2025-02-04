---
title: Descripción de Apache Spark para los desarrolladores de U-SQL de Azure Data Lake Analytics
description: En este artículo se describen los conceptos de Apache Spark para ayudarlo a entender las diferencias entre los desarrolladores de U-SQL.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.custom: understand-apache-spark-for-usql-developers
ms.date: 10/15/2019
ms.openlocfilehash: a66a82a6d2a5bb1f534ed211091793b2ab4d2a28
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92221101"
---
# <a name="understand-apache-spark-for-u-sql-developers"></a>Descripción de Apache Spark para desarrolladores de U-SQL

Microsoft admite varios servicios de análisis como [Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks) y [Azure HDInsight](../hdinsight/hdinsight-overview.md), así como Azure Data Lake Analytics. Los desarrolladores nos comentan que tienen una preferencia clara por las soluciones de código abierto cuando crean canalizaciones de análisis. Creamos esta guía para ayudar a los desarrolladores de U-SQL a comprender Apache Spark y para que sepa cómo se pueden transformar los scripts de U-SQL a Apache Spark.

Incluye una serie de pasos que puede realizar y varias alternativas.

## <a name="steps-to-transform-u-sql-to-apache-spark"></a>Pasos para transformar U-SQL en Apache Spark

1. Transforme las canalizaciones de orquestación de trabajos.

   Si usa [Azure Data Factory](../data-factory/introduction.md) para orquestar los scripts de Azure Data Lake Analytics, tendrá que ajustarlos para orquestar los nuevos programas de Spark.
2. Descripción de las diferencias entre cómo U-SQL y Spark administran los datos

   Si quiere migrar los datos de [Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md) a [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md), tendrá que copiar los datos del archivo y los datos mantenidos en el catálogo. Tenga en cuenta que Azure Data Lake Analytics solo admite Azure Data Lake Storage Gen1. Consulte [Descripción de los formatos de datos de Spark](understand-spark-data-formats.md).
3. Transformación de los scripts de U-SQL en Spark

   Antes de transformar los scripts de U-SQL, tendrá que elegir un servicio de análisis. Algunos de los servicios de proceso disponibles son:
      - [Flujos de datos de Azure Data Factory](../data-factory/concepts-data-flow-overview.md) Los flujos de datos de asignación son transformaciones de datos diseñadas de manera visual que permiten a los ingenieros de datos desarrollar una lógica de transformación de datos gráficos sin necesidad de escribir código. Aunque no son adecuados para ejecutar código de usuario complejo, pueden representar fácilmente transformaciones de flujo de datos similares a SQL tradicional.
      - [Hive de Azure HDInsight](../hdinsight/hadoop/apache-hadoop-using-apache-hive-as-an-etl-tool.md) Apache Hive en HDInsight es adecuado para las operaciones de extracción, transformación y carga (ETL). Esto significa que va a traducir los scripts de U-SQL a Apache Hive.
      - Motores de Apache Spark como [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) o [Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks) Esto significa que va a traducir los scripts de U-SQL a Spark. Para más información, consulte [Descripción de los formatos de datos de Spark](understand-spark-data-formats.md).

> [!CAUTION]
> Tanto [Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks) como [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) son servicios de clúster y no trabajos sin servidor como Azure Data Lake Analytics. Tendrá que tener en cuenta cómo aprovisionar los clústeres para obtener la relación de costo/rendimiento adecuada y cómo administrar su duración para minimizar los costos. Estos servicios tienen características de rendimiento distintas con el código de usuario escrito en .NET, por lo que tendrá que escribir contenedores o reescribir el código en un lenguaje compatible. Para más información, consulte [Descripción de los formatos de datos de Spark](understand-spark-data-formats.md), [Descripción de los conceptos de código de Apache Spark para los desarrolladores de U-SQL](understand-spark-code-concepts.md), [.NET para Apache Spark](https://dotnet.microsoft.com/apps/data/spark).

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de los formatos de datos de Spark para desarrolladores de U-SQL](understand-spark-data-formats.md)
- [Descripción de los conceptos de código de Spark para desarrolladores de U-SQL](understand-spark-code-concepts.md)
- [Actualización de las soluciones de análisis de macrodatos de Azure Data Lake Storage Gen1 a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-migrate-gen1-to-gen2.md)
- [.NET para Apache Spark](/dotnet/spark/what-is-apache-spark-dotnet)
- [Transformación de datos mediante la actividad de Hive de Hadoop en Azure Data Factory](../data-factory/transform-data-using-hadoop-hive.md)
- [Transformación de datos mediante la actividad de Spark en Azure Data Factory](../data-factory/transform-data-using-spark.md)
- [Qué es Apache Spark en Azure HDInsight](../hdinsight/spark/apache-spark-overview.md)