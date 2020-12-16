---
title: COUNT en el lenguaje de consulta de Azure Cosmos DB
description: Aprenda sobre la función del sistema de SQL Count (COUNT) en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 5228558f4bcb146ec08ee5fff45fb1bdf4d56f01
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550578"
---
# <a name="count-azure-cosmos-db"></a>COUNT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Esta función del sistema devuelve el recuento de los valores de la expresión.
  
## <a name="syntax"></a>Sintaxis
  
```sql
COUNT(<scalar_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*scalar_expr*  
   Es cualquier expresión escalar.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
En el ejemplo siguiente se devuelve el recuento total de elementos de un contenedor:
  
```sql
SELECT COUNT(1)
FROM c
``` 
COUNT puede tener cualquier expresión escalar como entrada. La siguiente consulta generará resultados equivalentes:

```sql
SELECT COUNT(2)
FROM c
```

## <a name="remarks"></a>Comentarios

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy) para cualquier propiedad del filtro de la consulta.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas en Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Funciones del sistema en Azure Cosmos DB](sql-query-system-functions.md)
- [Funciones de agregado en Azure Cosmos DB](sql-query-aggregate-functions.md)