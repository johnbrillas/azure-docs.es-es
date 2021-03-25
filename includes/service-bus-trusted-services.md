---
title: archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 03/08/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd4d5de5d93e4aea862aaabd10fb5d3c6d45cb1c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102510888"
---
## <a name="trusted-microsoft-services"></a>Servicios de Microsoft de confianza
Al activar la opción **¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?** , los siguientes servicios adquieren acceso a los recursos de Service Bus.

| Servicio de confianza | Escenarios de uso admitidos | 
| --------------- | ------------------------- | 
| Azure Event Grid | Permite a Azure Event Grid enviar eventos a colas o temas en el espacio de nombres de Service Bus. También debe seguir estos pasos: <ul><li>Habilite la identidad asignada por el sistema para un tema o un dominio</li><li>Agregue la identidad al rol Remitente de datos de Azure Service Bus en el espacio de nombres de Service Bus</li><li>A continuación, configure la suscripción de eventos que emplea una cola o tema de Service Bus como punto de conexión para usar la identidad asignada por el sistema.</li></ul> <p>Para obtener más información, vea [Entrega de evento con una identidad administrada](../articles/event-grid/managed-service-identity.md)</p>|
| Azure API Management | <p>El servicio API Management le permite enviar mensajes a una cola o un tema de Service Bus en el espacio de nombres de Service Bus.</p><ul><li>Puede desencadenar flujos de trabajo personalizados enviando mensajes a la cola o el tema de Service Bus cuando se invoca una API mediante la [directiva send-request](../articles/api-management/api-management-sample-send-request.md).</li><li>También puede tratar una cola o un tema de Service Bus como back-end en una API. Para ver una directiva de ejemplo, vea [Autenticación mediante una identidad administrada para acceder a una cola o un tema de Service Bus](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Authenticate%20using%20Managed%20Identity%20to%20access%20Service%20Bus.xml). También debe seguir estos pasos:<ol><li>Habilite una identidad asignada por el sistema en una instancia de API Management. Para instrucciones, vea [Uso de identidades administradas en Azure API Management](../articles/api-management/api-management-howto-use-managed-service-identity.md).</li><li>Agregue la identidad al rol **Remitente de datos de Azure Service Bus** en el espacio de nombres de Service Bus.</li></ol></li></ul> | 