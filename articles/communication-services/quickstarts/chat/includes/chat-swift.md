---
title: archivo de inclusión
description: archivo de inclusión
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 2/11/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 7833656b9b9be45aa3a0f0a8aa45cd70f925ce73
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379693"
---
## <a name="prerequisites"></a>Requisitos previos
Antes de comenzar, compruebe lo siguiente:

- Cree de una cuenta de Azure con una suscripción activa. Para más información, consulte [Creación de una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Instale [Xcode](https://developer.apple.com/xcode/) y [Cocoapods](https://cocoapods.org/). Se va a usar Xcode para crear una aplicación iOS para el inicio rápido y Cocoapods para instalar las dependencias.
- Cree un recurso de Azure Communication Services. Para obtener más información, consulte [Creación de un recurso de Azure Communication Services](../../create-communication-resource.md). Para este inicio rápido, tendrá que **registrar el punto de conexión del recurso**.
- Cree **dos** usuarios de ACS y genéreles un token de acceso de usuario [Token de acceso de usuario](../../access-tokens.md). Asegúrese de establecer el ámbito en **chat** y **anote la cadena del token, así como la cadena userId**. En este inicio rápido, se creará una conversación con un participante inicial y, después, se agregará un segundo participante a la conversación.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-ios-application"></a>Creación de una aplicación iOS

Abra Xcode y seleccione `Create a new Xcode project` (Crear un nuevo proyecto en Xcode).

En la ventana siguiente, seleccione `iOS` como plataforma y `App` para la plantilla.

Al elegir las opciones, escriba `ChatQuickstart` como nombre de proyecto. Seleccione `Storyboard` como interfaz, `UIKit App Delegate` como ciclo de vida y `Swift` como idioma.

Haga clic en Siguiente y elija el directorio en el que desea que se cree el proyecto.

### <a name="install-the-libraries"></a>Instalación de las bibliotecas

Se va a usar Cocoapods para instalar las dependencias de Communication Services necesarias.

Desde la línea de comandos, entre en el directorio raíz del proyecto de iOS `ChatQuickstart`.

Cree un podfile: `pod init`

Abra el podfile y agregue las siguientes dependencias al destino `ChatQuickstart`:
```
pod 'AzureCommunication', '~> 1.0.0-beta.8'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.8'
```

Instale las dependencias, lo que también creará un área de trabajo de Xcode: `pod install`

### <a name="setup-the-placeholders"></a>Configuración de los marcadores de posición

Abra el archivo de área de trabajo `ChatQuickstart.xcworkspace` en Xcode y, después, abra `ViewController.swift`.

En este inicio rápido, agregaremos el código a `viewController` y veremos la salida en la consola de Xcode. En este inicio rápido no se aborda la creación de una interfaz de usuario en iOS. 

Al principio de `viewController.swift` importe las bibliotecas `AzureCommunication` y `AzureCommunicatonChat`:

```
import AzureCommunication
import AzureCommunicationChat
```

Copie el código siguiente en el método `viewDidLoad()` de `ViewController`:

```
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global(qos: .background).async {
            do {
                // <CREATE A CHAT CLIENT>
                
                // <CREATE A CHAT THREAD>
                
                // <CREATE A CHAT THREAD CLIENT>
                
                // <SEND A MESSAGE>
                
                // <ADD A USER>
                
                // <LIST USERS>
                
                // <REMOVE A USER>
            } catch {
                print("Quickstart failed: \(error.localizedDescription)")
            }
        }
    }
```

Usaremos un semáforo para sincronizar el código para la demostración. En los pasos siguientes, se reemplazaran los marcadores de posición por código de ejemplo mediante la biblioteca de chat de Communication Services.


### <a name="create-a-chat-client"></a>Creación de un cliente de chat

Reemplace el comentario `<CREATE A CHAT CLIENT>` por el código siguiente:

```
let endpoint = "<ACS_RESOURCE_ENDPOINT>"
    let credential =
    try CommunicationTokenCredential(
        token: "<ACCESS_TOKEN>"
    )
    let options = AzureCommunicationChatClientOptions()

    let chatClient = try ChatClient(
        endpoint: endpoint,
        credential: credential,
        withOptions: options
    )
```

Reemplace `<ACS_RESOURCE_ENDPOINT>` por el punto de conexión del recurso de ACS.
Reemplace `<ACCESS_TOKEN>` por un token de acceso de ACS válido.

## <a name="object-model"></a>Modelo de objetos 
Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca cliente de chat de Azure Communication Services para JavaScript.

| Nombre                                   | Descripción                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Esta clase es necesaria para la funcionalidad de chat. Cree una instancia de esta con la información de suscripción y úsela para crear, obtener y eliminar subprocesos. |
| ChatThreadClient | Esta clase es necesaria para la funcionalidad de subproceso de chat. Obtiene una instancia de a través de la clase ChatClient y la usa para enviar, recibir, actualizar o eliminar mensajes, agregar, quitar u obtener usuarios, enviar notificaciones de escritura y confirmaciones de lectura, y suscribirse a eventos de chat. |

## <a name="start-a-chat-thread"></a>Inicio de un subproceso de chat

Ahora se usará `ChatClient` para crear una conversación con un usuario inicial.

Reemplace el comentario `<CREATE A CHAT THREAD>` por el código siguiente:

```
let request = CreateThreadRequest(
    topic: "Quickstart",
    participants: [
        Participant(
            id: "<USER_ID>",
            displayName: "Jack"
        )
    ]
)

var threadId: String?
chatClient.create(thread: request) { result, _ in
    switch result {
    case let .success(result):
        threadId = result.thread?.id

    case .failure:
        fatalError("Failed to create thread.")
    }
    semaphore.signal()
}
semaphore.wait()
```

Reemplace `<<USER_ID>>` por un identificador de usuario de Communication Services válido.

Aquí se va a usar un semáforo aquí esperar al controlador de finalización antes de continuar. Se usará `threadId` desde la respuesta que se devuelve al controlador de finalización en los pasos posteriores.

## <a name="get-a-chat-thread-client"></a>Obtención de un cliente de subproceso de chat

Ahora que hemos creado un hilo de chat, obtendremos un elemento `ChatThreadClient` para realizar operaciones en la conversación.

Reemplace el comentario `<CREATE A CHAT THREAD CLIENT>` por el código siguiente:

```
let chatThreadClient = try chatClient.createClient(forThread: threadId!)
```

## <a name="send-a-message-to-a-chat-thread"></a>Envío de un mensaje a un subproceso de chat

Reemplace el comentario `<SEND A MESSAGE>` por el código siguiente:

```
let message = SendChatMessageRequest(
    content: "Hello!",
    senderDisplayName: "Jack"
)

chatThreadClient.send(message: message) { result, _ in
    switch result {
    case let .success(result):
        print("Message sent, message id: \(result.id)")
    case .failure:
        print("Failed to send message")
    }
    semaphore.signal()
}
semaphore.wait()
```

En primer lugar, se crea `SendChatMessageRequest`, que contiene el nombre para mostrar de los remitentes y el contenido (también puede contener la hora del historial de recursos compartidos). La respuesta devuelta al controlador de finalización contiene el identificador del mensaje enviado.

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Adición de un usuario como miembro a la conversación del chat

Reemplace el comentario `<ADD A USER>` por el código siguiente:

```
let user = Participant(
    id: "<USER_ID>",
    displayName: "Jane"
)

chatThreadClient.add(participants: [user]) { result, _ in
    switch result {
    case let .success(result):
        (result.errors != nil) ? print("Added participant") : print("Error adding participant")
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```

Reemplace `<USER_ID>` por el identificador de ACS del usuario que se va a agregar.

Al agregar un participante a una conversación, la respuesta que se devolvió al finalizar puede contener errores. Estos errores indican que no se pueden agregar participantes concretos.

## <a name="list-users-in-a-thread"></a>Enumeración de usuarios en una conversación

Reemplace el comentario `<LIST USERS>` por el código siguiente:

```
chatThreadClient.listParticipants { result, _ in
    switch result {
    case let .success(participants):
        var iterator = participants.syncIterator
        while let participant = iterator.next() {
            print(participant.user.identifier)
        }
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```


## <a name="remove-user-from-a-chat-thread"></a>Eliminación de un usuario de un subproceso de chat

Reemplace el comentario `<REMOVE A USER>` por el código siguiente:

```
chatThreadClient
    .remove(
        participant: "<USER_ID>"
    ) { result, _ in
        switch result {
        case .success:
            print("Removed user from the thread.")
        case .failure:
            print("Failed to remove user from the thread.")
        }
    }
```

Reemplace `<USER ID>` por el identificador de usuario de Communication Services del participante que se va a quitar.

## <a name="run-the-code"></a>Ejecución del código

En Xcode, presione el botón Run (Ejecutar) para compilar y ejecutar el proyecto. En la consola, puede ver la salida del código y la salida del registrador de ChatClient.


