---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: 4a9454abc2c4e41d711a4aef6a30438a72d27edb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751049"
---
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Un recurso implementado de Communication Services. [Cree un recurso de Communication Services](../../create-communication-resource.md).
- `User Access Token` para habilitar el cliente de llamada. Para más información sobre [cómo obtener un `User Access Token`](../../access-tokens.md)
- Opcional: Complete el inicio rápido para [empezar a agregar llamadas a su aplicación](../getting-started-with-calling.md)

## <a name="setting-up"></a>Instalación

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

> [!NOTE]
> En este documento se usa la versión 1.0.0-beta.6 de la biblioteca de cliente para llamadas.

Use el comando `npm install` para instalar las bibliotecas cliente común y para llamadas de Azure Communication Services para JavaScript.
En este documento se hace referencia a los tipos de la versión 1.0.0-beta.5 de la biblioteca de llamadas.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save

```

## <a name="object-model"></a>Modelo de objetos

Las clases e interfaces siguientes controlan algunas de las características principales de la biblioteca cliente para llamadas de Azure Communication Services:

| Nombre                             | Descripción                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | CallClient es el punto de entrada principal a la biblioteca cliente para llamadas.                                                                       |
| CallAgent                        | CallAgent se usa para iniciar y administrar llamadas.                                                                                            |
| DeviceManager                    | DeviceManager se usa para administrar dispositivos multimedia.                                                                                           |
| AzureCommunicationTokenCredential | La clase AzureCommunicationTokenCredential implementa la interfaz CommunicationTokenCredential, que se usa para crear una instancia de CallAgent. |


## <a name="initialize-the-callclient-create-callagent-and-access-devicemanager"></a>Inicialización de CallClient, creación de CallAgent y acceso a DeviceManager

Cree una instancia nueva de `CallClient`. Se puede configurar con opciones personalizadas como una instancia del registrador.
Una vez que se crean instancias de `CallClient`, puede crear una instancia de `CallAgent` mediante una llamada al método `createCallAgent` en la instancia de `CallClient`. De este modo, se devuelve un objeto de instancia de `CallAgent` de manera asincrónica.
El método `createCallAgent` toma `CommunicationTokenCredential` como argumento, que acepta un [token de acceso de usuario](../../access-tokens.md).
Para acceder a `DeviceManager`, primero se debe crear una instancia de callAgent. Luego, puede usar el método `getDeviceManager` en la instancia de `CallClient` para obtener DeviceManager.

```js
const userToken = '<user token>';
callClient = new CallClient(options);
const tokenCredential = new AzureCommunicationTokenCredential(userToken);
const callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'optional ACS user name'});
const deviceManager = await callClient.getDeviceManager()
```

## <a name="place-an-outgoing-call"></a>Realización de una llamada saliente

Para crear e iniciar una llamada, debe usar una de las API en CallAgent y proporcionar un usuario que haya creado a través de la biblioteca cliente de administración de Communication Services.

La creación y el inicio de la llamada es sincrónico. La instancia de llamada le permite suscribirse a eventos de llamada.

## <a name="place-a-call"></a>Realización de una llamada

### <a name="place-a-11-call-to-a-user-or-pstn"></a>Realización de una llamada 1:1 a un usuario o RTC
Para realizar una llamada a otro usuario de Communication Services, invoque el método `startCall` en `callAgent` y pase CommunicationUserIdentifier del destinatario que ha [creado con la biblioteca de administración de Communication Services](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens).

```js
const userCallee = { communicationUserId: '<ACS_USER_ID>' }
const oneToOneCall = callAgent.startCall([userCallee]);
```

Para realizar una llamada a una RTC, invoque el método `startCall` en `callAgent` y pase PhoneNumberIdentifier del destinatario.
El recurso de Communication Services debe estar configurado para permitir llamadas de RTC.
Cuando llame a números de RTC, debe especificar su identificador alternativo de autor de llamada. Un identificador del autor de llamada alternativo hace referencia a un número de teléfono (basado en el estándar E.164), que identifica al autor de llamada en una llamada RTC. Por ejemplo, cuando se proporciona un identificador de autor de llamada alternativo a la llamada RTC, ese número de teléfono será el que se muestre al destinatario cuando la llamada sea entrante.

> [!WARNING]
> Llamadas RTC están actualmente en versión preliminar privada. Para acceder, [suscríbase al programa de usuarios pioneros](https://aka.ms/ACS-EarlyAdopter).
```js
const pstnCalee = { phoneNumber: '<ACS_USER_ID>' }
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const oneToOneCall = callAgent.startCall([pstnCallee], {alternateCallerId});
```

### <a name="place-a-1n-call-with-users-and-pstn"></a>Realice una llamada de uno a varios con usuarios y RTC
```js
const userCallee = { communicationUserId: <ACS_USER_ID> }
const pstnCallee = { phoneNumber: <PHONE_NUMBER>};
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const groupCall = callAgent.startCall([userCallee, pstnCallee], {alternateCallerId});

