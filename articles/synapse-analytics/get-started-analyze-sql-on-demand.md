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
ms.openlocfilehash: 5f0a7477df2e281748c053ea8c7e7d3e79626296
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104588025"
---
# <a name="analyze-data-with-a-serverless-sql-pool"></a>Análisis de datos con un grupo de SQL sin servidor

En este tutorial, aprenderá a analizar datos con un grupo de SQL sin servidor mediante los datos ubicados en bases de datos de Spark. 

## <a name="the-built-in-serverless-sql-pool"></a>El grupo de SQL sin servidor integrado

Los grupos de SQL sin servidor permiten usar SQL sin necesidad de reservar capacidad. La facturación de un grupo de SQL sin servidor se basa en la cantidad de datos procesados para ejecutar la consulta y no en el número de recursos usados para ello.

Todas las áreas de trabajo incluyen un grupo de SQL sin servidor preconfigurado llamado **Built-in**. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Análisis de los datos de taxis de Nueva York en Blob Storage mediante un grupo de SQL sin servidor

En esta sección, usará un grupo de SQL sin servidor para analizar los datos de taxis de Nueva York en una cuenta de Azure Blob Storage.

1. En Synapse Studio, vaya al menú central **Develop** (Desarrollo).
1. Se creará un nuevo script de SQL.
1. Pegue el código siguiente en el script.

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. Haga clic en **Ejecutar**

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de Spark sin servidor](get-started-analyze-spark.md)
