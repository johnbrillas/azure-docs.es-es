---
author: baanders
description: Archivo de inclusión para los recursos necesarios para crear puntos de conexión de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 1/26/2021
ms.author: baanders
ms.openlocfilehash: 58c90bae3dea0f3a47489ea7d8de6a79f823dcab
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99054520"
---
### <a name="prerequisite-create-endpoint-resources"></a>Requisito previo: Creación de recursos de punto de conexión

Para vincular un punto de conexión a Azure Digital Twins, es necesario que ya exista el tema de Event Grid, el centro de eventos o el tema de Service Bus que se use para el punto de conexión.

Utilice el gráfico siguiente para ver qué recursos se deben configurar antes de crear el punto de conexión.

| Tipo de punto de conexión | Recursos necesarios (vinculados a instrucciones de creación) |
| --- | --- |
| Punto de conexión de Event Grid | [Tema de Event Grid](../articles/event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) |
| Punto de conexión de Event Hubs | [Espacio de nombres de&nbsp;Event&nbsp;Hubs](../articles/event-hubs/event-hubs-create.md)<br/><br/>[centro de eventos](../articles/event-hubs/event-hubs-create.md)<br/><br/>(Opcional) [regla de autorización](../articles/event-hubs/authorize-access-shared-access-signature.md) para la autenticación basada en claves | 
| Extremo del bus de servicio | [Espacio de nombres de Service Bus](../articles/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md)<br/><br/>[Tema de Service Bus](../articles/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md)<br/><br/> (Opcional) [regla de autorización](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md#shared-access-signature) para la autenticación basada en claves|