```

### <a name="place-a-11-call-with-video-camera"></a>Realización de una llamada 1:1 con videocámara
> [!WARNING]
> Actualmente, no puede haber más de una secuencia de vídeo local saliente.
Para realizar una videollamada, debe enumerar las cámaras locales mediante la API `getCameras()` de deviceManager.
Una vez que seleccione la cámara deseada, úsela para crear una instancia de `LocalVideoStream` y pásela dentro de `videoOptions` como elemento dentro de la matriz `localVideoStream` al método `startCall`.
Una vez que la llamada se conecta, iniciará automáticamente el envío de una secuencia de vídeo desde la cámara seleccionada hasta los demás participantes. Esto también se aplica a las opciones de vídeo Call.Accept() y las opciones de vídeo CallAgent.join().
```js
const deviceManager = await callClient.getDeviceManager();
const cameras = await deviceManager.getCameras();
videoDeviceInfo = cameras[0];
localVideoStream = new LocalVideoStream(videoDeviceInfo);
const placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};
const call = callAgent.startCall(['acsUserId'], placeCallOptions);

```

### <a name="join-a-group-call"></a>Unión a una llamada grupal
Para iniciar una llamada grupal nueva o unirse a una en curso, use el método "join" y pase un objeto con una propiedad `groupId`. Este valor debe ser un GUID.
```js

const context = { groupId: <GUID>}
const call = callAgent.join(context);

```
### <a name="join-a-teams-meeting"></a>Unirse a una reunión de Teams
Para unirse a una reunión de Teams, use el método "join" y pase un vínculo de reunión o las coordenadas de una reunión.
```js
// Join using meeting link
const locator = { meetingLink: <meeting link>}
const call = callAgent.join(locator);

// Join using meeting coordinates
const locator = {
    threadId: <thread id>,
    organizerId: <organizer id>,
    tenantId: <tenant id>,
    messageId: <message id>
}
const call = callAgent.join(locator);
```

## <a name="receiving-an-incoming-call"></a>Recibir una llamada entrante

La instancia `CallAgent` emite un evento `incomingCall` cuando la identidad que ha iniciado sesión recibe una llamada entrante. Para escuchar este evento, suscríbase de la siguiente manera:

```js
const incomingCallHander = async (args: { incomingCall: IncomingCall }) => {
    //Get information about caller
    var callerInfo = incomingCall.callerInfo
    
    //accept the call
    var call = await incomingCall.accept();

    //reject the call
    incomingCall.reject();
};
callAgentInstance.on('incomingCall', incomingCallHander);
```

El evento `incomingCall` proporcionará una instancia de `IncomingCall` en la que puede aceptar o rechazar una llamada.


## <a name="call-management"></a>Administración de llamadas

Durante una llamada, puede acceder a las propiedades de la llamada y realizar varias operaciones para administrar la configuración relacionada con el vídeo y el audio.

### <a name="call-properties"></a>Propiedades de llamada
* Obtenga el identificador único (cadena) de esta llamada.
```js

const callId: string = call.id;

