---
title: ST_DISTANCE en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL ST_DISTANCE en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f01f5faf68821fe9f85657c74111efdbb02bd204
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100559934"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve la distancia entre las dos expresiones Point, Polygon, MultiPolygon o LineString de GeoJSON. Para más información, consulte el artículo [Datos de ubicación geoespaciales y GeoJSON](sql-query-geospatial-intro.md).
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica con la distancia, que se expresa en metros para el sistema de referencia predeterminado.  
  
## <a name="examples"></a>Ejemplos
  
  En el siguiente ejemplo se muestra cómo se devuelven todos los documentos de la familia en un radio de 30 km a partir de la ubicación especificada mediante la función integrada `ST_DISTANCE`. .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice geoespacial](index-policy.md#spatial-indexes), excepto en las consultas con agregados.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
