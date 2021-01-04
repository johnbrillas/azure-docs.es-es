---
title: 'Inicio rápido: unirse a una reunión de Teams'
author: chpalm
ms.author: mikben
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: c72083f205fae77de366125e666cee479fd46805
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97692000"
---
## <a name="prerequisites"></a>Requisitos previos

- Una [aplicación de llamadas de Communication Services](../getting-started-with-calling.md) operativa.
- Una [implementación de Teams](/deployoffice/teams-install).

## <a name="enable-teams-interoperability"></a>Habilitación de la interoperabilidad de Teams

La característica de interoperabilidad de Teams se encuentra actualmente en versión preliminar privada. Para habilitar esta característica para el recurso de Communication Services, envíe un correo electrónico a [acsfeedback@microsoft.com](mailto:acsfeedback@microsoft.com) que incluya:

1. El identificador de la suscripción de Azure que contiene el recurso de Communication Services.
2. El identificador de inquilino de Teams. La forma más fácil de conseguirlo es [obtener y compartir un vínculo con la instancia de Teams](https://support.microsoft.com/office/create-a-link-or-a-code-for-joining-a-team-11b0de3b-9288-4cb4-bc49-795e7028296f).

Para usar esta característica debe ser miembro de la organización propietaria de ambas entidades.

## <a name="add-the-teams-ui-controls"></a>Incorporación de los controles de la interfaz de usuario de Teams

Reemplace el código de index.html por el siguiente fragmento de código.
El cuadro de texto se utilizará para especificar el contexto de reunión de Teams y el botón se usará para la unión a la reunión especificada:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - Calling Sample</title>
</head>
<body>
    <h4>Azure Communication Services</h4>
    <h1>Teams meeting join quickstart</h1>
    <input id="teams-link-input" type="text" placeholder="Teams meeting link"
        style="margin-bottom:1em; width: 300px;" />
        <p>Call state <span style="font-weight: bold" id="call-state">-</span></p>
    <div>
        <button id="join-meeting-button" type="button" disabled="false">
            Join Teams Meeting
        </button>
        <button id="hang-up-button" type="button" disabled="true">
            Hang Up
        </button>
    </div>
    <script src="./bundle.js"></script>
</body>

</html>
```

## <a name="enable-the-teams-ui-controls"></a>Habilitación de los controles de la interfaz de usuario de Teams

Reemplace el contenido del archivo client.js por el siguiente fragmento de código.

```javascript
import { CallClient } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
let callAgent;
const meetingLinkInput = document.getElementById('teams-link-input');
const hangUpButton = document.getElementById('hang-up-button');
const teamsMeetingJoinButton = document.getElementById('join-meeting-button');
const callStateElement = document.getElementById('call-state');

async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'ACS user'});
    teamsMeetingJoinButton.disabled = false;
}
init();

hangUpButton.addEventListener("click", async () => {
    // end the current call
    await call.hangUp();
  
    // toggle button states
    hangUpButton.disabled = true;
    teamsMeetingJoinButton.disabled = false;
    callStateElement.innerText = '-';
  });

teamsMeetingJoinButton.addEventListener("click", () => {    
    // join with meeting link
    call = callAgent.join({meetingLink: meetingLinkInput.value}, {});
    
    call.on('callStateChanged', () => {
        callStateElement.innerText = call.state;
    })
    // toggle button states
    hangUpButton.disabled = false;
    teamsMeetingJoinButton.disabled = true;
});
```

## <a name="get-the-teams-meeting-link"></a>Obtención del vínculo de la reunión de Teams

El vínculo de la reunión de Teams se puede recuperar mediante las Graph API. Esto se detalla en la [documentación de Graph](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta).
El SDK de llamada de Communication Services acepta un vínculo a toda la reunión de Teams. Este vínculo se devuelve como parte del recurso `onlineMeeting`, al que se accede bajo la propiedad [`joinWebUrl`](/graph/api/resources/onlinemeeting?view=graph-rest-beta). También puede obtener la información necesaria de la reunión de la dirección URL **Unirse a la reunión** de la propia invitación a la reunión de Teams.

## <a name="run-the-code"></a>Ejecución del código

Los usuarios de WebPack pueden usar `webpack-dev-server` para compilar y ejecutar la aplicación. Ejecute el siguiente comando para agrupar el host de aplicación en un servidor web local:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Abra el explorador web y vaya a http://localhost:8080/. Verá lo siguiente:

:::image type="content" source="../media/javascript/acs-join-teams-meeting-quickstart.PNG" alt-text="Captura de pantalla de la aplicación JavaScript completada.":::

Inserte el contexto de Teams en el cuadro de texto y presione *Join Teams Meeting* (Unirse a la reunión de Teams) para unirse a la reunión de Teams desde dentro de la aplicación de Communication Services.
