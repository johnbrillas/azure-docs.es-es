---
title: 'Problemas conocidos y limitaciones de Azure Database for PostgreSQL: servidor único y servidor flexible (versión preliminar)'
description: Enumera los problemas conocidos que los clientes deben tener en cuenta.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 3c26368e6281ad74d617891ba15c980117b25a6e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100105419"
---
# <a name="azure-database-for-postgresql---known-issues-and-limitations"></a>Problemas conocidos y limitaciones de Azure Database for PostgreSQL

En esta página se proporciona una lista de problemas conocidos en Azure Database for PostgreSQL que podrían afectar a la aplicación. También se enumeran las mitigaciones y recomendaciones para solucionar el problema.

## <a name="intelligent-performance---query-store"></a>Rendimiento inteligente del almacén de consultas

Se aplica a Azure Database for PostgreSQL: servidor único.

| Aplicable | Causa | Corrección|
| ----- | ------ | ---- | 
| PostgreSQL 9.6, 10, 11 | La activación del parámetro de servidor `pg_qs.replace_parameter_placeholders` puede provocar un cierre del servidor en algunos escenarios poco comunes. | A través de la sección Parámetros del servidor de Azure Portal, convierta el valor del parámetro `pg_qs.replace_parameter_placeholders` en `OFF` y guárdelo.   | 

## <a name="server-parameters"></a>Parámetros del servidor

Se aplica a Azure Database for PostgreSQL: servidor único y servidor flexible.

| Aplicable | Causa | Corrección| 
| ----- | ------ | ---- | 
| PostgreSQL 9.6, 10, 11 | Cambiar el parámetro de servidor `max_locks_per_transaction` a un valor más alto que el [recomendado](https://www.postgresql.org/docs/11/kernel-resources.html) podría provocar que el servidor no pudiera aparecer después de un reinicio. | Déjelo en el valor predeterminado (32 o 64) o cambie a un valor razonable según la [documentación](https://www.postgresql.org/docs/11/kernel-resources.html) de PostgreSQL. <br> <br> En el lado del servicio, se está trabajando para limitar el valor alto en función de la SKU.  | 

## <a name="next-steps"></a>Pasos siguientes
- Consulte los [procedimientos recomendados](./concepts-query-store-best-practices.md) del almacén de consultas
