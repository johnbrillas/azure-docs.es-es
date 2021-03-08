---
title: archivo de inclusión
description: archivo de inclusión
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 0225c948fddf65b9312c689144ecc567a70aa27e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749992"
---
## <a name="prerequisites"></a>Requisitos previos
Antes de comenzar, compruebe lo siguiente:

- Cree de una cuenta de Azure con una suscripción activa. Para más información, consulte [Creación de una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Instalación de [Python](https://www.python.org/downloads/)
- Cree un recurso de Azure Communication Services. Para obtener más información, consulte [Creación de un recurso de Azure Communication Services](../../create-communication-resource.md). Deberá registrar el **punto de conexión** del recurso para esta guía de inicio rápido.
- Un [Token de acceso de usuario](../../access-tokens.md). Asegúrese de establecer el ámbito en "chat" y anote la cadena del token, así como la cadena userId.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Abra la ventana de comandos o de terminal, cree un nuevo directorio para la aplicación y navegue hasta este.

```console
mkdir chat-quickstart && cd chat-quickstart
```

Use un editor de texto para crear un archivo denominado **start-chat.py** en el directorio raíz del proyecto y agregue la estructura del programa, incluido el control de excepciones básico. En las secciones siguientes agregará todo el código fuente de esta guía de inicio rápido a este archivo.

```python
import os
# Add required client library components from quickstart here

try:
    print('Azure Communication Services - Chat Quickstart')
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-client-library"></a>Instalación de la biblioteca cliente

```console

pip install azure-communication-chat

```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca cliente de chat de Azure Communication Services para Python.

| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Esta clase es necesaria para la funcionalidad de chat. Cree una instancia de esta con la información de suscripción y úsela para crear, obtener y eliminar subprocesos. |
| ChatThreadClient | Esta clase es necesaria para la funcionalidad de subproceso de chat. Obtiene una instancia de a través de la clase ChatClient y la usa para enviar, recibir, actualizar o eliminar mensajes, agregar, quitar u obtener usuarios, enviar notificaciones de escritura y confirmaciones de lectura. |

## <a name="create-a-chat-client"></a>Creación de un cliente de chat

Para crear un cliente de chat, usará el punto de conexión de Communication Services y el `Access Token` que se generó como parte de los pasos de requisitos previos. Obtenga más información sobre los [tokens de acceso de usuario](../../access-tokens.md).

Este inicio rápido no cubre la creación de un nivel de servicio para administrar tokens para la aplicación de chat, aunque se recomienda. Para más información, consulte [Arquitectura del chat](../../../concepts/chat/concepts.md).

```console
pip install azure-communication-administration
```

```python
from azure.communication.chat import ChatClient, CommunicationTokenCredential, CommunicationTokenRefreshOptions

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
refresh_options = CommunicationTokenRefreshOptions(<Access Token>)
chat_client = ChatClient(endpoint, CommunicationTokenCredential(refresh_options))
```

## <a name="start-a-chat-thread"></a>Inicio de un subproceso de chat

Use el método `create_chat_thread` para crear un subproceso de chat.

- Use `topic` para proporcionar un tema de subproceso; el tema puede actualizarse después de crear el subproceso de chat mediante la función `update_thread`.
- Use `thread_participants` para enumerar el `ChatThreadParticipant` que se va a agregar al subproceso de chat, el elemento `ChatThreadParticipant` toma el tipo `CommunicationUserIdentifier` como `user`, que es el valor que obtuvo después de crearlo con [Creación de un usuario](../../access-tokens.md#create-an-identity).
- Use `repeatability_request_id` para indicar que la solicitud se puede repetir. El cliente puede realizar la solicitud varias veces con el mismo valor Repeatability-Request-ID y obtener una respuesta adecuada sin que el servidor tenga que ejecutar la solicitud varias veces.

La respuesta `chat_thread_client` se usa para realizar operaciones en la conversación de chat recién creada, como agregar participantes a la conversación, enviar o eliminar un mensaje, etc. Contiene una propiedad `thread_id` que es el id. único del subproceso de chat.

#### <a name="without-repeatability-request-id"></a>Sin Repeatability-Request-ID
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

chat_thread_client = chat_client.create_chat_thread(topic, participants)
```

#### <a name="with-repeatability-request-id"></a>Con Repeatability-Request-ID
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

repeatability_request_id = 'b66d6031-fdcc-41df-8306-e524c9f226b8' # some unique identifier
chat_thread_client = chat_client.create_chat_thread(topic, participants, repeatability_request_id)
```

## <a name="get-a-chat-thread-client"></a>Obtención de un cliente de subproceso de chat
El método `get_chat_thread_client` devuelve un cliente de subproceso para un subproceso que ya existe. Se puede usar para realizar operaciones en la conversación creada: agregar participantes, enviar un mensaje, etc. thread_id es el identificador único de la conversación de chat existente.

```python
thread_id = chat_thread_client.thread_id
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```

## <a name="list-all-chat-threads"></a>Lista de todas las conversaciones de chat
El método `list_chat_threads` devuelve un iterador de tipo `ChatThreadInfo`. Se puede usar para enumerar todas las conversaciones de chat.

- Utilice `start_time` para especificar el primer punto en el tiempo a partir del cual obtener las conversaciones de chat.
- Utilice `results_per_page` para especificar el número máximo de conversaciones de chat que se devuelven por página.

```python
from datetime import datetime, timedelta
import pytz

start_time = datetime.utcnow() - timedelta(days=2)
start_time = start_time.replace(tzinfo=pytz.utc)
chat_thread_infos = chat_client.list_chat_threads(results_per_page=5, start_time=start_time)

for chat_thread_info_page in chat_thread_infos.by_page():
    for chat_thread_info in chat_thread_info_page:
        # Iterate over all chat threads
        print("thread id:", chat_thread_info.id)
```

## <a name="delete-a-chat-thread"></a>Eliminar una conversación de chat
Para eliminar una conversación de chat, se utiliza `delete_chat_thread`.

- Se usa `thread_id` para especificar el thread_id de una conversación de chat existente que debe eliminarse.

```python
thread_id = chat_thread_client.thread_id
chat_client.delete_chat_thread(thread_id)
```

## <a name="send-a-message-to-a-chat-thread"></a>Envío de un mensaje a un subproceso de chat

Utilice el método `send_message` para enviar un mensaje a una conversación de chat recién creada, identificada mediante thread_id.

- Utilice `content` para proporcionar el contenido del mensaje de chat.
- Utilice `chat_message_type` para especificar el tipo de contenido del mensaje. Los valores posibles son "text" y "html"; si no se especifica, se asigna el valor predeterminado "text".
- Utilice `sender_display_name` para especificar el nombre para mostrar del remitente.

La respuesta es un identificador de tipo `str` que corresponde al identificador único del mensaje.

#### <a name="message-type-not-specified"></a>Tipo de mensaje no especificado
```python
chat_thread_client = chat_client.create_chat_thread(topic, participants)

content='hello world'
sender_display_name='sender name'

send_message_result_id = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name)
```

#### <a name="message-type-specified"></a>Tipo de mensaje especificado
```python
from azure.communication.chat import ChatMessageType

content='hello world'
sender_display_name='sender name'

# specify chat message type with pre-built enumerations
send_message_result_id_w_enum = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type=ChatMessageType.TEXT)

