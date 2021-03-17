---
title: Uso de flujo de datos para procesar datos de modelos de aprendizaje automático automatizado (AutoML)
description: Aprenda a usar flujos de datos de Azure Data Factory para procesar los datos de los modelos de aprendizaje automático automatizado (AutoML).
services: data-factory
author: amberz
co-author: ATLArcht
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 1/31/2021
ms.author: amberz
ms.co-author: Donnana
ms.openlocfilehash: e8352b687a3cdfac7ea2a819e1217906598a6837
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103563273"
---
# <a name="process-data-from-automated-machine-learningautoml-models-using-data-flow"></a>Procesamiento de datos de modelos de aprendizaje automático automatizado (AutoML) mediante flujo de datos

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Los proyectos de aprendizaje automático adoptan el aprendizaje automático automatizado (AutoML) para entrenar, ajustar y obtener el mejor modelo automáticamente mediante la métrica de destino que especifique para la clasificación, la regresión y la previsión de series temporales. 

Un desafío es que los datos sin procesar del almacenamiento de datos o de una base de datos transaccionales compondrían un enorme conjunto de datos, por ejemplo: 10 GB, un gran conjunto de datos requiere un mayor tiempo para entrenar los modelos, por lo que se recomienda optimizar el procesamiento de datos antes de entrenar los modelos de Azure Machine Learning. En este tutorial verá el uso de ADF para crear particiones de un conjunto de datos en archivos de Parquet para conjuntos de datos de Azure Machine Learning. 

En el proyecto de aprendizaje automático automatizado (AutoML), se aplicarían los tres escenarios siguientes de procesamiento de datos:

