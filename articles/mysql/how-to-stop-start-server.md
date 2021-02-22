---
title: Inicio o detención de un servidor de Azure Database for MySQL mediante Azure Portal
description: En este artículo se explica cómo iniciar y detener operaciones en Azure Database for MySQL.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: d297d215d4b0edfdd67b603ba4707bf02057ad78
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100516879"
---
# <a name="stopstart-an-azure-database-for-mysql"></a>Inicio o detención de una instancia de Azure Database for MySQL

> [!IMPORTANT]
>  Cuando se **detiene** el servidor permanece en ese estado durante los siete días siguientes. Si no lo **inicia** de forma manual durante ese tiempo, se iniciará automáticamente al final de los siete días. Puede optar por volver a **detenerlo** si no va a usar el servidor.

En este artículo se proporciona un procedimiento detallado para iniciar y detener un único servidor.

## <a name="prerequisites"></a>Prerrequisitos

Para completar esta guía, necesita:

-   Debe tener un único servidor de Azure Database for MySQL.

> [!NOTE]
> Consulte las limitaciones de usar el [inicio o detención](concepts-servers.md#limitations-of-stopstart-operation)

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-azure-portal"></a>Inicio o detención de una instancia de Azure Database for MySQL con Azure Portal

### <a name="stop-a-running-server"></a>Detención de un servidor en ejecución

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor único que quiera detener.

2.  En la página **Información general**, haga clic en el botón **Detener** de la barra de herramientas.

    :::image type="content" source="./media/howto-stop-start-server/mysql-stop-server.png" alt-text="Detener el servidor de Azure Database for MySQL":::

    > [!NOTE]
    > Una vez detenido el servidor, las operaciones de administración restantes no estarán disponibles para el servidor único.

### <a name="start-a-stopped-server"></a>Inicio de un servidor detenido

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor único que quiere iniciar.

2.  En la página **Información general**, haga clic en el botón **Iniciar** de la barra de herramientas.

    :::image type="content" source="./media/howto-stop-start-server/mysql-start-server.png" alt-text="Iniciar el servidor de Azure Database for MySQL":::

    > [!NOTE]
    > Una vez iniciado el servidor, todas las operaciones de administración estarán disponibles para el servidor único.

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-cli"></a>Inicio o detención de una instancia de Azure Database for MySQL con la CLI

### <a name="stop-a-running-server"></a>Detención de un servidor en ejecución

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor único que quiera detener.

2.  En la página **Información general**, haga clic en el botón **Detener** de la barra de herramientas.

    ```azurecli-interactive
    az mysql server stop --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Una vez detenido el servidor, las operaciones de administración restantes no estarán disponibles para el servidor único.

### <a name="start-a-stopped-server"></a>Inicio de un servidor detenido

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor único que quiere iniciar.

2.  En la página **Información general**, haga clic en el botón **Iniciar** de la barra de herramientas.

    ```azurecli-interactive
    az mysql server start --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Una vez iniciado el servidor, todas las operaciones de administración estarán disponibles para el servidor único.

## <a name="next-steps"></a>Pasos siguientes
Aprenda sobre la [creación de alertas de métricas](howto-alert-on-metric.md).
