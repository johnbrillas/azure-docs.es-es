---
title: Introducción a Event Grid de Azure Cache for Redis
description: Use Azure Event Grid para publicar eventos de Azure Cache for Redis.
author: curib
ms.author: cauribeg
ms.date: 12/21/2020
ms.topic: conceptual
ms.service: cache
ms.openlocfilehash: 0a0809076367356739dfeadcf8dd63f88866a987
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99056973"
---
# <a name="azure-cache-for-redis-event-grid-overview"></a>Introducción a Event Grid de Azure Cache for Redis 

Los eventos de Azure Cache for Redis, como la aplicación de revisiones, el escalado y la exportación o importación (RDB) se insertan mediante [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) en suscriptores como Azure Functions, Azure Logic Apps o incluso en su propio cliente de escucha HTTP. Event Grid proporciona servicios de entrega confiables para sus aplicaciones mediante directivas de reintento enriquecidas y colas de mensajes fallidos.

Consulte el artículo [Esquema de eventos de Azure Cache for Redis](../event-grid/event-schema-azure-cache.md) para ver la lista completa de los eventos que admite Azure Cache for Redis.

Si quiere probar eventos de Azure Cache for Redis, consulte cualquiera de estos inicios rápidos:

|Si desea utilizar esta herramienta:    |Consulte este inicio rápido: |
|--|-|
|Azure Portal    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con Azure Portal](cache-event-grid-quickstart-portal.md)|
|PowerShell    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con PowerShell](cache-event-grid-quickstart-powershell.md)|
|Azure CLI    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web mediante la CLI de Azure](cache-event-grid-quickstart-cli.md)|

## <a name="the-event-model"></a>Modelo de evento

Event Grid usa las [suscripciones a eventos](../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Esta imagen ilustra la relación entre los publicadores de eventos, las suscripciones a eventos y los controladores de eventos.

:::image type="content" source="media/cache-event-grid/event-grid-model.png" alt-text="Modelo de Event Grid.":::

Primero, suscriba un punto de conexión a un evento. A continuación, cuando se desencadene un evento, el servicio Event Grid enviará datos sobre ese evento al punto de conexión.

Consulte el artículo [Esquema de eventos de Azure Cache for Redis](../event-grid/event-schema-azure-cache.md) para ver:

> [!div class="checklist"]
> * Una lista completa de los eventos de Azure Cache for Redis y el modo en que se desencadena cada uno.
> * Un ejemplo de los datos que enviaría Event Grid para cada uno de estos eventos.
> * El propósito de cada par clave-valor que aparece en los datos.


## <a name="best-practices-for-consuming-events"></a>Procedimiento recomendado para el consumo de eventos

Las aplicaciones que controlan los eventos de Azure Cache for Redis deben seguir algunos procedimientos recomendados:
> [!div class="checklist"]
> * Dado que se pueden configurar varias suscripciones para enrutar los eventos al mismo controlador de eventos, es importante no asumir que los eventos provienen de un origen determinado, sino comprobar el tema del mensaje para asegurarse de que proviene de la instancia de Azure Cache for Redis prevista.
> * De igual forma, compruebe que eventType es uno de los que está preparado para procesar y no asuma que todos los eventos que reciba van a ser los tipos que espera.
> * Los eventos de Azure Cache for Redis garantizan la entrega a los suscriptores al menos una vez, lo que confirma que se generan todos los mensajes. Sin embargo, debido a los reintentos o a la disponibilidad de las suscripciones, en algunas ocasiones se pueden generar mensajes duplicados. Para más información sobre la entrega de los mensajes y los reintentos de entrega, consulte [Entrega y reintento de entrega de mensajes de Event Grid](../event-grid/delivery-and-retry.md).


## <a name="next-steps"></a>Pasos siguientes

Conozca más sobre Event Grid y pruebe los eventos de Azure Cache for Redis:

- [Una introducción a Azure Event Grid](../event-grid/overview.md)
- [Esquema de eventos de Azure Cache for Redis](../event-grid/event-schema-azure-cache.md)
- [Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web mediante la CLI de Azure](cache-event-grid-quickstart-cli.md)
- [Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con Azure Portal](cache-event-grid-quickstart-portal.md)
- [Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con PowerShell](cache-event-grid-quickstart-powershell.md)
