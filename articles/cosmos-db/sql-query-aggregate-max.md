---
title: MAX en el lenguaje de consulta de Azure Cosmos DB
description: Aprenda sobre la función del sistema de SQL Max (MAX) en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: a3d8e3f2687a492461bdbbdd761e26cdb58f9e96
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550579"
---
# <a name="max-azure-cosmos-db"></a>MAX (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Esta función de agregado devuelve el máximo de los valores de la expresión.
  
## <a name="syntax"></a>Sintaxis
  
```sql
MAX(<scalar_expr>)  
```  
  
## <a name="arguments"></a>Argumentos

*scalar_expr*  
   Es una expresión escalar. 
  
## <a name="return-types"></a>Tipos de valores devueltos
  
Devuelve una expresión escalar.  
  
## <a name="examples"></a>Ejemplos
  
El siguiente ejemplo devuelve el valor máximo de `propertyA`:
  
```sql
SELECT MAX(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy). Los argumentos de `MAX` pueden ser número, cadena, valor booleano o un valor null. Se omitirán los valores no definidos.

Al comparar distintos tipos de datos, se utiliza el orden de prioridad siguiente (en orden descendente):

- cadena
- número
- boolean
- null

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas en Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Funciones del sistema en Azure Cosmos DB](sql-query-system-functions.md)
- [Funciones de agregado en Azure Cosmos DB](sql-query-aggregate-functions.md)