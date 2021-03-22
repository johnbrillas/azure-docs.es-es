---
title: 'Inicio rápido: Adición de llamadas de vídeo a la aplicación (JavaScript)'
titleSuffix: An Azure Communication Services quickstart
description: En esta guía de inicio rápido, aprenderá a agregar funcionalidades de llamada de vídeo a la aplicación con Azure Communication Services.
author: xumo-95
ms.author: mikben
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 82f4d9028fa94d4df0ff089fda213d64e13d56ec
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103487877"
---
# <a name="quickstart-add-11-video-calling-to-your-app-javascript"></a>Inicio rápido: Adición de llamadas de vídeo 1:1 a la aplicación (JavaScript)

## <a name="download-code"></a>Descarga de código

Busque el código finalizado de este inicio rápido en [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/add-1-on-1-video-calling)

## <a name="prerequisites"></a>Requisitos previos
- Obtenga una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Versiones Active LTS y Maintenance LTS de [Node.js](https://nodejs.org/en/) (8.11.1 y 10.14.1)
- Cree un recurso activo de Communication Services. [Cree un recurso de Communication Services](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp).
- Cree un token de acceso de usuario para crear una instancia del cliente de llamada. [Creación y administración de tokens de acceso](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-csharp).

## <a name="setting-up"></a>Instalación
### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js
Abra la ventana de comandos o de terminal, cree un nuevo directorio para la aplicación y navegue hasta este.
```console
mkdir calling-quickstart && cd calling-quickstart
```
### <a name="install-the-package"></a>Instalar el paquete
Use el comando `npm install` para instalar la biblioteca cliente de llamadas de Azure Communication Services para JavaScript.

En esta guía de inicio rápido se usa la versión `1.0.0.beta-6` de la biblioteca cliente de llamadas de Azure Communication Services. 

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```
### <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Esta guía de inicio rápido usa webpack para agrupar los recursos de la aplicación. Ejecute el siguiente comando para instalar los paquetes npm `webpack`, `webpack-cli` y `webpack-dev-server`, y los enumera como dependencias de desarrollo en el archivo `package.json`:

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```
Cree un archivo `index.html` en el directorio raíz del proyecto. Este archivo lo usaremos para configurar un diseño básico que permitirá al usuario realizar una llamada de video 1:1.

Este es el código:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - 1:1 Video Calling Sample</title>
</head>

<body>
    <h4>Azure Communication Services</h4>
    <h1>1:1 Video Calling Quickstart</h1>
    <input 
    id="callee-id-input"
    type="text"
    placeholder="Who would you like to call?"
    style="margin-bottom:1em; width: 200px;"
    />

    <div>
    <button id="call-button" type="button" disabled="true">
        start call
    </button>
        &nbsp;
    <button id="hang-up-button" type="button" disabled="true">
        hang up
    </button>
        &nbsp;
    <button id="start-Video" type="button" disabled="true">
        start video
    </button>
        &nbsp;
    <button id="stop-Video" type="button" disabled="true">
        stop video
    </button>     
    </div>

    <div>Local Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;">
      <div id="myVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>     
    </div>
    <div>Remote Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;"> 
      <div id="remoteVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>
    </div>

    <script src="./bundle.js"></script>
</body>
</html>
```

Cree un archivo en el directorio raíz del proyecto denominado `client.js` que contendrá la lógica de la aplicación para esta guía de inicio rápido. Agregue el siguiente código para importar el cliente que realiza la llamada y obtener referencias a los elementos del DOM.

```JavaScript
import { CallClient, CallAgent, Renderer, LocalVideoStream } from "@azure/communication-calling";
import { AzureCommunicationTokenCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
const stopVideoButton = document.getElementById("stop-Video");
const startVideoButton = document.getElementById("start-Video");

let placeCallOptions;
let deviceManager;
let localVideoStream;
let rendererLocal;
let rendererRemote;
```
## <a name="object-model"></a>Modelo de objetos

Las clases e interfaces siguientes controlan algunas de las características principales de la biblioteca cliente para llamadas de Azure Communication Services:

| Nombre      | Descripción | 
| :---        |    :----   |
| CallClient  | CallClient es el punto de entrada principal a la biblioteca cliente para llamadas.      |
| CallAgent  | CallAgent se usa para iniciar y administrar llamadas.        |
| DeviceManager | DeviceManager se usa para administrar dispositivos multimedia.    |
| AzureCommunicationTokenCredential | La clase AzureCommunicationTokenCredential implementa la interfaz CommunicationTokenCredential, que se usa para crear una instancia de CallAgent.        |

## <a name="authenticate-the-client-and-access-devicemanager"></a>Autenticación del cliente y acceso a DeviceManager

Debe reemplazar <USER_ACCESS_TOKEN> por un token de acceso de usuario válido para el recurso. Consulte la documentación relativa al token de acceso de usuario si aún no tiene ningún token disponible. Con CallClient, inicialice una instancia de CallAgent con un elemento CommunicationUserCredential que nos permitirá realizar y recibir llamadas. Para acceder a DeviceManager, primero se debe crear una instancia de callAgent. Luego, puede usar el método `getDeviceManager` en la instancia de `CallClient` para obtener el elemento `DeviceManager`.

Agregue el siguiente código a `client.js`:

```JavaScript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationTokenCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, { displayName: 'optional ACS user name' });
    
    deviceManager = await callClient.getDeviceManager();
    callButton.disabled = false;
}
init();
```
## <a name="place-a-11-outgoing-video-call-to-a-user"></a>Realización de una llamada saliente de vídeo 1:1 a un usuario

Agregue una escucha de eventos para iniciar una llamada cuando se haga clic en `callButton`:

En primer lugar, debe enumerar las cámaras locales mediante la API getCameraList de deviceManager. En esta guía de inicio rápido usamos la primera cámara de la colección. Una vez seleccionada la cámara deseada, se creará una instancia de LocalVideoStream y se pasará al método de llamada dentro de videoOptions como un elemento de la matriz localVideoStream. Una vez que la llamada se conecta, iniciará automáticamente el envío de una secuencia de vídeo al otro participante. 

```JavaScript
callButton.addEventListener("click", async () => {
    const videoDevices = await deviceManager.getCameras();
    const videoDeviceInfo = videoDevices[0];
    localVideoStream = new LocalVideoStream(videoDeviceInfo);
    placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};

    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;

    const userToCall = calleeInput.value;
    call = callAgent.startCall(
        [{ communicationUserId: userToCall }],
        placeCallOptions
    );

    subscribeToRemoteParticipantInCall(call);

    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```  
Para representar un elemento `LocalVideoStream`, debe crear una nueva instancia de `Renderer` y, a continuación, crear una nueva instancia de RendererView con el método asincrónico `createView`. Después puede adjuntar `view.target` a cualquier elemento de la interfaz de usuario. 

```JavaScript
async function localVideoView() {
    rendererLocal = new Renderer(localVideoStream);
    const view = await rendererLocal.createView();
    document.getElementById("myVideo").appendChild(view.target);
}
```
Todos los participantes remotos están disponibles mediante la colección `remoteParticipants` de una instancia de la llamada. Debe suscribirse a los participantes remotos de la llamada actual y escuchar el evento `remoteParticipantsUpdated` para suscribirse a los participantes remotos agregados.

```JavaScript
function subscribeToRemoteParticipantInCall(callInstance) {
    callInstance.remoteParticipants.forEach( p => {
        subscribeToRemoteParticipant(p);
    })
    callInstance.on('remoteParticipantsUpdated', e => {
        e.added.forEach( p => {
            subscribeToRemoteParticipant(p);
        })
    });   
}
```
Puede suscribirse a la colección `remoteParticipants` de la llamada actual e inspeccionar las colecciones `videoStreams` para enumerar las secuencias de cada participante. También debe suscribirse al evento remoteParticipantsUpdated para administrar los participantes remotos agregados. 

```JavaScript
function subscribeToRemoteParticipant(remoteParticipant) {
    remoteParticipant.videoStreams.forEach(v => {
        handleVideoStream(v);
    });
    remoteParticipant.on('videoStreamsUpdated', e => {
        e.added.forEach(v => {
            handleVideoStream(v);
        })
    });
}
```
Para representar el elemento `remoteVideoStream`, se debe suscribir a un evento `isAvailableChanged`. Si la propiedad `isAvailable` cambia a `true`, un participante remoto envía una secuencia. Cada vez que cambie la disponibilidad de una secuencia remota, puede elegir destruir todo el elemento `Renderer`, un elemento `RendererView` específico o mantenerlos, pero esto hará que el fotograma del vídeo se vea en blanco.
```JavaScript
function handleVideoStream(remoteVideoStream) {
    remoteVideoStream.on('availabilityChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            remoteVideoView(remoteVideoStream);
        } else {
            rendererRemote.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        remoteVideoView(remoteVideoStream);
    }
}
```
Para representar un elemento `RemoteVideoStream`, debe crear una nueva instancia de `Renderer` y, a continuación, crear una nueva instancia de `RendererView` con el método asincrónico `createView`. Después puede adjuntar `view.target` a cualquier elemento de la interfaz de usuario. 

```JavaScript
async function remoteVideoView(remoteVideoStream) {
    rendererRemote = new Renderer(remoteVideoStream);
    const view = await rendererRemote.createView();
    document.getElementById("remoteVideo").appendChild(view.target);
}
```
## <a name="receive-an-incoming-call"></a>Recepción de una llamada entrante
Para controlar las llamadas entrantes, debe escuchar el evento `incomingCall` de `callAgent`. Una vez que haya una llamada entrante, debe enumerar las cámaras locales y construir una instancia de `LocalVideoStream` para enviar una transmisión de vídeo al otro participante. También debe suscribirse a `remoteParticipants` para controlar las secuencias de vídeo remotas. Puede aceptar o rechazar la llamada mediante la instancia de `incomingCall`. 

Coloque la implementación en `init()` para controlar las llamadas entrantes. 

```JavaScript
callAgent.on('incomingCall', async e => {
    const videoDevices = await deviceManager.getCameras();
    const videoDeviceInfo = videoDevices[0];
    localVideoStream = new LocalVideoStream(videoDeviceInfo);
    localVideoView();

    stopVideoButton.disabled = false;
    callButton.disabled = true;
    hangUpButton.disabled = false;

    const addedCall = await e.incomingCall.accept({videoOptions: {localVideoStreams:[localVideoStream]}});
    call = addedCall;

    subscribeToRemoteParticipantInCall(addedCall);   
});
```
## <a name="end-the-current-call"></a>Finalización de la llamada actual
Agregue un cliente de escucha de eventos para finalizar la llamada actual cuando se haga clic en el `hangUpButton`:
```JavaScript
hangUpButton.addEventListener("click", async () => {
    // dispose of the renderers
    rendererLocal.dispose();
    rendererRemote.dispose();
    // end the current call
    await call.hangUp();
    // toggle button states
    hangUpButton.disabled = true;
    callButton.disabled = false;
    stopVideoButton.disabled = true;
});
```
## <a name="subscribe-to-call-updates"></a>Suscripción a actualizaciones de llamadas
Deberá suscribirse al evento cuando el participante remoto finalice la llamada para desechar los representadores de vídeo y alternar los estados de los botones. 

Coloque la implementación en init() para suscribirse al evento `callsUpdated`. 
 ```JavaScript 
callAgent.on('callsUpdated', e => {
    e.removed.forEach(removedCall => {
        // dispose of video renders
        rendererLocal.dispose();
        rendererRemote.dispose();
        // toggle button states
        hangUpButton.disabled = true;
        callButton.disabled = false;
        stopVideoButton.disabled = true;
    })
})
```

## <a name="start-and-end-video-during-the-call"></a>Inicio y finalización del vídeo durante la llamada
Puede detener el vídeo durante la llamada actual mediante la incorporación de una escucha de eventos al botón para detener el vídeo para desechar el representador de `localVideoStream`. 
 ```JavaScript       
stopVideoButton.addEventListener("click", async () => {
    await call.stopVideo(localVideoStream);
    rendererLocal.dispose();
    startVideoButton.disabled = false;
    stopVideoButton.disabled = true;
});
```
Puede agregar una escucha de eventos al botón para iniciar el vídeo para volver a activar el vídeo cuando se detuvo durante la llamada actual. 
```JavaScript
startVideoButton.addEventListener("click", async () => {
    await call.startVideo(localVideoStream);
    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;
});
```
## <a name="run-the-code"></a>Ejecución del código
Utilice `webpack-dev-server` para compilar y ejecutar la aplicación. Ejecute el siguiente comando para agrupar el host de aplicación en un servidor web local:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Abra el explorador web y vaya a http://localhost:8080/. Verá lo siguiente:

:::image type="content" source="./media/javascript/1-on-1-video-calling.png" alt-text="Página para llamada de vídeo 1:1":::

Para hacer una llamada de vídeo saliente 1:1, proporcione un identificador de usuario en el campo de texto y haga clic en el botón para iniciar llamada. 

## <a name="sample-code"></a>Código de ejemplo
Puede descargar la aplicación de ejemplo de [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/add-1-on-1-video-calling).

## <a name="clean-up-resources"></a>Limpieza de recursos
Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte los siguientes artículos.
- Consulte [Introducción al ejemplo de llamada web](https://docs.microsoft.com/azure/communication-services/samples/web-calling-sample).
- Información sobre las [funcionalidades de la biblioteca cliente de llamadas](https://docs.microsoft.com/azure/communication-services/quickstarts/voice-video-calling/calling-client-samples?pivots=platform-web)
- Más información sobre [cómo funciona la llamada](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/about-call-types)
