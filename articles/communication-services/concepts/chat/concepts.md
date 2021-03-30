---
title: Conceptos de chat en Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre los conceptos de chat de Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e71917d917491037100b4ee666cbeb030b0a4786
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656148"
---
# <a name="chat-concepts"></a>Conceptos de chat 

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]    

Las bibliotecas cliente de chat de Azure Communication Services se pueden usar para agregar chat de texto en tiempo real a las aplicaciones. En esta página se resumen las funcionalidades y conceptos clave del chat.    

Consulte la [Información general sobre la biblioteca cliente de chat de Communication Services](./sdk-features.md) para obtener más información sobre los lenguajes y funcionalidades específicos de la biblioteca cliente.    

## <a name="chat-overview"></a>Introducción al chat    

Las conversaciones de chat se producen dentro de los hilos o conversaciones de chat. Una conversación de chat puede contener muchos mensajes y muchos usuarios. Cada mensaje pertenece a una conversación única, y un usuario puede formar parte de una o varias conversaciones. A cada usuario de la conversación de chat se le llama participante. Solo los participantes de la conversación pueden enviar y recibir mensajes y agregar o quitar otros usuarios en una conversación de chat. Communication Services almacena el historial del chat hasta que se ejecuta una operación de eliminación en la conversación o el mensaje de chat, o bien hasta que no queden participantes en la conversación de chat, en cuyo momento se queda huérfana y se pone en cola para su eliminación. 
    
## <a name="service-limits"></a>Límites de servicio   

- El número máximo de participantes permitido en una conversación de chat es de 250.   
- El tamaño máximo de mensaje permitido es de aproximadamente 28 KB.  
- En el caso de las conversaciones de chat con más de 20 participantes, no se admiten las características de confirmación de lectura e indicador de escritura.    
- 
## <a name="chat-architecture"></a>Arquitectura del chat    

Hay dos componentes principales en la arquitectura de chat: El 1) servicio de confianza y la 2) aplicación cliente.    

:::image type="content" source="../../media/chat-architecture.png" alt-text="Diagrama que muestra la arquitectura de chat de Communication Services."::: 

 - **Servicio de confianza:** para administrar correctamente una sesión de chat, necesita un servicio que le ayude a conectarse a Communication Services mediante la cadena de conexión del recurso. Este servicio es responsable de la creación de las conversaciones de chat, la administración de las listas de participantes de la conversación y el suministro de tokens de acceso a los usuarios. Puede encontrar más información sobre los tokens de acceso en la guía de inicio rápido sobre [tokens de acceso](../../quickstarts/access-tokens.md).   
 - **Aplicación cliente:**  la aplicación cliente se conecta al servicio de confianza y recibe los tokens de acceso que se usan para conectarse directamente a Communication Services. Una vez realizada la conexión, la aplicación cliente puede enviar y recibir mensajes.   
Se recomienda generar tokens de acceso mediante el nivel de servicio de confianza. En este escenario, el servidor sería el responsable no solo de la creación y administración de usuarios, sino también de la emisión de sus tokens.   
        
## <a name="message-types"></a>Tipos de mensaje    

El chat de Communication Services comparte los mensajes generados por el usuario, así como los mensajes generados por el sistema, denominados **actividades de conversación**. Las actividades de conversación se generan cuando se actualiza una conversación de chat. Cuando se llama a `List Messages` o `Get Messages` en una conversación de chat, el resultado contiene los mensajes de texto generados por el usuario, así como los mensajes del sistema en orden cronológico. Esto ayuda a identificar cuándo se ha agregado o quitado un participante, o cuándo se ha actualizado el tema de la conversación de chat. Los siguientes son los tipos de mensajes admitidos:  
    
 - `Text`: un mensaje de texto sin formato creado y enviado por el usuario como parte de la conversación de chat. 
 - `RichText/HTML`: un mensaje de texto con formato. Tenga en cuenta que los usuarios de Communication Services actualmente no pueden enviar mensajes de texto enriquecido. Este tipo de mensaje es compatible con los mensajes enviados desde usuarios a de Teams a usuarios de Communication Services en escenarios de interoperabilidad de Teams.   
 - `ThreadActivity/ParticipantAdded`: mensaje del sistema que indica que se han agregado uno o varios miembros a la conversación del chat. Por ejemplo: 


``` 
{   
            "id": "1613589626560",  
            "type": "participantAdded", 
            "sequenceId": "7",  
            "version": "1613589626560", 
            "content":  
            {   
                "participants": 
                [   
                    {   
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",    
                        "displayName": "Jane",  
                        "shareHistoryTime": "1970-01-01T00:00:00Z"  
                    }   
                ],  
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:26Z" 
        }   
``` 

