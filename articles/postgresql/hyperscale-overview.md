---
title: Información general de la opción Hiperescala (Citus) de Azure Database for PostgreSQL
description: Proporciona información general sobre la opción de implementación Hiperescala (Citus).
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: overview
ms.date: 09/01/2020
ms.openlocfilehash: 1734128384d63749d3c777cf6315278fced9d140
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "95025147"
---
# <a name="what-is-azure-database-for-postgresql---hyperscale-citus"></a>¿Qué es la opción Hiperescala (Citus) de Azure Database for PostgreSQL?

Azure Database for PostgreSQL es un servicio de base de datos relacional en la nube de Microsoft diseñado para desarrolladores. Se basa en la versión Community del motor de base de datos [PostgreSQL](https://www.postgresql.org/) de código abierto.

Hiperescala (Citus) es una opción de implementación que escala horizontalmente las consultas entre varias máquinas mediante el particionamiento. Su motor de consultas paraleliza las consultas SQL entrantes en estos servidores para agilizar las respuestas en conjuntos de datos grandes. Proporciona servicios a las aplicaciones que requieren mayor escala y mejor rendimiento. Por ejemplo, suele ser el caso de las cargas de trabajo que se aproximan a los 100 GB de datos (o que ya los superan).

Hiperescala (Citus) ofrece lo siguiente:

- Escalado horizontal entre varias máquinas mediante particionamiento
- Paralelización de consultas entre estos servidores, lo que agiliza las respuestas en conjuntos de datos grandes
- Una excelente compatibilidad con aplicaciones de varios inquilinos, análisis operativos en tiempo real y cargas de trabajo transaccionales de alto rendimiento

Las aplicaciones compiladas para PostgreSQL puede ejecutar consultas distribuidas en Citus con las [bibliotecas de conexiones](./concepts-connection-libraries.md) estándar y unos cambios mínimos.

## <a name="next-steps"></a>Pasos siguientes

- Para empezar, [cree su primer](./quickstart-create-hyperscale-portal.md) grupo de servidores de la opción Hiperescala (Citus) de Azure Database for PostgreSQL.
- Consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) para ver comparaciones de costos y calculadoras. Hiperscala (Citus) también ofrece descuentos para instancias reservadas de prepago. Consulte las páginas de [precios de las instancias reservadas de Hiperescala (Citus)](concepts-hyperscale-reserved-pricing.md) para obtener más información.
- Determine el mejor [tamaño inicial](howto-hyperscale-scale-initial.md) para el grupo de servidores.