```

* Para información sobre los demás participantes de la llamada, revise la colección `remoteParticipant` de la instancia `call`. La matriz contiene objetos `RemoteParticipant` de lista.
```js
const remoteParticipants = call.remoteParticipants;
```

* Identificador del autor de llamada si la llamada es entrante. El identificador es uno de los tipos de `CommunicationIdentifier`.
```js

const callerIdentity = call.callerInfo.identifier;

* Get the state of the Call.
```js

const callState = call.state;

```
Esto devuelve una cadena que representa el estado actual de una llamada:
* "None": estado inicial de la llamada
* "Incoming': indica que hay una llamada entrante que se debe aceptar o rechazar
* "Connecting": estado de transición inicial una vez que se realiza o acepta la llamada
* "Ringing": en el caso de una llamada saliente, indica que se está llamando a los participantes remotos. Aparece con el estado "Incoming" para ellos
* "EarlyMedia": indica un estado en el que se reproduce un anuncio antes de conectar la llamada
* "Connected": se conecta la llamada
* "LocalHold": la llamada se pone en espera, no fluye ningún elemento multimedia entre el punto de conexión local y los participantes remotos
* "RemoteHold": la llamada la pone en espera un participante remoto, no fluye ningún elemento multimedia entre el punto de conexión local y los participantes remotos
* "Disconnecting": estado de transición antes de que la llamada pase al estado "Disconnected"
* "Disconnected": estado final de la llamada
  * Si se pierde la conexión de red, el estado pasa a "Disconnected" después de unos 2 minutos.

* Para descubrir por qué se finalizó una llamada determinada, revise la propiedad `callEndReason`.
```js

const callEndReason = call.callEndReason;
// callEndReason.code (number) code associated with the reason
// callEndReason.subCode (number) subCode associated with the reason
```

* Para saber si la llamada actual es una llamada entrante o saliente, inspeccione la propiedad `direction`, devuelve `CallDirection`.
```js
const isIncoming = call.direction == 'Incoming';
const isOutgoing = call.direction == 'Outgoing';
```

*  Para comprobar si el micrófono actual está silenciado, revise la propiedad `muted`, devuelve `Boolean`.
```js

const muted = call.isMicrophoneMuted;

```

* Para ver si la secuencia de uso compartido de pantalla se envía desde un punto de conexión determinado, revise la propiedad `isScreenSharingOn`, devuelve `Boolean`.
```js

const isScreenSharingOn = call.isScreenSharingOn;

```

* Para revisar las secuencias de vídeo activas, compruebe la colección `localVideoStreams`, contiene objetos `LocalVideoStream`.
```js

const localVideoStreams = call.localVideoStreams;

```

### <a name="call-ended-event"></a>Evento de llamada finalizada

La instancia `Call` emite un evento `callEnded` cuando finaliza la llamada. Para escuchar este evento, suscríbase de la siguiente manera:

```js
const callEndHander = async (args: { callEndReason: CallEndReason }) => {
    console.log(args.callEndReason)
};

call.on('callEnded', callEndHander);
```

### <a name="mute-and-unmute"></a>Silencio y reactivación del sonido

Para silenciar o reactivar el sonido del punto de conexión local, puede usar las API asincrónicas `mute` y `unmute`:

```js

//mute local device 
await call.mute();

//unmute local device 
await call.unmute();

```

### <a name="start-and-stop-sending-local-video"></a>Inicio y detención del envío de vídeo local


Para iniciar un vídeo, tiene que enumerar las cámaras con el método `getCameras` en el objeto `deviceManager`. Luego, cree una instancia de `LocalVideoStream` pasando la cámara deseada al método `startVideo` como argumento:


```js
const localVideoStream = new LocalVideoStream(videoDeviceInfo);
await call.startVideo(localVideoStream);

```

Una vez que logre empezar a enviar vídeo, se agregará una instancia `LocalVideoStream` a la colección `localVideoStreams` en una instancia de llamada.

```js

call.localVideoStreams[0] === localVideoStream;

```

Para detener el vídeo local, pase la instancia `localVideoStream` disponible en la colección `localVideoStreams`:

```js

await call.stopVideo(localVideoStream);

```

