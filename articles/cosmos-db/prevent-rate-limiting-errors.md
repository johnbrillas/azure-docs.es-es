---
title: Evitación de errores de limitación de velocidad para las operaciones de Azure Cosmos DB API para MongoDB
description: Obtenga información acerca de cómo impedir que las operaciones de Azure Cosmos DB API para MongoDB alcancen los errores de limitación de velocidad con la característica SSR (reintento en el servidor).
author: gahl-levy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 01/13/2021
ms.author: gahllevy
ms.openlocfilehash: 73c2aba3028f42621f241bd8f295e83e0ef96e68
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540405"
---
# <a name="prevent-rate-limiting-errors-for-azure-cosmos-db-api-for-mongodb-operations"></a>Evitación de errores de limitación de velocidad para las operaciones de Azure Cosmos DB API para MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Es posible que se produzcan errores en las operaciones de Azure Cosmos DB API para MongoDB de tipo de limitación de velocidad (16500/429) si superan el límite de rendimiento de una colección (RU). 

Puede habilitar la característica de reintentos en el servidor (SSR) y dejar que el servidor vuelva a intentar realizar estas operaciones automáticamente. Las solicitudes se reintentan después de un breve período de tiempo para todas las colecciones de la cuenta. Esta característica es una alternativa práctica para controlar los errores de limitación de velocidad en la aplicación cliente.


## <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Vaya a la cuenta de Azure Cosmos DB API para MongoDB.

1. Vaya al panel **Características** debajo de la sección **Configuración**.

1. Seleccione **Reintento en el servidor**.

1. Haga clic en **Habilitar** para habilitar esta característica para todas las colecciones de su cuenta.

:::image type="content" source="./media/prevent-rate-limiting-errors/portal-features-server-side-retry.png" alt-text="Captura de pantalla de la característica de reintentos en el servidor para Azure Cosmos DB API para MongoDB":::


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la solución de errores comunes, consulte este artículo:

* [Solución de errores habituales de la API de Azure Cosmos DB para MongoDB](mongodb-troubleshoot.md)
