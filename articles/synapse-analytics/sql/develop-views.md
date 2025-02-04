---
title: Vistas T-SQL con grupos de SQL
description: Sugerencias para usar las vistas de T-SQL y desarrollar soluciones con un grupo de SQL dedicado y un grupo de SQL sin servidor en Azure Synapse Analytics.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 9f52c3fd1284ce7e55680d051c5292361067fad9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101673992"
---
# <a name="t-sql-views-with-dedicated-sql-pool-and-serverless-sql-pool-in-azure-synapse-analytics"></a>Vistas de T-SQL con grupos de SQL dedicados y grupos de SQL sin servidor en Azure Synapse Analytics

En este artículo, encontrará sugerencias para usar las vistas de T-SQL y desarrollar soluciones con un grupo de SQL dedicado y un grupo de SQL sin servidor en Azure Synapse Analytics.

## <a name="why-use-views"></a>Razones para usar las vistas

Las vista se pueden usar de formas diferentes para mejorar la calidad de la solución.  Este artículo resalta algunos ejemplos de cómo enriquecer su solución con vistas, e incluye las limitaciones que se deben tener en cuenta.

### <a name="sql-pool---create-view"></a>Grupo de SQL: creación de una vista

> [!NOTE]
> En este artículo no se explica la sintaxis de CREATE VIEW. Para más información, consulte la documentación de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?view=azure-sqldw-latest&preserve-view=true).

## <a name="architectural-abstraction"></a>Abstracción de arquitectura

Se trata de un patrón de aplicación común para volver a crear tablas con la característica [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?view=azure-sqldw-latest&preserve-view=true) (CTAS) seguida de un patrón de cambio de nombre de objetos mientras se cargan los datos.

En el ejemplo siguiente se agregan registros de fecha nuevos a una dimensión de fecha. Observe cómo una nueva tabla, DimDate_New, se crea por primera vez y luego cambia de nombre para reemplazar la versión original de la tabla.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;
```

Tenga en cuenta que este enfoque puede provocar que las tablas aparezcan y desaparezcan de la vista del usuario, así como mensajes de error del tipo "la tabla no existe". Las vistas pueden utilizarse para proporcionar a los usuarios una capa de presentación coherente mientras se cambia el nombre de los objetos subyacentes.

Al proporcionar acceso a los datos mediante vistas, los usuarios no necesitan visibilidad para las tablas subyacentes. Además de una experiencia de usuario coherente, este nivel garantiza que los diseñadores de análisis puedan desarrollar el modelo de datos. Tener la capacidad de evolucionar las tablas subyacentes significa que los diseñadores pueden usar CTAS para maximizar el rendimiento durante el proceso de carga de datos.

## <a name="performance-optimization"></a>Optimización del rendimiento

Las vistas se pueden usar también para aplicar combinaciones optimizadas para rendimiento entre tablas. Por ejemplo, una vista puede incorporar una clave de distribución redundante como parte de los criterios de combinación para minimizar el movimiento de datos.

Otra ventaja de usar vistas T-SQL consiste en forzar una consulta específica o una sugerencia de combinación. Como tal, la funcionalidad de las vistas garantiza que las uniones siempre se realizan de manera óptima. Se evitará que los usuarios tengan que recordar la construcción correcta para sus combinaciones.

## <a name="limitations"></a>Limitaciones

Las vistas de SQL de Synapse solo se almacenan como metadatos. Por lo tanto, las siguientes opciones no están disponibles:

* No hay ninguna opción de enlace de esquema.
* No se puede actualizar las tablas base mediante la vista.
* No se pueden crear vistas en tablas temporales.
* No hay compatibilidad con las sugerencias EXPAND y NOEXPAND.
* No hay ninguna vista indexada en SQL de Synapse.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de SQL de Synapse](develop-overview.md).



