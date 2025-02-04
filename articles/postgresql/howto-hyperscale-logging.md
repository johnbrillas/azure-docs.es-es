---
title: 'Registros: Hiperescala (Citus) en Azure Database for PostgreSQL'
description: 'Procedimiento para acceder a los registros de base de datos para Azure Database for PostgreSQL: Hiperescala (Citus)'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: ca3cc2873fbc6db72b10c80daecddf1471e30ff4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100577067"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Registros en Hiperescala (Citus) para Azure Database for PostgreSQL

Los registros de PostgreSQL están disponibles en cada nodo de un grupo de servidores de Hiperescala (Citus). Puede enviar registros a un servidor de almacenamiento o a un servicio de análisis. Los registros se pueden usar para identificar, solucionar y reparar errores de configuración y casos de rendimiento no óptimo.

## <a name="accessing-logs"></a>Acceso a los registros

Para acceder a los registros de PostgreSQL para un nodo de trabajo o coordinador de Hiperescala (Citus), abra el nodo en Azure Portal:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="lista de nodos":::

En el nodo seleccionado, abra **Configuración de diagnóstico** y haga clic en **+ Agregar configuración de diagnóstico**.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="Botón Agregar configuración de diagnóstico":::

Elija un nombre para la nueva configuración de diagnóstico y active la casilla **PostgreSQLLogs**.  Elija el o los destinos que deben recibir los registros.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="Elección de los registros de PostgreSQL":::

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a las consultas de Log Analytics](../azure-monitor/logs/log-analytics-tutorial.md)
- Información acerca de [Azure Event Hubs](../event-hubs/event-hubs-about.md)