Puede cambiar a un dispositivo de cámara distinto mientras se envía vídeo si invoca `switchSource` en una instancia `localVideoStream`:

```js
const cameras = await callClient.getDeviceManager().getCameras();
localVideoStream.switchSource(cameras[1]);

```

## <a name="remote-participants-management"></a>Administración de participantes remotos

El tipo `RemoteParticipant` representa a todos los participantes remotos, los que están disponibles a través de la colección `remoteParticipants` en una instancia de llamada.

### <a name="list-participants-in-a-call"></a>Lista de los participantes en una llamada
La colección `remoteParticipants` devuelve una lista de los participantes remotos en una llamada determinada:

```js

call.remoteParticipants; // [remoteParticipant, remoteParticipant....]

```

### <a name="remote-participant-properties"></a>Propiedades de los participantes remotos
El participante remoto tiene un conjunto de propiedades y colecciones asociadas.
#### <a name="communicationidentifier"></a>CommunicationIdentifier
Obtenga el identificador de este participante remoto.
"Identity" es uno de los tipos de "CommunicationIdentifier":
```js
const identifier = remoteParticipant.identifier;
```
Puede ser de uno de los tipos "CommunicationIdentifier":
  * { communicationUserId: '<ID_USUARIO_ACS>' }: objeto que representa al usuario de ACS
  * { phoneNumber: '<E.164>' }: objeto que representa el número de teléfono en formato E.164
  * { microsoftTeamsUserId: '<ID_USUARIO_TEAMS>', isAnonymous?: boolean; cloud?: "public" | "dod" | "gcch" }: objeto que representa al usuario de Teams

#### <a name="state"></a>State
Obtenga el estado de este participante remoto.
```js

const state = remoteParticipant.state;
```
El estado puede ser uno de los siguientes:
* "Idle": estado inicial
* "Connecting": estado de transición mientras el participante se conecta a la llamada
* "Ringing": el participante está sonando
* "Connected": el participante se conecta a la llamada
* "Hold": el participante está en espera
* "EarlyMedia": se reproduce un anuncio antes de que el participante se conecte a la llamada
* "Disconnected": estado final; el participante se desconecta de la llamada
  * Si el participante remoto pierde la conectividad de red, el estado del participante remoto pasa a "Disconnected" después de unos 2 minutos.

#### <a name="call-end-reason"></a>Motivo del final de la llamada
Para saber por qué el participante dejó la llamada, revise la propiedad `callEndReason`:
```js
const callEndReason = remoteParticipant.callEndReason;
// callEndReason.code (number) code associated with the reason
// callEndReason.subCode (number) subCode associated with the reason
```
#### <a name="is-muted"></a>Está silenciado
Para comprobar si este participante remoto está silenciado o no, revise la propiedad `isMuted`, devuelve `Boolean`.
```js
const isMuted = remoteParticipant.isMuted;
```
#### <a name="is-speaking"></a>Está hablando
Para comprobar si este participante remoto está hablando o no, revise la propiedad `isSpeaking`, devuelve `Boolean`.
```js
const isSpeaking = remoteParticipant.isSpeaking;
```

#### <a name="video-streams"></a>Secuencias de vídeo
Para revisar todas las secuencias de vídeo que un participante determinado envía en esta llamada, compruebe la colección `videoStreams`, contiene objetos `RemoteVideoStream`.
```js

const videoStreams = remoteParticipant.videoStreams; // [RemoteVideoStream, ...]

```


### <a name="add-a-participant-to-a-call"></a>Incorporación de un participante a una llamada

Para agregar un participante a una llamada (ya sea un usuario o un número de teléfono), puede invocar `addParticipant`.
Proporcione uno de los tipos de "Identifier".
Esto devolverá de manera sincrónica la instancia del participante remoto.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
const remoteParticipant = call.addParticipant(userIdentifier);
const remoteParticipant = call.addParticipant(pstnIdentifier, {alternateCallerId: '<Alternate Caller ID>'});
```

### <a name="remove-participant-from-a-call"></a>Eliminación del participante de una llamada

Para quitar un participante de una llamada (ya sea un usuario o un número de teléfono), puede invocar `removeParticipant`.
Debe pasar uno de los tipos de "Identifier". Esto se resolverá de manera asincrónica una vez que el participante se quite de la llamada.
El participante también se quitará de la colección `remoteParticipants`.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
await call.removeParticipant(userIdentifier);
await call.removeParticipant(pstnIdentifier);
```

