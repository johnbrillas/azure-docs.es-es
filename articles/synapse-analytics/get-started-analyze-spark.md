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
ms.date: 12/31/2020
ms.openlocfilehash: 7410fcbc890780281763a91f33525562e9de1853
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182492"
---
# <a name="analyze-with-apache-spark"></a>Análisis con Apache Spark

En este tutorial, aprenderá los pasos básicos para cargar y analizar datos con Apache Spark para Azure Synapse.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Análisis de los datos de taxis de Nueva York en Blob Storage con Spark

1. En el centro **Datos**, haga clic en el botón **+** para **agregar un nuevo recurso** y, luego, haga clic en **Examinar la galería**. 
1. Busque el conjunto de datos **NYC Taxi & Limousine Commission - yellow taxi trip records** y haga clic en él. 
1. En la parte inferior de la página, presione **Continuar** y, luego, **Agregar conjunto de datos**. 
1. Al cabo de un momento, en el centro **Datos**, en **Vinculado**, haga clic con el botón derecho en **Azure Blob Storage > > Sample Datasets (Conjuntos de datos de ejemplo) > > nyc_tlc_yellow** y seleccione **Nuevo cuaderno** y, luego, **Load to Data Frame** (Cargar en Dataframe).
1. Así se creará un cuaderno con el código siguiente:
    ```

    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    df = data.to_spark_dataframe()
    # Display 10 rows
    display(df.limit(10))
    ```
1. En el cuaderno, en el menú **Attach to** (Asociar a), elija el grupo de Spark sin servidor **Spark1** que se creó anteriormente.
1. Seleccione **Ejecutar** en la celda.
1. Si solo desea ver el esquema de la trama de datos, ejecute una celda con el siguiente código:
    ```

    df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Carga de los datos de los taxis de Nueva York en la base de datos nyctaxi de Spark

Los datos están disponibles en una tabla en **SQLPOOL1**. Cárguelos en una base de datos de Spark denominada **nyctaxi**.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo).
1. Seleccione **+**  > **Cuaderno**.
1. En la parte superior del cuaderno, establezca el valor de **Adjuntar a** en **Spark1**.
1. En la primera celda de código del nuevo cuaderno, escriba el código siguiente:


    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```


1. Ejecute el script. Esta operación puede tardar entre 2 y 3 minutos.
1. En el centro **Datos**, en la pestaña **Área de trabajo**, haga clic con el botón derecho en **Bases de datos** y seleccione **Actualizar**. Ahora debería ver la base de datos **nyctaxi (Spark)** en la lista.


## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Análisis de los datos de los taxis de Nueva York mediante Spark y cuadernos

1. Vuelva al cuaderno.
1. Cree una celda de código y escriba el código siguiente. 


   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Ejecute la celda para que se muestren los datos de taxis de Nueva York que cargamos en la base de datos **nyctaxi** de Spark.
1. Cree una celda de código y escriba el código siguiente. Luego, ejecute la celda para realizar el mismo análisis que hicimos anteriormente con el grupo de SQL **SQLPOOL1** dedicado. Este código guarda y muestra los resultados del análisis en una tabla llamada **nyctaxi.passengercountstats**.


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

Anteriormente copiamos datos de la tabla de grupos de SQL dedicados **SQLPOOL1.dbo.Trip** en la tabla de Spark **nyctaxi.trip**. A continuación, agregó los datos a la tabla de Spark **nyctaxi.passengercountstats**. Ahora se copiarán los datos de **nyctaxi.passengercountstats** en una tabla de grupos de SQL dedicados denominada **SQLPOOL1.dbo.PassengerCountStats**.

1. Cree una celda de código y escriba el código siguiente. Ejecute la celda en el cuaderno. Esta copia la tabla de Spark agregada en la tabla de grupos de SQL dedicados.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de SQL sin servidor](get-started-analyze-sql-on-demand.md)
