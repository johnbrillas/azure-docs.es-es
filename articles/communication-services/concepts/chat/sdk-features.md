---
title: Información general de la biblioteca cliente de chat para Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre la biblioteca cliente de chat de Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 705bd926c2ac6f414464254969b5c511c88891f0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656114"
---
# <a name="chat-client-library-overview"></a>Información general de la biblioteca cliente de chat  

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]    

Las bibliotecas cliente de chat de Azure Communication Services se pueden usar para agregar chats enriquecidos en tiempo real a las aplicaciones.
    
## <a name="chat-client-library-capabilities"></a>Funcionalidades de la biblioteca cliente de chat 

En la lista siguiente se presenta el conjunto de características que están disponibles actualmente en las bibliotecas cliente de chat de Communication Services.  

| Grupo de características | Capacidad | JavaScript  | Java | .NET | Python | iOS | Android |
|-----------------|-------------------|---|-----|----|-----|----|----|
| Funcionalidades principales | Crear una conversación de chat entre 2 o más usuarios (hasta 250 usuarios)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |    
|                   | Actualizar el tema de una conversación de chat                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Incorporación o eliminación de participantes de una conversación de chat                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | Elegir si desea compartir el historial de mensajes de chat con el participante que se va a agregar                                   | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   | 
|                   | Obtener una lista de participantes de una conversación de chat                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   | 
|                   | Eliminar una conversación de chat                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Dado un usuario de comunicación, obtenga la lista de conversaciones de chat de las que forma parte el usuario.                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Obtener información sobre una conversación de chat determinada                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Enviar y recibir mensajes en una conversación de chat                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Editar el contenido de un mensaje enviado                                                                                | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Eliminar un mensaje                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Confirmaciones de lectura de mensajes leídos por otros participantes de un chat <br/> *No está disponible cuando hay más de 20 participantes en una conversación de chat*    | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Recibir notificaciones cuando los participantes escriben activamente un mensaje en una conversación de chat <br/> *No está disponible cuando hay más de 20 miembros en una conversación de chat*      | ✔️   | ✔️   | ✔️    | ✔️    |  ✔️    | ✔️   | 
|                   | Obtener todos los mensajes de una conversación de chat <br/>                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | Enviar emojis de Unicode como parte del contenido del mensaje                                                                            | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|Señalización en tiempo real (habilitada por un paquete de señalización propietario**)|  Suscribirse para obtener actualizaciones en tiempo real de los mensajes entrantes y otras operaciones de la aplicación de chat. Para ver una lista de las actualizaciones admitidas para la señalización en tiempo real, consulte [Conceptos de chat](concepts.md#real-time-signaling).                                     | ✔️   | ❌    | ❌  | ❌  | ❌  | ❌  |    
| Compatibilidad con Event Grid             | Usar la integración con Azure Event Grid y configurar el servicio de comunicación para ejecutar la lógica de negocios basada en la actividad de chat o para conectar un servicio de notificaciones de inserción personalizado   | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
| Supervisión        | Usar las métricas de solicitud de API emitidas en Azure Portal para compilar paneles, supervisar el estado de la aplicación de chat y establecer alertas para detectar anomalías      | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Configurar el recurso de Communication Services para que reciba registros operativos del chat con fines de supervisión y diagnóstico          | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  


** El paquete de señalización propietario se implementa mediante sockets web. Si no se admiten los sockets web, se reservará a un sondeo prolongado.  

## <a name="javascript-chat-client-library-support-by-os-and-browser"></a>Compatibilidad de la biblioteca cliente de chat de JavaScript por sistema operativo y explorador 

En la tabla siguiente se representa el conjunto de exploradores y versiones compatibles que están disponibles actualmente.
    
|                                  | Windows          | macOS          | Ubuntu | Linux  | Android | iOS    | SO de iPad|
|--------------------------------|----------------|--------------|-------|------|------|------|-------|
| **Biblioteca cliente de chat** | Firefox *, Chrome*, nuevo Edge | Firefox *, Chrome*, Safari* | Chrome*  | Chrome* | Chrome* | Safari* | Safari* |

\* Tenga en cuenta que se admite la versión más reciente además de las dos versiones anteriores.<br/>   

## <a name="next-steps"></a>Pasos siguientes   

> [!div class="nextstepaction"] 
> [Introducción al chat](../../quickstarts/chat/get-started.md)    

Puede que los siguientes documentos le resulten interesantes:  
- Familiarícese con los [conceptos del chat](../chat/concepts.md)