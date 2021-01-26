---
title: 'Tutorial: Introducción al análisis de datos con un grupo de SQL sin servidor'
description: En este tutorial, aprenderá a analizar datos con un grupo de SQL sin servidor mediante los datos ubicados en bases de datos de Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: c9f8760bd1a7b5d3700f3fdf03331fe7013e116f
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209414"
---
# <a name="analyze-data-with-serverless-sql-pool-in-azure-synapse-analytics"></a>Análisis de datos con un grupo de SQL sin servidor en Azure Synapse Analytics

En este tutorial, aprenderá a analizar datos con un grupo de SQL sin servidor mediante los datos ubicados en bases de datos de Spark. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Análisis de los datos de taxis de Nueva York en Blob Storage mediante un grupo de SQL sin servidor

1. En el centro de conectividad **Data** (Datos) de **Linked** (Vinculados), haga clic con el botón derecho en **Azure Blob Storage > Sample Datasets > nyc_tlc_yellow** (Azure Blob Storage > Conjuntos de datos de ejemplo > nyc_tlc_yellow), **New SQL Script** (Nuevo script SQL) y seleccione **SELECT TOP 100 rows** (SELECCIONAR 100 PRIMERAS filas).
1. Así se creará un script de SQL con el código siguiente:

    ```
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. Haga clic en **Ejecutar**

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-serverless-sql-pool"></a>Análisis de los datos de taxis de Nueva York en las bases de datos de Spark mediante un grupo de SQL sin servidor

Las tablas de las bases de datos de Spark se ven automáticamente y se pueden consultar mediante un grupo de SQL sin servidor.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo) y cree un script de SQL.
1. Establezca **Conectarse a** en el grupo de SQL sin servidor **integrado**.
1. Pegue el siguiente texto en el script y ejecute el script.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > La primera vez que se ejecuta una consulta que usa un grupo de SQL sin servidor dicho grupo tarda alrededor de 10 segundos en recopilar los recursos de SQL necesarios para ejecutar las consultas. Las consultas posteriores serán mucho más rápidas.
  


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos en Storage](get-started-analyze-storage.md)
