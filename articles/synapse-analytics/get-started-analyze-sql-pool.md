---
title: 'Tutorial: Introducción al análisis de datos con grupos de SQL dedicados'
description: En este tutorial, usará los datos de ejemplo de NYC Taxi para explorar las funcionalidades de análisis del grupo de SQL.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 03/18/2020
ms.openlocfilehash: f03fa84c02c4b3894efe069289b0ecbb9e90dfdb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654635"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Análisis de datos con grupos de SQL dedicados

En este tutorial usará los datos de NYC Taxi para explorar las funcionalidades de un grupo de SQL dedicado.

## <a name="create-a-dedicated-sql-pool"></a>Creación de un grupo de SQL dedicado

1. En Synapse Studio, en el panel izquierdo, seleccione **Administrar** > **Grupos de SQL**.
1. Seleccione **Nuevo paso**.
1. En **Nombre del grupo de SQL** seleccione **SQLPOOL1**
1. En **Nivel de rendimiento** seleccione **DW100C**
1. Seleccione **Revisar y crear** > **Crear**. El grupo de SQL dedicado estará listo en unos minutos. 

El grupo de SQL dedicado se asocia con una base de datos SQL, que también se denomina **SQLPOOL1**.
1. Vaya a **Datos** > **Área de trabajo**.
1. Debería ver una base de datos denominada **SQLPOOL1**. Si no la ve, haga clic en **Refresh** (Actualizar).

Los grupos de SQL dedicados consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

> [!NOTE] 
> Al crear un nuevo grupo de SQL dedicado (anteriormente SQL DW) en el área de trabajo, se abrirá la página de aprovisionamiento del grupo de SQL dedicado. El aprovisionamiento se llevará a cabo en el servidor SQL lógico.
## <a name="load-the-nyc-taxi-data-into-sqlpool1"></a>Carga de los datos NYC Taxi en SQLPOOL1

1. En Synapse Studio, vaya al centro de **desarrollo**, haga clic en el botón **+** para agregar un nuevo recurso y cree un nuevo script SQL.
1. Seleccione el grupo "SQLPOOL1" (creado en el [paso 1](./get-started-create-workspace.md) de este tutorial) en la sección "Conectar a" del script.
1. Escriba el siguiente código:
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Haga clic en el botón Ejecutar para ejecutar el script.
1. Este script finalizará en menos de 60 segundos. Carga dos millones filas de datos de NYC Taxi en una tabla denominada **dbo.Trip**.

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Exploración de los datos de NYC Taxi en el grupo de SQL dedicado

1. En Synapse Studio, vaya al centro **Data** (Datos).
1. Vaya a **SQLPOOL1** > **Tablas**. 
3. Haga clic con el botón derecho en la tabla **dbo.Trip** y seleccione **New SQL Script** > **Select TOP 100 Rows** (Nuevo script SQL > Seleccionar 100 primeras filas).
4. Espere mientras se crea un nuevo script de SQL y se ejecuta.
5. Observe que en la parte superior del script de SQL, en **Connect to** (Conectar a) está seleccionado automáticamente el grupo de SQL llamado **SQLPOOL1**.
6. Reemplace el texto del script de SQL por este código y ejecútelo.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount;
    ```

    En esta consulta se muestra la relación entre las distancias recorridas totales y la distancia media recorrida, y el número de pasajeros.
1. En la ventana de resultados del script de SQL, cambie el valor de **View** (Vista) a **Chart** (Gráfico) para ver los resultados en un gráfico de líneas.
    
    > [!NOTE]
    > Un área de trabajo habilitada para un grupo de SQL dedicado (anteriormente SQL DW) se identifica mediante la información sobre herramientas del centro de datos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos en una cuenta de Azure Storage](get-started-analyze-storage.md)
