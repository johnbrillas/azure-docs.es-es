---
title: 'Reinicio de servidor mediante la CLI de Azure de Azure Database for PostgreSQL: servidor único'
description: En este artículo se describe cómo reiniciar una instancia de Azure Database for PostgreSQL con un único servidor mediante la CLI de Azure.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: e812b7872dd4d41d9a6cb87d75403524106c5981
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97706871"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Reiniciar una instancia de Azure Database for PostgreSQL con un único servidor con la CLI de Azure
En este tema se describe cómo reiniciar un servidor de Azure Database for PostgreSQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.
 
> [!NOTE] 
> El tiempo necesario para completar un reinicio depende del proceso de recuperación de PostgreSQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio. También puede aumentar la frecuencia de los puntos de comprobación. También puede ajustar los valores de los parámetros relacionados con el punto de comprobación, entre los que se incluye `max_wal_size`. También se recomienda ejecutar el comando `CHECKPOINT` antes de reiniciar el servidor.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía de procedimientos:
- Cree un [servidor de Azure Database for PostgreSQL](quickstart-create-server-up-azure-cli.md).

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="restart-the-server"></a>Reinicio del servidor

Reinicie el servidor con el siguiente comando:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo establecer parámetros en Azure Database for PostgreSQL](howto-configure-server-parameters-using-cli.md)