# specify chat message type as string
send_message_result_id_w_str = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type='text')
```

## <a name="get-a-specific-chat-message-from-a-chat-thread"></a>Obtención de un mensaje específico de una conversación de chat
La función `get_message` se puede utilizar para recuperar un mensaje específico, identificado por un valor message_id.

- Utilice `message_id` para especificar el identificador del mensaje.

La respuesta de tipo `ChatMessage` contiene toda la información relacionada con el mensaje único.

```python
message_id = send_message_result_id
chat_message = chat_thread_client.get_message(message_id)
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Recepción de mensajes de chat de un subproceso de chat

Puede recuperar mensajes de chat mediante el sondeo del método `list_messages` a intervalos especificados.

- Utilice `results_per_page` para especificar el número máximo de mensajes que se devuelven por página.
- Utilice `start_time` para especificar el primer punto en el tiempo a partir del cual obtener los mensajes.

```python
chat_messages = chat_thread_client.list_messages(results_per_page=1, start_time=start_time)
for chat_message_page in chat_messages.by_page():
    for chat_message in chat_message_page:
        print('ChatMessage: ', chat_message)
        print('ChatMessage: ', chat_message.content.message)
```

`list_messages` devuelve la versión más reciente del mensaje, incluidas las modificaciones o eliminaciones que se han producido en este mediante `update_message` y `delete_message`. En el caso de los mensajes eliminados, `ChatMessage.deleted_on` devuelve un valor de fecha y hora que indica cuándo se eliminó el mensaje. En el caso de los mensajes editados, `ChatMessage.edited_on` devuelve un valor de fecha y hora que indica cuándo se editó el mensaje. Es posible acceder a la hora original de creación del mensaje mediante `ChatMessage.created_on`, que se puede usar para ordenar los mensajes.

