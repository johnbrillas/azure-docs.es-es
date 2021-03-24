---
title: Uso de la repartición para optimizar los trabajos de Azure Stream Analytics
description: En este artículo se describe cómo usar la creación de particiones para optimizar los trabajos de Azure Stream Analytics que no se pueden ejecutar en paralelo.
ms.service: stream-analytics
author: sidramadoss
ms.author: sidram
ms.date: 03/04/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 95749f2acea6b605cfdba5a4f3d4f5526e751c5a
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182543"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Vuelva a crear particiones para optimizar el procesamiento con Azure Stream Analytics

En este artículo se muestra cómo usar la creación de particiones para escalar la consulta de Azure Stream Analytics en escenarios que no se pueden [ejecutar completamente en paralelo](stream-analytics-scale-jobs.md).

Es posible que no pueda usar la [paralelización](stream-analytics-parallelization.md) si:

* No controla la clave de partición para el flujo de entrada.
* El origen divide la entrada en varias particiones que se deben combinar posteriormente.

Se requiere volver a crear particiones, o remodelar, cuando se procesan los datos de una secuencia que no está particionada de acuerdo con un esquema de entrada natural, como **PartitionId** para Event Hubs. Al volver a particionar, cada partición se puede procesar de forma independiente, lo que le permite escalar horizontalmente la canalización de streaming. 

## <a name="how-to-repartition"></a>Cómo volver a crear particiones
Puede volver a particionar la entrada de dos maneras:
1. Usar un trabajo de Stream Analytics independiente que realiza la nueva creación de particiones
2. Usar un solo trabajo, pero realizar primero la nueva creación de particiones antes de la lógica de análisis personalizada

### <a name="creating-a-separate-stream-analytics-job-to-repartition-input"></a>Creación de un trabajo de Stream Analytics independiente para volver a particionar la entrada
Puede crear un trabajo que lea la entrada y escriba en una salida del centro de eventos mediante una clave de partición. Este centro de eventos puede servir como entrada para otro trabajo de Stream Analytics en el que se implementa la lógica de análisis. Al configurar esta salida del centro de eventos en el trabajo, debe especificar la clave de partición por la que Stream Analytics volverá a particionar los datos. 
```sql
-- For compat level 1.2 or higher
SELECT * 
INTO output
FROM input

--For compat level 1.1 or lower
SELECT *
INTO output
FROM input PARTITION BY PartitionId
```

### <a name="repartition-input-within-a-single-stream-analytics-job"></a>Creación de una nueva partición de la entrada en un único trabajo de Stream Analytics
También puede introducir un paso en la consulta que primero vuelva a particionar la entrada y que se pueda usar en otros pasos de la consulta. Por ejemplo, si quiere volver a particionar la entrada en función de **DeviceId**, la consulta sería:
```sql
WITH RepartitionedInput AS 
( 
SELECT * 
FROM input PARTITION BY DeviceID
)

SELECT DeviceID, AVG(Reading) as AvgNormalReading  
INTO output
FROM RepartitionedInput  
GROUP BY DeviceId, TumblingWindow(minute, 1)  
```

En la consulta de ejemplo siguiente se combinan dos flujos de datos con particiones. Al combinar dos flujos de datos con particiones, los flujos deben tener la misma clave de partición y el mismo número. El resultado es un flujo que tiene el mismo esquema de partición.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

El esquema de salida debe coincidir con el número y la clave del esquema de flujo para que cada subflujo se pueda vaciar de forma independiente. El flujo también se podría combinar y volver a particionar de nuevo mediante un esquema diferente antes del vaciado, pero debe evitar ese método porque se suma a la latencia general del procesamiento y aumenta el uso de los recursos.

## <a name="streaming-units-for-repartitions"></a>Unidades de streaming para las reparticiones

Experimente y observe el uso de recursos de su trabajo para determinar el número exacto de particiones que necesita. El número de [unidades de streaming (SU)](stream-analytics-streaming-unit-consumption.md) debe ajustarse según los recursos físicos necesarios para cada partición. En general, se necesitan seis unidades de streaming para cada partición. Si no hay suficientes recursos asignados al trabajo, el sistema aplicará la repartición solo si beneficia al trabajo.

## <a name="repartitions-for-sql-output"></a>Reparticiones para la salida de SQL

Cuando el trabajo usa SQL Database para la salida, use el reparticionamiento explícito para hacer coincidir el recuento de particiones óptimo y maximizar el rendimiento. Dado que SQL funciona mejor con ocho escritores, volver a particionar el flujo con ocho antes del vaciado, o en algún otro flujo ascendente, puede beneficiar el rendimiento del trabajo. 

Cuando hay más de 8 particiones de entrada, es posible que heredar el esquema de partición de entrada no sea una opción adecuada. Considere el uso de [INTO](/stream-analytics-query/into-azure-stream-analytics#into-shard-count) en la consulta para especificar explícitamente el número de redactores de salida. 

En el ejemplo siguiente se lee desde la entrada, independientemente de que se cree una partición de forma natural, y se crea una partición de la secuencia diez veces mayor según la dimensión de DeviceID y se vacían los datos en la salida. 

```sql
SELECT * INTO [output] FROM [input] PARTITION BY DeviceID INTO 10
```

Para obtener más información, vea [Salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md).


## <a name="next-steps"></a>Pasos siguientes

* [Introducción al uso de Azure Stream Analytics](stream-analytics-introduction.md)
* [Aprovechamiento de la paralelización de consultas en Azure Stream Analytics](stream-analytics-parallelization.md)
