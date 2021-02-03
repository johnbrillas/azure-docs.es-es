---
title: 'Inicio rápido: unirse a una reunión de Teams'
author: askaur
ms.author: askaur
ms.date: 12/08/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 780ef2bbb7851d8bef5fc52a51421a7938043ecb
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98932295"
---
## <a name="join-the-meeting-chat"></a>Unión al chat de la reunión 

Una vez que la interoperabilidad de Teams se habilita, cualquier usuario de Communication Services puede unirse a la llamada de Teams como usuario invitado mediante la biblioteca cliente que llama. Al unirse a la llamada se le agregará también como participante en el chat de la reunión, donde podrán enviar mensajes a otros usuarios durante la llamada, y recibirlos de ellos. El usuario no tendrá acceso a los mensajes del chat enviados antes de que se haya unido a la llamada. 

## <a name="get-a-teams-meeting-chat-thread-for-a-communication-services-user"></a>Obtención de un subproceso del chat de la reunión para un usuario de Communication Services

En primer lugar, cree una instancia de `ChatThreadClient` para el subproceso del chat de reunión. Para obtener el identificador del subproceso, analice el vínculo de la reunión o use las Graph API con el identificador de reunión. 

- Así es el vínculo de una reunión en Teams: `https://teams.microsoft.com/l/meetup-join/meeting_chat_thread_id/1606337455313?context=some_context_here`. El identificador del subproceso será el lugar en que `meeting_chat_thread_id` esté en ese vínculo. 
- Si tiene el identificador de la reunión, puede usar [Graph API](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta) para obtener el identificador del subproceso. La respuesta de [GET API](/graph/api/onlinemeeting-get?tabs=http%22+%5c&view=graph-rest-beta) tendrá un objeto `chatInfo` que contiene el elemento `threadID`. 

Una vez que se tenga el identificador de subproceso del chat, el cliente del subproceso del chat se puede obtener mediante la biblioteca de cliente de chat de JavaScript: 

```javascript
let chatThreadClient = await chatClient.getChatThreadClient(threadId); 

console.log(`Chat Thread client for threadId:${chatThreadClient.threadId}`); 
```
  
`chatThreadClient` se puede usar para enumerar los miembros del subproceso del chat, obtener el historial del chat y enviar mensajes.  

## <a name="send-and-receive-messages"></a>Envío y recepción de mensajes  

Use `SendMessage` para enviar un mensaje al chat de la reunión. Para recibir los mensajes entrantes, la capacidad para suscribirse a eventos como `chatMessageReceived` no se admite, ya que la señalización en tiempo real todavía no está habilitada para este escenario. Para obtener los mensajes más recientes, puede sondear `ListMessages` API. En el escenario de interoperabilidad, `ListMessages` API ahora admite la devolución de tres nuevos tipos de mensaje:
- `RichText/HTML`
- `ThreadActivity/MemberJoined`
- `ThreadActivity/MemberLeft` </br>

Para más información sobre los tipos de mensaje, haga clic [aquí](../../../concepts/chat/concepts.md). 

**Nota**: actualmente solo se admite el envío y la recepción de mensajes para escenarios de interoperabilidad con Teams. Otras características, como los indicadores de escritura y que los usuarios de Communication Services agreguen o quiten otros usuarios de la reunión de Teams aún no se admiten.  

