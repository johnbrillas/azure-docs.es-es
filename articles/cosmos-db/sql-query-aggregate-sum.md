---
title: SUM en el lenguaje de consulta de Azure Cosmos DB
description: Aprenda sobre la función del sistema de SQL Sum (SUM) en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: d5a86cd5af504072480e0cd749caa6c0532d0bc1
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550586"
---
# <a name="sum-azure-cosmos-db"></a>SUM (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Esta función de agregado devuelve la suma de todos los valores de la expresión.
  
## <a name="syntax"></a>Sintaxis
  
```sql
SUM(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
El siguiente ejemplo devuelve la suma de `propertyA`:
  
```sql
SELECT SUM(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy). Si algún argumento de `SUM` es una cadena, un valor booleano o un valor null, toda la función del sistema de agregado devolverá `undefined`. Si algún argumento tiene un valor `undefined`, no afectará al cálculo de `SUM`.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas en Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Funciones del sistema en Azure Cosmos DB](sql-query-system-functions.md)
- [Funciones de agregado en Azure Cosmos DB](sql-query-aggregate-functions.md)