* Partición de grandes datos en archivos de Parquet antes de entrenar los modelos. 

     El [marco de datos de Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/overview.html) se usa normalmente para procesar los datos antes de entrenar los modelos. El marco de datos de Pandas funciona bien para tamaños de datos inferiores a 1 GB, pero si los datos tienen un tamaño superior a 1 GB, el marco de datos de Pandas ralentiza la velocidad de procesamiento de los datos, incluso a veces se genera un mensaje de error de memoria insuficiente. Los formatos de [archivo de Parquet](https://parquet.apache.org/) se recomiendan para el aprendizaje automático, ya que se trata de un formato de columnas binarias.
    
    Los flujos de datos de asignación de Azure Data Factory son transformaciones de datos sin código diseñadas visualmente para ingenieros de datos. El procesamiento de datos de gran tamaño resulta eficaz, ya que la canalización usa clústeres de Spark escalados horizontalmente.

* Divida el conjunto de datos de entrenamiento y el conjunto de datos de prueba.
    
    El conjunto de datos de entrenamiento se usará para entrenar el modelo; el conjunto de datos de prueba se usará para evaluar los modelos en el proyecto de aprendizaje automático. La actividad división condicional de flujos de datos de asignación dividiría los datos de entrenamiento y los datos de prueba. 

* Quite los datos no cualificados.

    Es posible que quiera quitar los datos no cualificados, como un archivo de Parquet con una fila de ceros. En este tutorial, usaremos la actividad Agregado para obtener el recuento de filas, el recuento de filas será una condición para quitar datos no cualificados. 


## <a name="preparation"></a>Preparación
Use la siguiente tabla de Azure SQL Database. 
```
CREATE TABLE [dbo].[MyProducts](
    [ID] [int] NULL,
    [Col1] [char](124) NULL,
    [Col2] [char](124) NULL,
    [Col3] datetime NULL,
    [Col4] int NULL

) 

```

## <a name="convert-data-format-to-parquet"></a>Conversión del formato de datos a Parquet

El flujo de datos convertirá una tabla de Azure SQL Database al formato de archivo de Parquet. 

**Conjunto de datos de origen**: Tabla de transacción de Azure SQL Database

**Conjunto de datos de receptor**: Almacenamiento de blobs con formato Parquet


## <a name="remove-unqualified-data-based-on-row-count"></a>Eliminación de datos no cualificados según el recuento de filas

Supongamos que quiere quitar el recuento de filas menor que 2. 

1. Use la actividad Agregado para obtener el recuento de filas: **Agrupar por** basado en Col2 y **Agregados** con count(1) en el recuento de filas. 

    ![Configuración de la actividad Agregado para obtener el recuento de filas](./media/scenario-dataflow-process-data-aml-models/aggregate-activity-addrowcount.png)

1. Use la actividad Receptor, elija **Sink type** (Tipo de receptor) como Caché en la pestaña **Receptor** y, a continuación, elija la columna deseada en la lista desplegable **columnas de clave** en la pestaña **Configuración**. 

    ![Configuración de la actividad CacheSink para obtener el número de filas del receptor almacenado en caché](./media/scenario-dataflow-process-data-aml-models/cachesink-activity-addrowcount.png)

1. Use la actividad columna derivada para agregar la columna del recuento de filas en el flujo de origen. En la pestaña **Derived column's settings** (Configuración de columna derivada), use la expresión CacheSink#lookup para obtener el recuento de filas de SinkCache.
    ![Configuración de la actividad de columna derivada para agregar el recuento de filas en el origen 1](./media/scenario-dataflow-process-data-aml-models/derived-column-activity-rowcount-source-1.png)

1. Use la actividad División condicional para quitar los datos no cualificados. En este ejemplo, el recuento de filas se basa en la columna Col2, y la condición es quitar el recuento de filas menor que 2, por lo que se quitarán dos filas (ID=2 e ID=7). Guardaría los datos no cualificados en un almacenamiento de blobs para la administración de datos. 

    ![Configuración de la actividad División condicional para obtener datos mayores o iguales que 2](./media/scenario-dataflow-process-data-aml-models/conditionalsplit-greater-or-equal-than-2.png)

> [!NOTE]
>    *    Cree un nuevo origen para obtener el recuento de filas que se usará en el origen original en los pasos posteriores. 
>    *    Use CacheSink desde el punto de vista del rendimiento. 

## <a name="split-training-data-and-test-data"></a>División de datos de entrenamiento y datos de prueba 

1. Queremos dividir los datos de entrenamiento y los datos de prueba para cada partición. En este ejemplo, para el mismo valor de Col2, obtenga las 2 primeras filas como datos de prueba y las filas restantes como datos de entrenamiento. 

    Use la actividad Ventana para agregar un número de columna o fila para cada partición. En la pestaña **Over** (Por encima), elija la columna de la partición (en este tutorial, se creará una partición para Col2), proporcione el orden en la pestaña **Sort** (Orden) (en este tutorial, se basará en el identificador para ordenar), y en la pestaña **Window columns** (Columnas de Ventana) agregue una columna como número de fila para cada partición. 
    ![Configuración de la actividad Ventana para agregar una nueva columna que sea el número de fila](./media/scenario-dataflow-process-data-aml-models/window-activity-add-row-number.png)

1. Use la actividad División condicional para dividir las 2 primeras filas de cada partición en el conjunto de datos de pruebas, y las filas restantes en el conjunto de datos de entrenamiento. En la pestaña **Conditional split settings** (Configuración de división condicional), use la expresión lesserOrEqual(RowNum,2) como condición. 

    ![Configuración de la actividad División condicional para dividir el conjunto de datos actual en el conjunto de datos de entrenamiento y el conjunto de datos de prueba](./media/scenario-dataflow-process-data-aml-models/split-training-dataset-test-dataset.png)

## <a name="partition-training-dataset-and-test-dataset-with-parquet-format"></a>Particionamiento del conjunto de datos de entrenamiento y el conjunto de datos de prueba con el formato Parquet

1. Use la actividad Receptor, en la pestaña **Optimizar**, con **Unique value per partition** (valor único por partición) para establecer una columna como clave de columna para la partición. 
    ![Configuración de la actividad Receptor para establecer la partición del conjunto de datos de entrenamiento](./media/scenario-dataflow-process-data-aml-models/partition-training-dataset-sink.png)

    Echemos un vistazo a toda la lógica de la canalización.
    ![Lógica de toda la canalización](./media/scenario-dataflow-process-data-aml-models/entire-pipeline.png)


## <a name="next-steps"></a>Pasos siguientes

* Compile el resto de la lógica del flujo de datos mediante [transformaciones](concepts-data-flow-overview.md) de flujos de datos de asignación.
