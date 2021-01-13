---
title: 'Guía de inicio rápido: incorporación de llamadas VOIP a una aplicación web mediante Azure Communication Services'
description: En este tutorial, aprenderá a usar la biblioteca de cliente de llamadas telefónicas de Azure Communication Services para JavaScript.
author: ddematheu
ms.author: nimag
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: f3d6023ffd3043bc57727fc39f077dd0ce7eccb8
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98024298"
---
En esta guía de inicio rápido, obtendrá información sobre cómo iniciar una llamada con la biblioteca de cliente de llamadas de Azure Communication Services para JavaScript.

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Versiones de [Node.js](https://nodejs.org/), Active LTS y Maintenance LTS (se recomiendan 8.11.1 y 10.14.1).
- Recurso activo de Communication Services. [Creación de un recurso de Communication Services](../../create-communication-resource.md).
- Token de acceso de usuario para crear una instancia del cliente de llamada. Aprenda cómo [crear y administrar token de acceso de usuarios](../../access-tokens.md).


[!INCLUDE [Calling with JavaScript](./get-started-javascript-setup.md)]

Este es el código:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Cree un archivo en el directorio raíz del proyecto denominado **client.js** que contendrá la lógica de la aplicación para esta guía de inicio rápido. Agregue el siguiente código para importar el cliente que realiza la llamada y obtener referencias a los elementos DOM para que podamos adjuntar la lógica de negocios. 

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca cliente de llamadas de Azure Communication Services:

| Nombre                             | Descripción                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | CallClient es el punto de entrada principal a la biblioteca cliente de llamadas.                                                                       |
| CallAgent                        | CallAgent se usa para iniciar y administrar llamadas.                                                                                            |
| AzureCommunicationUserCredential | La clase AzureCommunicationUserCredential implementa la interfaz CommunicationUserCredential interface que se usa para crear una instancia de CallAgent. |


## <a name="authenticate-the-client"></a>Autenticar el cliente

Debe reemplazar `<USER_ACCESS_TOKEN>` por un token de acceso de usuario válido para el recurso. Consulte la documentación relativa al [token de acceso de usuario](../../access-tokens.md) si aún no tiene ningún token disponible. Con el `CallClient`, inicialice una instancia de `CallAgent` con un `CommunicationUserCredential` que nos permita realizar y recibir llamadas. Agregue el código siguiente a **client.js**:

```javascript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential);
    callButton.disabled = false;
}
init();
```

## <a name="start-a-call"></a>Iniciar una llamada

Agregue un controlador de eventos para iniciar una llamada cuando se haga clic en el `callButton`:

```javascript
callButton.addEventListener("click", () => {
    // start a call
    const userToCall = calleeInput.value;
    call = callAgent.call(
        [{ communicationUserId: userToCall }],
        {}
    );
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```

## <a name="end-a-call"></a>Finalizar una llamada

Agregue un cliente de escucha de eventos para finalizar la llamada actual cuando se haga clic en el `hangUpButton`:

```javascript
hangUpButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({ forEveryone: true });

  // toggle button states
  hangUpButton.disabled = true;
  callButton.disabled = false;
});
```

La propiedad `forEveryone` finaliza la llamada para todos los participantes.

## <a name="run-the-code"></a>Ejecución del código

Utilice `webpack-dev-server` para compilar y ejecutar la aplicación. Ejecute el siguiente comando para agrupar el host de aplicación en un servidor web local:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Abra el explorador web y vaya a http://localhost:8080/. Verá lo siguiente:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Captura de pantalla de la aplicación JavaScript completada.":::

Para hacer una llamada de VOIP saliente, proporcione un identificador de usuario en el campo de texto y haga clic en el botón **Iniciar llamada** . La llamada a `8:echo123` le conecta a un bot de eco; esto es excelente como introducción y para comprobar que los dispositivos de audio funcionan.
