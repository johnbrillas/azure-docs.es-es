---
title: Transformación de análisis de datos en el flujo de datos de asignación
description: Análisis de documentos de columnas insertadas
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: 4db9503ea84ae13148a89a03048c73399413e5cc
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101710199"
---
# <a name="parse-transformation-in-mapping-data-flow"></a>Transformación de análisis en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Use la transformación de análisis para analizar las columnas de los datos del documento. Los tipos admitidos actualmente de los documentos insertados que se pueden analizar son JSON y texto delimitado.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWykdO]

## <a name="configuration"></a>Configuración

En el panel de configuración de la transformación de análisis, primero se selecciona el tipo de datos contenidos en las columnas que desea analizar en línea. La transformación de análisis también contiene los siguientes valores de configuración.

![Configuración de análisis](media/data-flow/data-flow-parse-1.png "Analizar")

### <a name="column"></a>Columna

De forma similar a las columnas derivadas y los agregados, aquí es donde modificará una columna de salida, para lo cual deberá seleccionarla en el selector desplegable. También puede escribir aquí el nombre de una nueva columna. ADF almacenará los datos de origen analizados en esta columna.

### <a name="expression"></a>Expression

Use el generador de expresiones para establecer el origen del análisis. Esto puede ser tan sencillo como seleccionar la columna de origen con los datos independientes que desea analizar, o bien puede crear expresiones complejas para analizarlas.

### <a name="output-column-type"></a>Tipo de columna de salida

Aquí es donde va a configurar el esquema de salida de destino a partir del análisis que se escribirá en una sola columna.

![Ejemplo de análisis](media/data-flow/data-flow-parse-2.png "Ejemplo de análisis")

En este ejemplo, hemos definido el análisis del campo de entrada "jsonString", que es texto sin formato, pero con formato de estructura JSON. Vamos a almacenar los resultados analizados como JSON en una nueva columna denominada "json" con este esquema:

```(trade as boolean, customers as string[])```

Consulte la pestaña de inspección y la vista previa de los datos para comprobar que la salida se ha asignado correctamente.

## <a name="examples"></a>Ejemplos

```
source(output(
        name as string,
        location as string,
        satellites as string[],
        goods as (trade as boolean, customers as string[], orders as (orderId as string, orderTotal as double, shipped as (orderItems as (itemName as string, itemQty as string)[]))[])
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    ignoreNoFilesFound: false,
    documentForm: 'documentPerLine') ~> JsonSource
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    ignoreNoFilesFound: false) ~> CsvSource
JsonSource derive(jsonString = toString(goods)) ~> StringifyJson
StringifyJson parse(json = jsonString ? (trade as boolean,
        customers as string[]),
    format: 'json',
    documentForm: 'arrayOfDocuments') ~> ParseJson
CsvSource derive(csvString = 'Id|name|year\n\'1\'|\'test1\'|\'1999\'') ~> CsvString
CsvString parse(csv = csvString ? (id as integer,
        name as string,
        year as string),
    format: 'delimited',
    columnNamesAsHeader: true,
    columnDelimiter: '|',
    nullValue: '',
    documentForm: 'documentPerLine') ~> ParseCsv
ParseJson select(mapColumn(
        jsonString,
        json
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> KeepStringAndParsedJson
ParseCsv select(mapColumn(
        csvString,
        csv
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> KeepStringAndParsedCsv
```

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

### <a name="examples"></a>Ejemplos

```
parse(json = jsonString ? (trade as boolean,
                                customers as string[]),
                format: 'json',
                documentForm: 'singleDocument') ~> ParseJson

parse(csv = csvString ? (id as integer,
                                name as string,
                                year as string),
                format: 'delimited',
                columnNamesAsHeader: true,
                columnDelimiter: '|',
                nullValue: '',
                documentForm: 'documentPerLine') ~> ParseCsv
```    

## <a name="next-steps"></a>Pasos siguientes

* Use la [transformación Aplanar](data-flow-flatten.md) para dinamizar las filas en columnas.
* Use la [transformación de columna derivada](data-flow-derived-column.md) para dinamizar columnas en filas.
