---
title: ST_INTERSECTS en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema de SQL ST_INTERSECTS en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c123446c7d7f654f0e3ace6c9d92983558509c75
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100559949"
---
# <a name="st_intersects-azure-cosmos-db"></a>ST_INTERSECTS (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon, MultiPolygon o LineString) especificado en el primer argumento forma intersección con el objeto de GeoJSON (Point, Polygon, MultiPolygon o LineString) del segundo.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es cualquier expresión de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve un valor booleano.  
  
## <a name="examples"></a>Ejemplos
  
  En el siguiente ejemplo se muestra cómo buscar todas las áreas de intersección con el polígono indicado.  
  
```sql
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{ "id": "IntersectingPolygon" }]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice geoespacial](index-policy.md#spatial-indexes), excepto en las consultas con agregados.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
