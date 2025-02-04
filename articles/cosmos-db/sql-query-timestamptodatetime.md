---
title: TimestampToDateTime en el lenguaje de consulta de Azure Cosmos DB
description: Conozca la función del sistema SQL TimestampToDateTime en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: e9a77ab0ac32d627d59e2cb0fa4a680f174a6833
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104587243"
---
# <a name="timestamptodatetime-azure-cosmos-db"></a>TimestampToDateTime (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Convierte el valor de marca de tiempo especificado en un valor de fecha y hora.
  
## <a name="syntax"></a>Sintaxis
  
```sql
TimestampToDateTime (<Timestamp>)
```

## <a name="arguments"></a>Argumentos

*Timestamp*  

Un valor numérico con signo, el número actual milisegundos que han transcurrido desde la época de Unix. Es decir, el número de milisegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.

## <a name="return-types"></a>Tipos de valores devueltos

Devuelve el valor de cadena de fecha y hora UTC ISO 8601 con el formato `YYYY-MM-DDThh:mm:ss.fffffffZ`, donde:
  
|Formato|Descripción|
|-|-|
|AAAA|año con cuatro dígitos|
|MM|mes con dos dígitos (01 = enero, etc.)|
|DD|día con dos dígitos del mes (de 01 a 31)|
|T|indica el comienzo de los elementos de hora|
|hh|hora con dos dígitos (de 00 a 23)|
|MM|minutos con dos dígitos (de 00 a 59)|
|ss|segundos con dos dígitos (de 00 a 59)|
|.fffffff|fracciones de segundos de siete dígitos|
|Z|Indicador de UTC (hora universal coordinada)|
  
  Para más información sobre el formato ISO 8601, consulte [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="remarks"></a>Observaciones

TimestampToDateTime devolverá `undefined` si el valor de marca de tiempo especificado no es válido.

## <a name="examples"></a>Ejemplos
  
En el siguiente ejemplo, la marca de tiempo se convierte en un valor de fecha y hora:

```sql
SELECT TimestampToDateTime(1594227912345) AS DateTime
```

```json
[
    {
        "DateTime": "2020-07-08T17:05:12.3450000Z"
    }
]
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de fecha y hora (Azure Cosmos DB)](sql-query-date-time-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
