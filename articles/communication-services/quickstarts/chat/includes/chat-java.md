---
title: archivo de inclusión
description: archivo de inclusión
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 146053ffd72b24216bfa86577787727257da2516
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495429"
---
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Kit de desarrollo de Java (JDK)](/java/azure/jdk/), versión 8 o posterior.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Un recurso de Communication Services implementado y una cadena de conexión. [Cree un recurso de Communication Services](../../create-communication-resource.md).
- Un [Token de acceso de usuario](../../access-tokens.md). Asegúrese de establecer el ámbito en "chat" y anote la cadena del token, así como la cadena userId.


## <a name="setting-up"></a>Instalación

### <a name="create-a-new-java-application"></a>Creación de una aplicación Java

Abra la ventana de terminal o de comandos y navegue hasta el directorio en el que quiere crear la aplicación de Java. Ejecute el siguiente comando para generar el proyecto de Java a partir de la plantilla maven-archetype-quickstart.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

Observará que el objetivo "generar" creó un directorio con el mismo nombre que el objeto artifactId. En este directorio, el elemento `src/main/java directory` contiene el código fuente del proyecto, el directorio `src/test/java` contiene el origen de la prueba y el archivo pom.xml es el modelo de objetos del proyecto o [POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html).

Actualice el archivo POM de la aplicación para usar Java 8 o posterior:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

### <a name="add-the-package-references-for-the-chat-client-library"></a>Adición de las referencias de paquete para la biblioteca de cliente de chat

En el archivo POM, haga referencia al paquete `azure-communication-chat` con las API de chat:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-chat</artifactId>
    <version>1.0.0-beta.4</version> 
</dependency>
```

Para la autenticación, el cliente debe hacer referencia al paquete `azure-communication-common`:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-common</artifactId>
    <version>1.0.0</version> 
</dependency>
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca cliente de chat de Azure Communication Services para Java.

| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Esta clase es necesaria para la funcionalidad de chat. Cree una instancia de esta con la información de suscripción y úsela para crear, obtener y eliminar subprocesos. |
| ChatAsyncClient | Esta clase es necesaria para la funcionalidad de chat asincrónica. Cree una instancia de esta con la información de suscripción y úsela para crear, obtener y eliminar subprocesos. |
| ChatThreadClient | Esta clase es necesaria para la funcionalidad de subproceso de chat. Obtiene una instancia de a través de la clase ChatClient y la usa para enviar, recibir, actualizar o eliminar mensajes, agregar, quitar u obtener usuarios, enviar notificaciones de escritura y confirmaciones de lectura. |
| ChatThreadAsyncClient | Esta clase es necesaria para la funcionalidad de subproceso de chat asincrónica. Obtiene una instancia de a través de la clase ChatAsyncClient y la usa para enviar, recibir, actualizar o eliminar mensajes, agregar, quitar u obtener usuarios, enviar notificaciones de escritura y confirmaciones de lectura. |

## <a name="create-a-chat-client"></a>Creación de un cliente de chat
Para crear un cliente de chat, usará el punto de conexión de Communication Services y el token de acceso que se generó como parte de los pasos de requisitos previos. Los tokens de acceso de usuario permiten compilar aplicaciones cliente que se autentiquen directamente en Azure Communication Services. Después de generar estos tokens en el servidor, vuelva a pasarlos a un dispositivo cliente. Debe usar la clase CommunicationTokenCredential de la biblioteca de cliente común para pasar el token al cliente de chat. 

[Más información sobre la arquitectura del chat](../../../concepts/chat/concepts.md).

Al agregar las instrucciones de importación, asegúrese de agregar solo las importaciones de los espacios de nombres com.azure.communication.chat y com.azure.communication.chat.models, y no del espacio de nombres com.azure.communication.chat.implementation. En el archivo App.java que se generó a través de Maven, puede usar el código siguiente para empezar con:

```Java
import com.azure.communication.chat.*;
import com.azure.communication.chat.models.*;
import com.azure.communication.common.*;
import com.azure.core.http.HttpClient;

