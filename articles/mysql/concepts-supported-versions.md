---
title: Versiones admitidas de Azure Database for MySQL
description: Obtenga información sobre las versiones del servidor de MySQL que se admiten en el servicio Azure Database for MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 920f6a4fec1ec8a260a98641888268e4955bbf44
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101718784"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Versiones admitidas de servidores de Azure Database for MySQL

Azure Database for MySQL se ha desarrollado a partir de [MySQL Community Edition](https://www.mysql.com/products/community/), con el motor de almacenamiento InnoDB. El servicio es compatible con todas las versiones principales actuales admitidas por la comunidad: MySQL 5.6, 5.7 y 8.0. MySQL usa el esquema de nomenclatura X.Y.Z, donde X es la versión principal, Y es la secundaria y Z es la versión de corrección de errores. Para más información sobre el esquema, consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

> [!NOTE]
> En la opción de implementación de un solo servidor, se usa una puerta de enlace para redirigir las conexiones a las instancias de servidor. Una vez establecida la conexión, el cliente de MySQL muestra la versión de MySQL establecida en la puerta de enlace, no la versión real que se ejecuta en la instancia del servidor MySQL. Para determinar la versión de la instancia del servidor MySQL, use el comando `SELECT VERSION();` en el símbolo del sistema de MySQL.

Actualmente, Azure Database for MySQL admite las siguientes versiones principales y secundarias de MySQL:


| Versión | Servidor único <br/> Versión secundaria actual |Servidor flexible (versión preliminar) <br/> Versión secundaria actual  |
|:-------------------|:-------------------------------------------|:---------------------------------------------|
|MySQL versión 5.6 |  [5.6.47](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) | No compatible|
|MySQL versión 5.7 | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html)|
|MySQL versión 8.0 | [8.0.15](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) | [8.0.21](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-21.html)|

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
El servicio administra automáticamente la aplicación de partes para las actualizaciones de las versiones de corrección de errores. Por ejemplo, 5.7.20 a 5.7.21.  

El servicio admite actualmente la actualización de la versión principal para las actualizaciones de MySQL versión 5.6 a 5.7. Para obtener más información, consulte [cómo realizar actualizaciones de versión principal](how-to-major-version-upgrade.md). Si quiere actualizar de la versión 5.7 a la 8.0, se recomienda que realice un [volcado y restauración de memoria](./concepts-migrate-dump-restore.md) en un servidor que se haya creado con la nueva versión del motor.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la directiva de control de versiones de Azure Database for MySQL, consulte [este documento](concepts-version-policy.md).
- Para obtener información sobre las cuotas y limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).