## <a name="render-remote-participant-video-streams"></a>Representación de secuencias de vídeo de participantes remotos

Para una lista de las secuencias de vídeo y de las secuencias de uso compartido de pantalla de participantes remotos, revise las colecciones `videoStreams`:

```js
const remoteVideoStream: RemoteVideoStream = call.remoteParticipants[0].videoStreams[0];
const streamType: MediaStreamType = remoteVideoStream.mediaStreamType;
```
 
Para representar una `RemoteVideoStream`, se debe suscribir a un evento de `isAvailableChanged`.
Si la propiedad `isAvailable` cambia a `true`, un participante remoto envía una secuencia.
Cuando esto suceda, cree una instancia de `Renderer` y, luego, cree una instancia de `RendererView` nueva a través del método `createView` asincrónico.  Después puede adjuntar `view.target` a cualquier elemento de la interfaz de usuario.
Cada vez que cambia la disponibilidad de una secuencia remota, puede elegir destruir todo el representador, un `RendererView` específico o mantenerlos, pero esto hará que el fotograma del vídeo se vea en blanco.

```js
function subscribeToRemoteVideoStream(remoteVideoStream: RemoteVideoStream) {
    let renderer: Renderer = new Renderer(remoteVideoStream);
    const displayVideo = () => {
        const view = await renderer.createView();
        htmlElement.appendChild(view.target);
    }
    remoteVideoStream.on('availabilityChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            displayVideo();
        } else {
            renderer.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        displayVideo();
    }
}
```

### <a name="remote-video-stream-properties"></a>Propiedades de secuencias de vídeo remotas
Las secuencias de vídeo remotas tienen las propiedades siguientes:

* `Id`: id. de una secuencia de vídeo remota
```js
const id: number = remoteVideoStream.id;
```

* `StreamSize`: tamaño (ancho y alto) de una secuencia de vídeo remota
```js
const size: {width: number; height: number} = remoteVideoStream.size;
```

* `MediaStreamType`: puede ser "Video" o "ScreenSharing"
```js
const type: MediaStreamType = remoteVideoStream.mediaStreamType;
```
* `isAvailable`: indica si el punto de conexión del participante remoto envía secuencias de manera activa
```js
const type: boolean = remoteVideoStream.isAvailable;
```

### <a name="renderer-methods-and-properties"></a>Métodos y propiedades del representador

* Cree una instancia `RendererView` que se pueda adjuntar posteriormente en la interfaz de usuario de la aplicación para representar la secuencia de vídeo remota.
```js
renderer.createView()
```

* Elimine el representador y todas las instancias `RendererView` asociadas.
```js
renderer.dispose()
```


### <a name="rendererview-methods-and-properties"></a>Métodos y propiedades de RendererView
Al crear un objeto `RendererView`, puede especificar las propiedades `scalingMode` y `isMirrored`.
El modo de escalado puede ser "Stretch", "Crop" o "Fit". Si se especifica `isMirrored`, la secuencia representada se volteará de manera vertical.

```js
const rendererView: RendererView = renderer.createView({ scalingMode, isMirrored });
```
Toda instancia `RendererView` determinada tiene una propiedad `target` que representa la superficie de representación. Se debe adjuntar en la interfaz de usuario de la aplicación:
```js
document.body.appendChild(rendererView.target);
```

Más adelante, puede invocar el método `updateScalingMode` para actualizar el modo de escalado.
```js
view.updateScalingMode('Crop')
```

## <a name="device-management"></a>Administración de dispositivos

`DeviceManager` permite enumerar los dispositivos locales que se pueden usar en una llamada para transmitir las secuencias de audio o vídeo. También permite solicitar permiso a un usuario para acceder a su micrófono y cámara mediante la API nativa del explorador.