import java.io.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Chat Quickstart");
        
        // Your unique Azure Communication service endpoint
        String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

        // Create an HttpClient builder of your choice and customize it
        // Use com.azure.core.http.netty.NettyAsyncHttpClientBuilder if that suits your needs
        NettyAsyncHttpClientBuilder yourHttpClientBuilder = new NettyAsyncHttpClientBuilder();
        HttpClient httpClient = yourHttpClientBuilder.build();

        // User access token fetched from your trusted service
        String userAccessToken = "<USER_ACCESS_TOKEN>";

        // Create a CommunicationTokenCredential with the given access token, which is only valid until the token is valid
        CommunicationTokenCredential userCredential = new CommunicationTokenCredential(userAccessToken);

        // Initialize the chat client
        final ChatClientBuilder builder = new ChatClientBuilder();
        builder.endpoint(endpoint)
            .credential(userCredential)
            .httpClient(httpClient);
        ChatClient chatClient = builder.buildClient();
    }
}
```


## <a name="start-a-chat-thread"></a>Inicio de un subproceso de chat

Use el método `createChatThread` para crear un subproceso de chat.
`createChatThreadOptions` se usa para describir la solicitud de subproceso.

- Use `topic` para proporcionar un tema a este chat; el tema puede actualizarse después de crear el subproceso de chat mediante la función `UpdateThread`.
- Use `participants` para enumerar los participantes de la conversación que se van a agregar a la misma. `ChatParticipant` toma el usuario que ha creado en el inicio rápido de [Token de acceso de usuario](../../access-tokens.md).

La respuesta `chatThreadClient` se usa para realizar operaciones en el subproceso de chat creado: agregar participantes a la conversación del chat, enviar o eliminar un mensaje, etc. Contiene una propiedad `chatThreadId` que es el id. único del subproceso de chat. La propiedad es accesible para el método público .getChatThreadId().

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(firstUser)
    .setDisplayName("Participant Display Name 1");
    
ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(secondUser)
    .setDisplayName("Participant Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

CreateChatThreadOptions createChatThreadOptions = new CreateChatThreadOptions()
    .setTopic("Topic")
    .setParticipants(participants);
ChatThreadClient chatThreadClient = chatClient.createChatThread(createChatThreadOptions);
String chatThreadId = chatThreadClient.getChatThreadId();
```

## <a name="send-a-message-to-a-chat-thread"></a>Envío de un mensaje a un subproceso de chat

Utilice el método `sendMessage` para enviar un mensaje al subproceso recién creado, identificado mediante chatThreadId.
`sendChatMessageOptions` se utiliza para describir la solicitud del mensaje de chat.

- Utilice `content` para proporcionar el contenido del mensaje de chat.
- Utilice `type` usa para especificar el tipo de contenido del mensaje de chat, texto o HTML.
- Utilice `senderDisplayName` para especificar el nombre para mostrar del remitente.

La respuesta `sendChatMessageResult` contiene un `id`, que es el identificador único del mensaje.

```Java
SendChatMessageOptions sendChatMessageOptions = new SendChatMessageOptions()
    .setContent("Message content")
    .setType(ChatMessageType.TEXT)
    .setSenderDisplayName("Sender Display Name");

SendChatMessageResult sendChatMessageResult = chatThreadClient.sendMessage(sendChatMessageOptions);
String chatMessageId = sendChatMessageResult.getId();
```


## <a name="get-a-chat-thread-client"></a>Obtención de un cliente de subproceso de chat

El método `getChatThreadClient` devuelve un cliente de subproceso para un subproceso que ya existe. Se puede usar para realizar operaciones en la conversación creada: agregar participantes, enviar un mensaje, etc. `chatThreadId` es el identificador único de la conversación del chat existente.

