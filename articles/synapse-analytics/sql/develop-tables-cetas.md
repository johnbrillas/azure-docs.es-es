---
title: Instrucción CREATE EXTERNAL TABLE AS SELECT (CETAS) en Synapse SQL
description: Uso de CREATE EXTERNAL TABLE AS SELECT (CETAS) con Synapse SQL
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 09/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 23b74c06ebd45092b9efe36a870eeac7f6fb828a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101677559"
---
# <a name="cetas-with-synapse-sql"></a>CETAS con Synapse SQL

Puede usar la instrucción CREATE EXTERNAL TABLE AS SELECT (CETAS) en el grupo de SQL dedicado o sin servidor para completar las siguientes tareas:  

- Crear una tabla externa
- Exportar, en paralelo, los resultados de una instrucción SELECT de Transact-SQL a:

  - Hadoop
  - Blob de Azure Storage
  - Azure Data Lake Storage Gen2

## <a name="cetas-in-dedicated-sql-pool"></a>CETAS en el grupo de SQL dedicado

Para más información sobre el grupo de SQL dedicado, y el uso y la sintaxis de CETAS, consulte el artículo [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql?view=azure-sqldw-latest&preserve-view=true). Además, para instrucciones sobre CTAS con el grupo de SQL dedicado, consulte el artículo [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?view=azure-sqldw-latest&preserve-view=true).

## <a name="cetas-in-serverless-sql-pool"></a>CETAS en el grupo de SQL sin servidor

Al usar el grupo de SQL sin servidor, con CETAS se crea una tabla externa y se exportan los resultados de la consulta a Azure Storage Blob o Azure Data Lake Storage Gen2.

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Argumentos

*[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name**

Nombre de entre una y tres partes de la tabla que se va a crear. Si se trata de una tabla externa, el grupo de SQL sin servidor almacena solo sus metadatos. En el grupo de SQL sin servidor, no se desplazan ni se almacenan datos reales.

LOCATION = *'path_to_folder'*

Especifica dónde se deben escribir los resultados de la instrucción SELECT en el origen de datos externo. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo. La ubicación debe apuntar a una carpeta y tener un carácter "/" final. Ejemplo: aggregated_data/

DATA_SOURCE = *external_data_source_name*

Especifica el nombre del objeto de origen de datos externo que contiene la ubicación en donde se almacenarán los datos externos. Para crear un origen de datos externo, use [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source).

FILE_FORMAT = *external_file_format_name*

Especifica el nombre del objeto de formato de archivo externo que contiene el formato del archivo de datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format). Actualmente solo se admiten formatos de archivos externos con FORMAT_TYPE=PARQUET y FORMAT_TYPE=DELIMITEDTEXT. No se admite la compresión GZip para el formato DELIMITEDTEXT.

WITH *<common_table_expression>*

Especifica un conjunto de resultados temporal con nombre, conocido como expresión de tabla común (CTE). Para más información, consulte [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?view=azure-sqldw-latest&preserve-view=true).

SELECT <select_criteria>

Rellena la nueva tabla con los resultados de una instrucción SELECT. *select_criteria* es el cuerpo de la instrucción SELECT que determina qué datos se copian en la nueva tabla. Para más información sobre las instrucciones SELECT, consulte [SELECT (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?view=azure-sqldw-latest&preserve-view=true).

> [!NOTE]
> La cláusula ORDER BY en SELECT no se admite en CETAS.

## <a name="permissions"></a>Permisos

Debe tener permisos para mostrar el contenido de la carpeta y escribir en la carpeta LOCATION de CETAS para que funcione.

## <a name="examples"></a>Ejemplos

En estos ejemplos se usa CETAS para guardar la población total agregada por año y estado a una carpeta aggregated_data que se encuentra en el origen de datos population_ds.

Este ejemplo se basa en las credenciales, el origen de datos y el formato de archivo externo que se crearon anteriormente. Consulte el documento [tablas externas](develop-tables-external-tables.md). Para guardar los resultados de la consulta en una carpeta diferente del mismo origen de datos, cambie el argumento LOCATION. 

Para guardar los resultados en una cuenta de almacenamiento distinta, cree y use un origen de datos diferente para el argumento DATA_SOURCE.

> [!NOTE]
> Los ejemplos siguientes usan una cuenta de almacenamiento pública de Azure Open Data. Es de solo lectura. Para ejecutar estas consultas, debe proporcionar el origen de datos para el que tenga permisos de escritura.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query the newly created external table
SELECT * FROM population_by_year_state
```

En el ejemplo siguiente se usa una tabla externa como origen de CETAS. Esta se basa en las credenciales, el origen de datos, el formato de archivo externo y la tabla externa que se crearon anteriormente. Consulte el documento [tablas externas](develop-tables-external-tables.md).

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query the newly created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Tipos de datos admitidos

CETAS se puede usar para almacenar conjuntos de resultados con los siguientes tipos de datos de SQL:

- binary
- varbinary
- char
- varchar
- NCHAR
- NVARCHAR
- smalldate
- date
- datetime
- datetime2
- datetimeoffset
- time
- Decimal
- NUMERIC
- FLOAT
- real
- bigint
- TINYINT
- SMALLINT
- int
- bigint
- bit
- money
- SMALLMONEY
- UNIQUEIDENTIFIER

> [!NOTE]
> Los objetos grandes mayores de 1 MB no se pueden usar con CETAS.

## <a name="next-steps"></a>Pasos siguientes

Pruebe a consultar [Tablas externas de Apache Spark para Azure Synapse](develop-storage-files-spark-tables.md).
