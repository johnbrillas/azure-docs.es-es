---
title: 'Inicio rápido: Introducción al análisis con Spark'
description: En este tutorial aprenderá a analizar datos con Apache Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 3b5f5d64498922e9fc35942ff4570d801aa6c516
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118887"
---
# <a name="analyze-with-apache-spark"></a>Análisis con Apache Spark

En este tutorial, aprenderá los pasos básicos para cargar y analizar datos con Apache Spark para Azure Synapse.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Análisis de los datos de taxis de Nueva York en Blob Storage con Spark

1. En el centro de conectividad **Data** (Datos), seleccione **Add a new resource** (Agregar un nuevo recurso) (botón con el signo más encima de **Linked** [Vinculados]) >> **Browse gallery** (Examinar la galería).
1. Seleccione **NYC Taxi & Limousine Commission - yellow taxi trip records**.
1. En la parte inferior de la página, seleccione **Continue** > **Add dataset** (Continuar > Agregar conjunto de datos).
1. En el centro de conectividad **Data** (Datos) de **Linked** (Vinculados), haga clic con el botón derecho en **Azure Blob Storage** y, a continuación, seleccione **Sample Datasets** > **nyc_tlc_yellow** (Conjuntos de datos de ejemplo > nyc_tlc_yellow).
1. Seleccione **New notebook** (Nuevo cuaderno) para crear un nuevo cuaderno con el siguiente código:

    ```py
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```

1. Elija un grupo de Spark sin servidor en el menú **Attach to** (Adjuntar a) del cuaderno.
1. Seleccione **Run** (Ejecutar) en la celda.
1. Si solo desea ver el esquema de la trama de datos, ejecute una celda con el siguiente código:

    ```py
    data_df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Carga de los datos de los taxis de Nueva York en la base de datos nyctaxi de Spark

Los datos están disponibles en una tabla en **SQLPOOL1**. Cárguelos en una base de datos de Spark denominada **nyctaxi**.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo).
1. Seleccione **+**  > **Cuaderno**.
1. En la parte superior del cuaderno, establezca el valor de **Adjuntar a** en **Spark1**.
1. Seleccione **Agregar código** (Agregar código) para agregar una celda de código del cuaderno y, a continuación, escriba el siguiente texto:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Seleccione **Run** (Ejecutar) en la celda.
1. En el centro de conectividad **Data** (Datos), haga clic con el botón derecho en **Databases** (Bases de datos) y seleccione **Refresh** (Actualizar). Debería ver estas bases de datos:

    - **SQLPOOL1 (SQL)**
    - **nyctaxi (Spark)**

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Análisis de los datos de los taxis de Nueva York mediante Spark y cuadernos

1. Vuelva al cuaderno.
1. Cree una nueva celda de código y escriba el texto siguiente.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Ejecute la celda para que se muestren los datos de taxis de Nueva York que cargó en la base de datos **nyctaxi** de Spark.
1. Ejecute el código siguiente para realizar el mismo análisis que hizo anteriormente con el grupo de SQL dedicado **SQLPOOL1**. Este código guarda y muestra los resultados del análisis en una tabla llamada **nyctaxi.passengercountstats**.

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. En los resultados de la celda, seleccione **Chart** (Gráfico) para ver los datos visualizados.

## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>Carga de datos de una tabla de Spark en una tabla de grupos de SQL dedicado

Anteriormente copió datos de la tabla del grupo de SQL dedicado **SQLPOOL1.dbo.Trip** en la tabla de Spark **nyctaxi.trip**. A continuación, agregó los datos a la tabla de Spark **nyctaxi.passengercountstats**. Ahora va a copiar los datos de **nyctaxi.passengercountstats** en una tabla de un grupo de SQL dedicado llamada **SQLPOOL1.dbo.PassengerCountStats**.

Ejecute la siguiente celda en el cuaderno. Esta copia la tabla de Spark agregada en la tabla de grupos de SQL dedicados.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de SQL sin servidor](get-started-analyze-sql-on-demand.md)
