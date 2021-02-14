---
title: GetCurrentTicks en lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL GetCurrentTicks en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 16004e6e471094c99229c32a63396ac3b0490905
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99524315"
---
# <a name="getcurrentticks-azure-cosmos-db"></a>GetCurrentTicks (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Devuelve el número de tics de 100 nanosegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.
  
## <a name="syntax"></a>Sintaxis
  
```sql
GetCurrentTicks ()
```

## <a name="return-types"></a>Tipos de valores devueltos

Devuelve un valor numérico con signo, el número actual de tics de 100 nanosegundos que han transcurrido desde la época de Unix. En otras palabras, GetCurrentTicks devuelve el número de tics de 100 nanosegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.

## <a name="remarks"></a>Observaciones

GetCurrentTicks() es una función no determinista. El resultado devuelto está en UTC (hora universal coordinada).

> [!NOTE]
> Esta función del sistema no usará el índice. Si necesita comparar valores con la hora actual, obtenga esta antes de la ejecución de la consulta y use ese valor de cadena constante en la cláusula `WHERE`.

## <a name="examples"></a>Ejemplos

Este es un ejemplo que devuelve la hora actual, medida en tics:

```sql
SELECT GetCurrentTicks() AS CurrentTimeInTicks
```

```json
[
    {
        "CurrentTimeInTicks": 15973607943002652
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de fecha y hora (Azure Cosmos DB)](sql-query-date-time-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
