---
title: 'Conexión y consulta: PostgreSQL: servidor flexible'
description: 'Vínculos a inicios rápidos en los que se muestra cómo conectarse a Azure Database for PostgreSQL: servidor flexible y ejecutar consultas.'
services: postgresql
ms.service: postgresql
ms.topic: how-to
author: mksuni
ms.author: sumuth
ms.date: 12/08/2020
ms.openlocfilehash: ee3b1f7db8bdafb1233b32579e032e8c864c37a9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97364539"
---
# <a name="connect-and-query-overview-for-azure-database-for-postgresql--flexible-server"></a>Información general sobre la conexión y consulta de Azure Database for PostgreSQL: servidor flexible

En el documento siguiente se incluyen vínculos a ejemplos que muestran cómo conectarse al Servidor único de Azure Database for PostgreSQL y realizar consultas en él. En esta guía también se incluyen las recomendaciones y la extensión de TLS que puede usar para conectarse al servidor en los idiomas admitidos a continuación.

>[!IMPORTANT]
> Azure Database for PostgreSQL: servidor flexible se encuentra en **versión preliminar**.

## <a name="quickstarts"></a>Guías de inicio rápido

| Guía de inicio rápido | Descripción |
|---|---|
|[Pgadmin](https://www.pgadmin.org/)|Puede usar pgAdmin para conectarse al servidor y simplifica la creación, el mantenimiento y el uso de objetos de base de datos.|
|[psql en Azure Cloud Shell](./quickstart-create-server-cli.md#connect-using-postgresql-command-line-client)|En este artículo se muestra cómo ejecutar [**psql**](https://www.postgresql.org/docs/current/static/app-psql.html) en [Azure Cloud Shell](../../cloud-shell/overview.md) para conectarse al servidor y, después, ejecutar instrucciones para consultar, insertar, actualizar y eliminar datos en la base de datos. Puede ejecutar **psql** si está instalado en el entorno de desarrollo.|
|[Python](connect-python.md)|En este inicio rápido se muestra cómo utilizar Python para conectarse a una base de datos y usar el trabajo con objetos de base de datos a fin de consultar los datos. |
|[Django con App Service](tutorial-django-app-service-postgres.md)|En este tutorial se muestra cómo usar Ruby para crear un programa que se conecte a una base de datos y usarla para trabajar con objetos de base de datos a fin de consultar los datos.|

## <a name="tls-considerations-for-database-connectivity"></a>Consideraciones de TLS para la conectividad de bases de datos

El protocolo Seguridad de capa de transporte (TLS) lo usan todos los controladores que Microsoft proporciona o admite para conectarse a bases de datos de Azure Database for PostgreSQL. No es necesaria ninguna configuración especial, pero se aplica TLS 1.2 para los servidores recientemente creados. Se recomienda que, si usa TLS 1.0 y 1.1, después actualice la versión de TLS para los servidores. Vea [ Procedimientos para configurar TLS](how-to-connect-tls-ssl.md)

## <a name="postgresql-extensions"></a>Extensiones de PostgreSQL

PostgreSQL ofrece la capacidad de ampliar la funcionalidad de su base de datos mediante extensiones. Las extensiones agrupan varios objetos SQL relacionados en un solo paquete que se puede cargar o quitar de la base de datos con un solo comando. Después de cargarse en la base de datos, las extensiones funcionan como características integradas.

- [Extensiones de Postgres 12](./concepts-extensions.md#postgres-12-extensions)
- [Extensiones de Postgres 11](./concepts-extensions.md#postgres-11-extensions)
- [dblink y postgres_fdw](./concepts-extensions.md#dblink-and-postgres_fdw)
- [pg_prewarm](./concepts-extensions.md#pg_prewarm)
- [pg_stat_statements](./concepts-extensions.md#pg_stat_statements)

Para más información, consulte [¿Cómo se utilizan las extensiones de PostgreSQL en el servidor flexible?](concepts-extensions.md).

## <a name="next-steps"></a>Pasos siguientes

- [Migración de datos mediante volcado y restauración](../howto-migrate-using-dump-and-restore.md)
- [Migración de datos mediante importación y exportación](../howto-migrate-using-export-and-import.md)