`list_messages` devuelve distintos tipos de mensajes que se pueden identificar mediante `ChatMessage.type`. Estos tipos son:

- `ChatMessageType.TEXT`: mensaje de chat normal enviado por un participante de la conversación.

- `ChatMessageType.HTML`: mensaje de chat en formato HTML enviado por un participante de la conversación.

- `ChatMessageType.TOPIC_UPDATED`: mensaje del sistema que indica que el tema se ha actualizado.

- `ChatMessageType.PARTICIPANT_ADDED`: mensaje del sistema que indica que se han agregado uno o varios miembros a la conversación del chat.

- `ChatMessageType.PARTICIPANT_REMOVED`: mensaje del sistema que indica que se ha eliminado un miembro de la conversación del chat.

Para obtener más información, consulte [Tipos de mensajes](../../../concepts/chat/concepts.md#message-types).

## <a name="update-topic-of-a-chat-thread"></a>Actualización del tema de una conversación de chat
Puede actualizar el tema de una conversación de chat con el método `update_topic`.

```python
topic = "updated thread topic"
chat_thread_client.update_topic(topic=topic)
updated_topic = chat_client.get_chat_thread(chat_thread_client.thread_id).topic
print('Updated topic: ', updated_topic)
```

## <a name="update-a-message"></a>Actualización de un mensaje
Puede actualizar el contenido de un mensaje existente, identificado por el valor de message_id, con el método `update_message`.

- Utilice `message_id` para especificar el identificador del mensaje.
- Utilice `content` para establecer el nuevo contenido del mensaje.

```python
content = 'Hello world!'
send_message_result_id = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name)

content = 'Hello! I am updated content'
chat_thread_client.update_message(message_id=send_message_result_id, content=content)

chat_message = chat_thread_client.get_message(send_message_result_id)
print('Updated message content: ', chat_message.content.message)
```

## <a name="send-read-receipt-for-a-message"></a>Envío de confirmación de lectura de un mensaje
Puede usar el método `send_read_receipt` para publicar un evento de confirmación de lectura en una conversación en nombre de un usuario.

- Use `message_id` para especificar el identificador del último mensaje leído por el usuario actual.

```python
message_id=send_message_result_id
chat_thread_client.send_read_receipt(message_id=message_id)
```

## <a name="list-read-receipts-for-a-chat-thread"></a>Lista de confirmaciones de lectura de una conversación de chat
Se puede usar el método `list_read_receipts` para obtener las confirmaciones de lectura de una conversación.

- Utilice `results_per_page` para especificar el número máximo de confirmaciones de lectura de mensajes de chat que se van a devolver por página.
- Use `skip` para especificar que se omitan las confirmaciones de lectura de mensajes de chat hasta una posición especificada en la respuesta.

```python
read_receipts = chat_thread_client.list_read_receipts(results_per_page=2, skip=0)

for read_receipt_page in read_receipts.by_page():
    for read_receipt in read_receipt_page:
        print('ChatMessageReadReceipt: ', read_receipt)
```

## <a name="send-typing-notification"></a>Envío de notificación de escritura
Puede usar el método `send_typing_notification` para publicar un evento de escritura en una conversación en nombre de un usuario.

```python
chat_thread_client.send_typing_notification()
```

## <a name="delete-message"></a>Eliminación de mensajes
Se puede usar el método `delete_message` para eliminar un mensaje, identificado por un valor de message_id.

- Utilice `message_id` para especificar el identificador del mensaje.

```python
message_id=send_message_result_id
chat_thread_client.delete_message(message_id=message_id)
```

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Adición de un usuario como participante a la conversación del chat

Una vez que se crea un subproceso de chat, puede agregar y quitar usuarios de este. Al agregar usuarios, les concede acceso para enviar mensajes a la conversación del chat, y para agregar o quitar otros participantes. Antes de llamar al método `add_participant`, asegúrese de que ha adquirido un nuevo token de acceso y una identidad para ese usuario. El usuario necesitará ese token de acceso para poder inicializar su cliente de chat.

Utilice el método `add_participant` para agregar participantes a la conversación identificada por thread_id.

- Use `thread_participant` para especificar los participantes que se van a agregar a la conversación del chat.
- El elemento obligatorio `user` es el `CommunicationUserIdentifier` que creó mediante `CommunicationIdentityClient` en el paso [Creación de un usuario](../../access-tokens.md#create-an-identity).
- `display_name` es opcional y es el nombre para mostrar del participante de la conversación.
- `share_history_time` es opcional y es la hora a partir de la cual el historial de chat se compartió con el participante. Para compartir el historial desde el inicio del subproceso de chat, establezca esta propiedad en cualquier fecha igual o anterior a la hora de creación del subproceso. Para no compartir ningún historial anterior a la hora en que se agregó el participante, establézcala en la fecha actual. Para compartir el historial parcialmente, establézcala en una fecha intermedia.

```python
new_user = identity_client.create_user()

from azure.communication.chat import ChatThreadParticipant
from datetime import datetime

new_chat_thread_participant = ChatThreadParticipant(
    user=new_user,
    display_name='name',
    share_history_time=datetime.utcnow())

chat_thread_client.add_participant(new_chat_thread_participant)
```

También se pueden agregar varios usuarios a la conversación de chat con el método `add_participants`, siempre que todos los usuarios dispongan de un nuevo token de acceso e identificación.

```python
from azure.communication.chat import ChatThreadParticipant
from datetime import datetime

new_chat_thread_participant = ChatThreadParticipant(
        user=self.new_user,
        display_name='name',
        share_history_time=datetime.utcnow())
thread_participants = [new_chat_thread_participant] # instead of passing a single participant, you can pass a list of participants
chat_thread_client.add_participants(thread_participants)
```


## <a name="remove-user-from-a-chat-thread"></a>Eliminación de un usuario de un subproceso de chat

En una conversación, los participantes también se pueden eliminar de forma similar a como se agregan. Para quitarlos, será preciso realizar un seguimiento de los identificadores de los participantes que se han agregado.

Utilice el método `remove_participant` para quitar participantes de la conversación identificada por threadId.
- `user` corresponde al `CommunicationUserIdentifier` que se va a quitar de la conversación.

```python
chat_thread_client.remove_participant(new_user)
```

## <a name="run-the-code"></a>Ejecución del código

Ejecute la aplicación desde el directorio de la aplicación con el comando `python`.

```console
python start-chat.py
```