- `ThreadActivity/ParticipantRemoved`: mensaje del sistema que indica que se ha eliminado un miembro de la conversación del chat. Por ejemplo:  

``` 
{   
            "id": "1613589627603",  
            "type": "participantRemoved",   
            "sequenceId": "8",  
            "version": "1613589627603", 
            "content":  
            {   
                "participants": 
                [   
                    {   
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",    
                        "displayName": "Jane",  
                        "shareHistoryTime": "1970-01-01T00:00:00Z"  
                    }   
                ],  
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:27Z" 
        }   
``` 

- `ThreadActivity/TopicUpdate`: mensaje del sistema que indica que el tema del subproceso se ha actualizado. Por ejemplo:   
``` 
{   
            "id": "1613589623037",  
            "type": "topicUpdated", 
            "sequenceId": "2",  
            "version": "1613589623037", 
            "content":  
            {   
                "topic": "New topic",   
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:23Z" 
        }   
``` 

## <a name="real-time-signaling"></a>Señalización en tiempo real  

La biblioteca cliente de chat para JavaScript incluye señalización en tiempo real. De este modo, los clientes pueden escuchar actualizaciones en tiempo real y mensajes entrantes en una conversación de chat sin tener que sondear las API. Entre los eventos disponibles se incluye:

 - `ChatMessageReceived`: cuando se envía un mensaje nuevo a una conversación de chat. Este evento no se envía para los mensajes del sistema generados automáticamente, como se describió en el tema anterior.   
 - `ChatMessageEdited`: cuando se edita un mensaje en una conversación de chat. 
 - `ChatMessageDeleted`: cuando se elimina un mensaje en una conversación de chat.   
 - `TypingIndicatorReceived`: cuando otro participante está escribiendo un mensaje en una conversación de chat.   
 - `ReadReceiptReceived`: cuando otro participante ha leído el mensaje que el usuario envió en una conversación de chat.     
 - `ChatThreadCreated`: cuando un usuario de Communication Services crea una conversación de chat. 
 - `ChatThreadDeleted`: cuando un usuario de Communication Services elimina una conversación de chat. 
 - `ChatThreadPropertiesUpdated`: cuando se actualizan las propiedades de la conversación de chat; actualmente, solo se admite la actualización del tema de la conversación.   
 - `ParticipantsAdded`: cuando se agrega un usuario como participante a una conversación de chat.  
 - `ParticipantsRemoved`: cuando se quita un participante existente de la conversación de chat.


## <a name="chat-events"></a>Eventos de chat  

La señalización en tiempo real permite que los usuarios conversen en tiempo real. Los servicios pueden usar Azure Event Grid para suscribirse a los eventos relacionados con el chat. Para obtener más información, consulte los [conceptos del control de eventos](https://docs.microsoft.com/azure/event-grid/event-schema-communication-services?tabs=event-grid-event-schema).


## <a name="using-cognitive-services-with-chat-client-library-to-enable-intelligent-features"></a>Uso de Cognitive Services con la biblioteca cliente de chat para habilitar características inteligentes 

Puede usar [Azure Cognitive APIs](../../../cognitive-services/index.yml) con la biblioteca cliente de chat para agregar características inteligentes a sus aplicaciones. Por ejemplo, puede:  

- Permitir a los usuarios conversar entre sí en distintos idiomas.  
- Ayudar a un agente de soporte técnico a priorizar los vales mediante la detección de una opinión negativa en un nuevo problema de un cliente.   
- Analizar los mensajes entrantes para detectar claves reconocer entidades, así como solicitar información pertinente al usuario en la aplicación en función del contenido del mensaje.

Una manera de lograrlo es hacer que su servicio de confianza actúe como participante de una conversación de chat. Supongamos que desea quiere la traducción de idiomas. Este servicio será responsable de escuchar los mensajes que intercambian otros participantes [1], llamar a Cognitive Services APIs para traducir el contenido al idioma deseado [2, 3] y enviar el resultado traducido como mensaje en la conversación de chat [4].

De este modo, el historial de mensajes contendrá los mensajes originales y los traducidos. En la aplicación cliente, puede agregar lógica para mostrar el mensaje original o el traducido. Consulte [esta guía de inicio rápido](../../../cognitive-services/translator/quickstart-translator.md) para más información sobre cómo usar Cognitive Services APIs para traducir texto a diferentes idiomas. 
    
:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Diagrama que muestra a Cognitive Services interactuando con Communication Services."::: 

## <a name="next-steps"></a>Pasos siguientes   

> [!div class="nextstepaction"] 
> [Introducción al chat](../../quickstarts/chat/get-started.md)    

Puede que los siguientes documentos le resulten interesantes:  
- Familiarícese con nuestra [biblioteca cliente de chat](sdk-features.md)