```Java
String chatThreadId = "Id";
ChatThread chatThread = chatClient.getChatThread(chatThreadId);
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Recepción de mensajes de chat de un subproceso de chat

Puede recuperar mensajes de chat mediante el sondeo del método `listMessages` en el cliente de subproceso de chat a intervalos especificados.

```Java
chatThreadClient.listMessages().iterableByPage().forEach(resp -> {
    System.out.printf("Response headers are %s. Url %s  and status code %d %n", resp.getHeaders(),
        resp.getRequest().getUrl(), resp.getStatusCode());
    resp.getItems().forEach(message -> {
        System.out.printf("Message id is %s.", message.getId());
    });
});
```

`listMessages` devuelve la versión más reciente del mensaje, incluidas las modificaciones o eliminaciones que se han producido en este mediante .editMessage() y .deleteMessage(). En el caso de los mensajes eliminados, `chatMessage.getDeletedOn()` devuelve un valor de fecha y hora que indica cuándo se eliminó el mensaje. En el caso de los mensajes editados, `chatMessage.getEditedOn()` devuelve un valor de fecha y hora que indica cuándo se editó el mensaje. Es posible acceder a la hora original de creación del mensaje mediante `chatMessage.getCreatedOn()` y se puede usar para ordenar los mensajes.

`listMessages` devuelve distintos tipos de mensajes que se pueden identificar mediante `chatMessage.getType()`. Estos tipos son:

- `text`: mensaje de chat normal enviado por un participante de la conversación.

- `html`: mensaje de chat en formato HTML enviado por un participante de la conversación.

- `topicUpdated`: mensaje del sistema que indica que el tema se ha actualizado.

- `participantAdded`: mensaje del sistema que indica que se han agregado uno o varios miembros a la conversación del chat.

- `participantRemoved`: mensaje del sistema que indica que se ha eliminado un miembro de la conversación del chat.

Para obtener más información, consulte [Tipos de mensajes](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Adición de un usuario como participante a la conversación del chat

Una vez que se crea un subproceso de chat, puede agregar y quitar usuarios de este. Al agregar usuarios, les concede acceso para enviar mensajes a la conversación del chat, y agregar o quitar otros participantes. Deberá empezar por obtener un token de acceso y una identidad nuevos para ese usuario. Antes de llamar al método addParticipants, asegúrese de que ha adquirido un token de acceso y una identidad nuevos para ese usuario. El usuario necesitará ese token de acceso para poder inicializar su cliente de chat.

Utilice el método `addParticipants` para agregar participantes a la conversación identificada por threadId.

- Use `listParticipants` para enumerar los participantes que se van a agregar a la conversación del chat.
- `communicationIdentifier` es obligatorio y es el elemento CommunicationIdentifier que creó mediante CommunicationIdentityClient en el inicio rápido [Token de acceso de usuario](../../access-tokens.md).
- `display_name` es opcional y es el nombre para mostrar del participante de la conversación.
- `share_history_time` es opcional y es la hora a partir de la cual el historial de chat se compartió con el participante. Para compartir el historial desde el inicio del subproceso de chat, establezca esta propiedad en cualquier fecha igual o anterior a la hora de creación del subproceso. Para no compartir ningún historial anterior a la hora en que se agregó el participante, establézcala en la fecha actual. Para compartir el historial parcialmente, establezca la opción en la fecha necesaria.

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity1)
    .setDisplayName("Display Name 1");

ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity2)
    .setDisplayName("Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

AddChatParticipantsOptions addChatParticipantsOptions = new AddChatParticipantsOptions()
    .setParticipants(participants);
chatThreadClient.addParticipants(addChatParticipantsOptions);
```

## <a name="remove-participant-from-a-chat-thread"></a>Eliminación del participante de una conversación de chat

En una conversación del chat, los participantes se pueden eliminar de forma similar a como se agregan. Para ello, debe realizar un seguimiento de las identidades de los participantes que ha agregado.

Use `removeParticipant`, donde `identifier` es el elemento CommunicationIdentifier que ha creado.

```Java
chatThreadClient.removeParticipant(identity);
```

## <a name="run-the-code"></a>Ejecución del código

Navegue hasta el directorio que contiene el archivo *pom.xml* y compile el proyecto mediante el siguiente comando `mvn`.

```console
mvn compile
```

A continuación, compile el paquete.

```console
mvn package
```

Ejecute el siguiente comando `mvn` para ejecutar la aplicación.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```