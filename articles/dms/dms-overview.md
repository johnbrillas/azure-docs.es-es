---
title: ¿Qué es Azure Database Migration Service?
description: Introducción a Azure Database Migration Service, que proporciona migraciones completas desde muchos orígenes de base de datos hasta las plataformas de datos de Azure.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.topic: overview
ms.date: 02/20/2020
ms.openlocfilehash: 328c29afee3752ecb11b83f22d67f20aa3a2c93e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94963018"
---
# <a name="what-is-azure-database-migration-service"></a>¿Qué es Azure Database Migration Service?

Azure Database Migration Service es un servicio totalmente administrado diseñado para permitir migraciones completas desde varios orígenes de base de datos hasta las plataformas de datos de Azure con un tiempo de inactividad mínimo (migraciones en línea).

## <a name="migrate-databases-to-azure-with-familiar-tools"></a>Migración de bases de datos a Azure con herramientas familiares

Azure Database Migration Service integra una parte de la funcionalidad de nuestras herramientas y servicios existentes. Así, ofrece a los clientes una solución completa y de alta disponibilidad. El servicio utiliza [Data Migration Assistant](/sql/dma/dma-overview) para generar informes de evaluación que proporcionen recomendaciones que le guíen a través de los cambios necesarios antes de realizar una migración. Es decisión suya aplicar los remedios necesarios. Cuando esté listo para comenzar el proceso de migración, Azure Database Migration Service realizará todos los pasos necesarios. Ya puede iniciar sus proyectos de migración y olvidarse de ellos con la tranquilidad de saber que el proceso utiliza los procedimientos recomendados que determina Microsoft. 

> [!NOTE]
> El uso de Azure Database Migration Service para realizar una migración en línea requiere la creación de una instancia basada en el plan de tarifa Premium.

## <a name="regional-availability"></a>Disponibilidad regional

Para obtener información actualizada sobre la disponibilidad regional de Azure Database Migration Service, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration).

## <a name="pricing"></a>Precios

Para obtener información actualizada sobre los precios de Azure Database Migration Service, consulte [Precios de Azure Database Migration Service](https://azure.microsoft.com/pricing/details/database-migration/).

## <a name="next-steps"></a>Pasos siguientes

* [Estado de los escenarios de migración que admite Azure Database Migration Service](resource-scenario-status.md).
* [Creación de una instancia de Azure Database Migration Service mediante Azure Portal](quickstart-create-data-migration-service-portal.md).
* [Migración de SQL Server a Azure SQL Database](tutorial-sql-server-to-azure-sql.md).
* [Introducción a los requisitos previos para usar Azure Database Migration Service](pre-reqs.md).
* [Preguntas más frecuentes sobre el uso de Azure Database Migration Service](faq.md).
* [Servicios y herramientas disponibles para escenarios de migración de datos](dms-tools-matrix.md).