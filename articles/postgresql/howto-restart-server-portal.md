---
title: 'Reinicio de servidor mediante Azure Portal de Azure Database for PostgreSQL: servidor único'
description: En este artículo se describe cómo reiniciar una instancia de Azure Database for PostgreSQL con un único servidor mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: how-to
ms.date: 12/20/2020
ms.openlocfilehash: d3dc01ee163b941f450eeac98adfdf1ea0e91190
ms.sourcegitcommit: 0830e02635d2f240aae2667b947487db01f5fdef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97706905"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Reiniciar una instancia de Azure Database for PostgreSQL con un único servidor con Azure Portal
En este tema se describe cómo reiniciar un servidor de Azure Database for PostgreSQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.
 
> [!NOTE] 
> El tiempo necesario para completar un reinicio depende del proceso de recuperación de PostgreSQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio. También puede aumentar la frecuencia de los puntos de comprobación. También puede ajustar los valores de los parámetros relacionados con el punto de comprobación, entre los que se incluye `max_wal_size`. También se recomienda ejecutar el comando `CHECKPOINT` antes de reiniciar el servidor.

## <a name="prerequisites"></a>Prerrequisitos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md).

## <a name="perform-server-restart"></a>Realización del reinicio del servidor

Los pasos siguientes reinician el servidor de PostgreSQL:

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for PostgreSQL.

2. En la barra de herramientas de la página de **información general** del servidor, haga clic en **Restart** (reiniciar).

   :::image type="content" source="./media/howto-restart-server-portal/2-server.png" alt-text="Azure Database for PostgreSQL - Información general - Botón Reiniciar":::

3. Haga clic en **Sí** para confirmar el reinicio del servidor.

   :::image type="content" source="./media/howto-restart-server-portal/3-restart-confirm.png" alt-text="Azure Database for PostgreSQL - Confirmación del reinicio":::

4. Observe que el estado del servidor cambia a "Reiniciando".

   :::image type="content" source="./media/howto-restart-server-portal/4-restarting-status.png" alt-text="Azure Database for PostgreSQL - Estado del reinicio":::

5. Confirme que el reinicio del servidor es correcto.

   :::image type="content" source="./media/howto-restart-server-portal/5-restart-success.png" alt-text="Azure Database for PostgreSQL - Reinicio correcto":::

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo establecer parámetros en Azure Database for PostgreSQL](howto-configure-server-parameters-using-portal.md)