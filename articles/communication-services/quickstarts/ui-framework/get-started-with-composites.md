---
title: Introducción a los componentes compuestos del SDK del marco de trabajo de la interfaz de usuario de Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: En este inicio rápido, aprenderá a trabajar con los componentes compuestos del marco de trabajo de la interfaz de usuario.
author: ddematheu2
ms.author: dademath
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 17a8369463a27acffc8bdc52c48e4ae0624f41cd
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539717"
---
# <a name="quickstart-get-started-with-ui-framework-composite-components"></a>Inicio rápido: Introducción a los componentes compuestos del marco de trabajo de la interfaz de usuario

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Comience a usar Azure Communication Services mediante el marco de trabajo de la interfaz de usuario para integrar rápidamente las experiencias de comunicación en sus aplicaciones. En este inicio rápido, aprenderá a integrar los componentes compuestos del marco de trabajo de la interfaz de usuario en la aplicación para crear experiencias de comunicación.

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/), versiones Active LTS y Maintenance LTS (se recomienda Node 12).
- Recurso activo de Communication Services. [Creación de un recurso de Communication Services](./../create-communication-resource.md).
- Token de acceso de usuario para crear una instancia del compuesto de llamada. Aprenda cómo [crear y administrar token de acceso de usuarios](./../access-tokens.md).

## <a name="setting-up"></a>Instalación

El marco de trabajo de la interfaz de usuario requiere que se configure un entorno de React. Eso es lo que vamos a hacer a continuación. Si ya tiene una aplicación React, puede omitir esta sección.

### <a name="set-up-react-app"></a>Configuración de la aplicación React

En este inicio rápido se usará la plantilla create-react-app. Para más información, consulte: [Introducción a React](https://reactjs.org/docs/create-a-new-react-app.html)

```console

npx create-react-app my-app

cd my-app

```

Al final de este proceso, debería tener una aplicación completa dentro de la carpeta `my-app`. En este inicio rápido, se modificarán los archivos dentro de la carpeta `src`.

### <a name="install-the-package"></a>Instalar el paquete

Use el comando `npm install` para instalar la biblioteca cliente de llamadas de Azure Communication Services para JavaScript. Mueva el tarball proporcionado (versión preliminar privada) al directorio my-app.

```console

//Private Preview install tarball

npm install --save ./{path for tarball}

```

La opción `--save` muestra la biblioteca como dependencia en el archivo **package.json**.

### <a name="run-create-react-app"></a>Ejecución de create-react-app

Vamos a probar la instalación de la plantilla create-react-app mediante la ejecución de:

```console

npm run start 

```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca cliente de la interfaz de usuario de Azure Communication Services:

| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| GroupCall | Componente compuesto que representa una experiencia de llamada grupal con galería de participantes y controles. |
| GroupChat | Componente compuesto que representa una experiencia de chat grupal con subproceso y entrada de chat |


## <a name="initialize-group-call-and-group-chat-composite-components"></a>Inicialización de los componentes compuestos de llamada y de chat grupal

Vaya a la carpeta `src` dentro de `my-app` y busque el archivo `app.js`. Aquí vamos a insertar el siguiente código para inicializar los componentes compuestos de chat y de llamada grupal. Puede elegir cuál usar en función del tipo de experiencia de comunicación que se va a crear. Si es necesario, puede usar ambos al mismo tiempo. Para inicializar los componentes, necesitará un token de acceso que se recupera de Azure Communication Services. Para más información sobre cómo obtener tokens de acceso, consulte: [Creación y administración de tokens de acceso del usuario](./../access-tokens.md).

> [!NOTE]
> Los componentes no generan tokens de acceso, identificadores de grupo ni identificadores de subprocesos. Estos elementos provienen de servicios que realizan los pasos adecuados para generar estos identificadores y pasarlos a la aplicación cliente. Para más información, consulte: [Arquitectura de cliente y servidor](./../../concepts/client-and-server-architecture.md).
> 
> Por ejemplo: El compuesto de chat grupal espera que el `userId` asociado al `token` que se usa para inicializarlo ya se haya unido al `threadId` que se proporciona. Si el token no se ha unido al identificador del subproceso, se producirá un error en el compuesto de chat grupal. Para más información sobre el chat, consulte: [Introducción al chat](./../chat/get-started.md)


`App.js`
```javascript

import {GroupCall, GroupChat} from "@azure/acs-ui-sdk"

function App(){

    return(<>
        {/* Example styling provided, developers can provide their own styling to position and resize components */}
        <div style={{height: "35rem", width: "50rem", float: "left"}}>
            <GroupCall
                displayName={DISPLAY_NAME} //Required, Display name for the user entering the call
                token={TOKEN} // Required, Azure Communication Services access token retrieved from authentication service
                refreshTokenCallback={CALLBACK} //Optional, Callback to refresh the token in case it expires
                groupId={GROUPID} //Required, Id for group call that will be joined. (GUID)
                onEndCall = { () => {
                    //Optional, Action to be performed when the call ends
                }}
            />
        </div>

        {/*Note: Make sure that the userId associated to the token has been added to the provided threadId*/}
        {/* Example styling provided, developers can provide their own styling to position and resize components */}
        <div style={{height: "35rem", width: "30rem", float: "left"}}>
            <GroupChat 
                displayName={DISPLAY_NAME} //Required, Display name for the user entering the call
                token={TOKEN} // Required, Azure Communication Services access token retrieved from authentication service
                threadId={THREADID} //Required, Id for group chat thread that will be joined.
                endpointUrl={ENDPOINT_URL} //Required, URL for Azure endpoint being used for Azure Communication Services
                onRenderAvatar = { (acsId) => {
                    //Optional, function to override the avatar image on the chat thread. Function receives one parameters for the Azure Communication Services Identity. Must return a React element.
                }}
                refreshToken = { () => {
                    //Optional, function to refresh the access token in case it expires
                }}
                options = {{
                    //Optional, options to define chat behavior
                    sendBoxMaxLength: number | undefined //Optional, Limit the max send box length based on viewport size change.
                }}
            />
        </div>
    </>);
}

export default App;

```

## <a name="run-quickstart"></a>Ejecución del inicio rápido

Para ejecutar el código anterior, use el comando:

```console

npm run start 

```

Para probar completamente las capacidades, necesitará un segundo cliente con funcionalidades de llamada y chat para unirse a los subprocesos correspondientes. Consulte nuestro [ejemplo de elementos principales de una llamada](./../../samples/calling-hero-sample.md) y el [ejemplo de elementos principales de un chat](./../../samples/chat-hero-sample.md) como posibles opciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Probar componentes básicos del marco de trabajo de la interfaz de usuario](./get-started-with-components.md)

Para obtener más información, consulte los siguientes recursos:
- [Introducción al marco de trabajo de la interfaz de usuario](../../concepts/ui-framework/ui-sdk-overview.md)
- [Funcionalidades del marco de trabajo de la interfaz de usuario](./../../concepts/ui-framework/ui-sdk-features.md)