Puede llamar al método `callClient.getDeviceManager()` para acceder a `deviceManager`.
> [!WARNING]
> Actualmente, se debe crear una instancia de un objeto `callAgent` para poder obtener acceso a DeviceManager.

```js

const deviceManager = await callClient.getDeviceManager();

```

### <a name="enumerate-local-devices"></a>Enumeración de los dispositivos locales

Para acceder a los dispositivos locales, puede usar métodos de enumeración en el Administrador de dispositivos. La enumeración es una acción asincrónica.

```js

//  Get a list of available video devices for use.
const localCameras = await deviceManager.getCameras(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
const localMicrophones = await deviceManager.getMicrophones(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
const localSpeakers = await deviceManager.getSpeakers(); // [AudioDeviceInfo, AudioDeviceInfo...]

```

### <a name="set-default-microphonespeaker"></a>Establecimiento de micrófono/altavoz predeterminados

El administrador de dispositivos le permite establecer un dispositivo predeterminado que se usará al iniciar una llamada.
Si no se establecen los valores predeterminados del cliente, Communication Services revertirá a los valores predeterminados del sistema operativo.

```js

// Get the microphone device that is being used.
const defaultMicrophone = deviceManager.selectedMicrophone;

// Set the microphone device to use.
await deviceManager.selectMicrophone(AudioDeviceInfo);

// Get the speaker device that is being used.
const defaultSpeaker = deviceManager.selectedSpeaker;

// Set the speaker device to use.
await deviceManager.selectSpeaker(AudioDeviceInfo);

```

### <a name="local-camera-preview"></a>Vista previa de la cámara local

Puede usar `DeviceManager` y `Renderer` para empezar a representar secuencias desde la cámara local. Esta secuencia no se enviará a los demás participantes, porque es una fuente de vista previa local. Se trata de una acción asincrónica.

```js
const cameras = await deviceManager.getCameras();
const localVideoDevice = cameras[0];
const localCameraStream = new LocalVideoStream(localVideoDevice);
const renderer = new Renderer(localCameraStream);
const view = await renderer.createView();
document.body.appendChild(view.target);

```

### <a name="request-permission-to-cameramicrophone"></a>Solicitud de permiso para acceder a la cámara o micrófono

Pídale a un usuario que conceda permisos para la cámara o el micrófono de la manera siguiente:

```js
const result = await deviceManager.askDevicePermission({audio: true, video: true});
```
Esto se resolverá de manera asincrónica con un objeto que indica si se concedieron los permisos `audio` y `video`:
```js
console.log(result.audio);
console.log(result.video);
```


## <a name="call-recording-management"></a>Administración de la grabación de llamadas

La grabación de llamadas es una característica extendida de la API `Call` principal. Primero debe obtener el objeto de API de la característica de grabación:

```js
const callRecordingApi = call.api(Features.Recording);
```

Luego, para poder comprobar si se está grabando la llamada, inspeccione la propiedad `isRecordingActive` de `callRecordingApi`; devolverá `Boolean`.

```js
const isResordingActive = callRecordingApi.isRecordingActive;
```

También puede suscribirse a los cambios de grabación:

```js
const isRecordingActiveChangedHandler = () => {
  console.log(callRecordingApi.isRecordingActive);
};

callRecordingApi.on('isRecordingActiveChanged', isRecordingActiveChangedHandler);
               
```

## <a name="call-transfer-management"></a>Administración de la transferencia de llamadas

La trasferencia de llamadas es una característica extendida de la API `Call` principal. Primero debe obtener el objeto de API de la característica de transferencia:

```js
const callTransferApi = call.api(Features.Transfer);
```

La transferencia de llamadas implica tres partes: *transferente*, *transferido* y *destino de la transferencia*. El flujo de transferencia funciona de la siguiente manera:

1. Ya hay una llamada conectada entre el *transferente* y el *transferido*.
2. El *transferente* decide transferir la llamada (*transferido* -> *destino de la transferencia*).
3. El *transferente* llama a la API `transfer`.
4. El *transferido* decide si `accept` o `reject` la solicitud de transferencia al *destino de la transferencia* a través del evento `transferRequested`.
5. El *destino de la transferencia* recibirá una llamada entrante solo si el *transferido* `accept` la solicitud de transferencia.

### <a name="transfer-terminology"></a>Terminología sobre transferencias

- Transferente: Quien inicia la solicitud de transferencia.
- Transferido: A quien transfiere el transferente al destino de la transferencia.
- Destino de la transferencia: Quien es el destino al que se transfiere.

Para transferir la llamada actual, puede usar la API sincrónica `transfer`. `transfer` toma `TransferCallOptions` opcional, que permite establecer la marca `disableForwardingAndUnanswered`:

- `disableForwardingAndUnanswered` = false: Si el *destino de la transferencia* no responde a la llamada de transferencia, seguirá las configuraciones de reenvío y sin respuesta del *destino de la transferencia*.
- `disableForwardingAndUnanswered` = true: Si el *destino de la transferencia* no responde a la llamada de transferencia, el intento de transferencia finalizará.

```js
// transfer target can be ACS user
const id = { communicationUserId: <ACS_USER_ID> };
```

```js
// call transfer API
const transfer = callTransferApi.transfer({targetParticipant: id});
```

La transferencia le permite suscribirse a los eventos `transferStateChanged` y `transferRequested`. El evento `transferRequsted` procede de la instancia `call`, y el evento `transferStateChanged` y la transferencia `state` y `error` proceden de la instancia `transfer`.

```js
// transfer state
const transferState = transfer.state; // None | Transferring | Transferred | Failed

// to check the transfer failure reason
const transferError = transfer.error; // transfer error code that describes the failure if transfer request failed
```

El transferente puede aceptar o rechazar la solicitud de transferencia iniciada por el transferente en el evento `transferRequested` a través de `accept()` o `reject()` en `transferRequestedEventArgs`. Puede acceder a la información de `targetParticipant`, y los métodos `accept` y `reject` en `transferRequestedEventArgs`.

```js
// Transferee to accept the transfer request
callTransferApi.on('transferRequested', args => {
  args.accept();
});

// Transferee to reject the transfer request
callTransferApi.on('transferRequested', args => {
  args.reject();
});
```

## <a name="eventing-model"></a>Modelo de eventos
Tiene que inspeccionar los valores actuales y suscribirse a los eventos de actualización para los valores futuros.

### <a name="properties"></a>Propiedades

```js
// Inspect current value
console.log(object.property);

// Subscribe to value updates
object.on('propertyChanged', () => {
    // Inspect new value
    console.log(object.property)
});

// Unsubscribe from updates:
object.off('propertyChanged', () => {});



// Example for inspecting call state
console.log(call.state);
call.on('stateChanged', () => {
    console.log(call.state);
});
call.off('stateChanged', () => {});
```

### <a name="collections"></a>Colecciones
```js
// Inspect current collection
object.collection.forEach(v => {
    console.log(v);
});

// Subscribe to collection updates
object.on('collectionUpdated', e => {
    // Inspect new values added to the collection
    e.added.forEach(v => {
        console.log(v);
    });
    // Inspect values removed from the collection
    e.removed.forEach(v => {
        console.log(v);
    });
});

// Unsubscribe from updates:
object.off('collectionUpdated', () => {});



// Example for subscribing to remote participants and their video streams
call.remoteParticipants.forEach(p => {
    subscribeToRemoteParticipant(p);
})

call.on('remoteParticipantsUpdated', e => {
    e.added.forEach(p => { subscribeToRemoteParticipant(p) })
    e.removed.forEach(p => { unsubscribeFromRemoteParticipant(p) })
});

function subscribeToRemoteParticipant(p) {
    console.log(p.state);
    p.on('stateChanged', () => { console.log(p.state); });
    p.videoStreams.forEach(v => { subscribeToRemoteVideoStream(v) });
    p.on('videoStreamsUpdated', e => { e.added.forEach(v => { subscribeToRemoteVideoStream(v) }) })
}
```
