---
title: Introducción a los componentes base del marco de trabajo de la interfaz de usuario de Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: En este inicio rápido aprenderá a trabajar con los componentes base del marco de trabajo de la interfaz de usuario.
author: ddematheu2
ms.author: dademath
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 01fa817bd5173abc3b51e708b69b787c8b655e90
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539760"
---
# <a name="quickstart-get-started-with-ui-framework-base-components"></a>Inicio rápido: Introducción a los componentes base del marco de trabajo de la interfaz de usuario

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Comience a usar Azure Communication Services mediante el marco de trabajo de la interfaz de usuario para integrar rápidamente las experiencias de comunicación en sus aplicaciones. En este inicio rápido aprenderá a integrar los componentes base del marco de trabajo de la interfaz de usuario en la aplicación para crear experiencias de comunicación. 

Los componentes del marco de trabajo de la interfaz de usuario tienen dos versiones: base y compuesta.

- Los **componentes base** representan funcionalidades de comunicación discretas; son los bloques de creación básicos que se pueden usar para crear experiencias complejas de comunicación. 
- Los **componentes compuestos** son experiencias preconfiguradas para escenarios de comunicación comunes que se han creado con **componentes base**, como bloques de creación y empaquetados, para una integración sencilla en las aplicaciones.

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/), versiones Active LTS y Maintenance LTS (se recomienda Node 12).
- Recurso activo de Communication Services. [Creación de un recurso de Communication Services](./../create-communication-resource.md).
- Token de acceso de usuario para crear una instancia del cliente de llamada. Aprenda cómo [crear y administrar token de acceso de usuarios](./../access-tokens.md).

## <a name="setting-up"></a>Instalación

El marco de trabajo de la interfaz de usuario requiere que se configure un entorno de React. Eso es lo que vamos a hacer a continuación. Si ya tiene una aplicación React, puede omitir esta sección.

### <a name="set-up-react-app"></a>Configuración de la aplicación React

En este inicio rápido se usará la plantilla create-react-app. Para más información, consulte: [Introducción a React](https://reactjs.org/docs/create-a-new-react-app.html)

```console

npx create-react-app my-app

cd my-app

```

Al final de este proceso, debería tener una aplicación completa dentro de la carpeta `my-app`. En este inicio rápido se modificarán los archivos dentro de la carpeta `src`.

### <a name="install-the-package"></a>Instalar el paquete

Use el comando `npm install` para instalar la biblioteca cliente de llamadas de Azure Communication Services para JavaScript. Mueva el tarball proporcionado (versión preliminar privada) al directorio my-app.

```console

//For Private Preview install tarball

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
| Proveedor| Proveedor de la interfaz de usuario Fluent que permite a los desarrolladores modificar los componentes subyacentes de esta interfaz|
| CallingProvider| Proveedor de llamadas que crea una instancia de llamada. Necesario para agregar componentes adicionales|
| ChatProvider | Proveedor de chat que crea una instancia de un subproceso de chat. Necesario para agregar componentes adicionales|
| MediaGallery | Componente base que muestra los participantes de la llamada y sus transmisiones de vídeo remotas |
| MediaControls | Componente base para controlar la llamada, como el silenciador, el vídeo, y la pantalla compartida |
| ChatThread | Componente base que representa un subproceso de chat con indicadores tipográficos, confirmaciones de lectura, etc. |
| SendBox | Componente base que permite al usuario escribir mensajes que se enviarán al subproceso unido|

## <a name="initialize-calling-and-chat-providers-using-azure-communication-services-credentials"></a>Inicialización de proveedores de chat y llamadas mediante las credenciales de Azure Communication Services

Vaya a la carpeta `src` dentro de `my-app` y busque el archivo `app.js`. Aquí se colocará el siguiente código para inicializar los proveedores de chat y llamadas. Estos proveedores se encargan del mantenimiento del contexto de las experiencias de llamada y chat. Puede elegir cuál usar en función del tipo de experiencia de comunicación que va a crear. Si es necesario, puede usar ambos al mismo tiempo. Para inicializar los componentes, necesitará un token de acceso que se recupera de Azure Communication Services. Para más información sobre cómo obtener tokens de acceso, consulte [Creación y administración de tokens de acceso](./../access-tokens.md).

> [!NOTE]
> Los componentes no generan tokens de acceso, identificadores de grupo ni identificadores de subproceso. Estos elementos provienen de servicios que realizan los pasos adecuados para generar estos identificadores y pasarlos a la aplicación cliente. Para más información, consulte: [Arquitectura de cliente y servidor](./../../concepts/client-and-server-architecture.md).
> 
> Por ejemplo: El proveedor de chat espera que el `userId` asociado al `token` que se usa para inicializarlo ya se haya unido al `threadId` que se proporciona. Si el token no se ha unido al identificador del subproceso, se producirá un error en el proveedor de chat. Para más información sobre el chat, consulte: [Introducción al chat](./../chat/get-started.md)

Usaremos un tema de interfaz de usuario Fluent para mejorar el aspecto de la aplicación y la experiencia:

`App.js`
```javascript

import {CallingProvider, ChatProvider} from "@azure/acs-ui-sdk"
import { mergeThemes, teamsTheme } from '@fluentui/react-northstar';
import { Provider } from '@fluentui/react-northstar/dist/commonjs/components/Provider/Provider';
import { svgIconStyles } from '@fluentui/react-northstar/dist/es/themes/teams/components/SvgIcon/svgIconStyles';
import { svgIconVariables } from '@fluentui/react-northstar/dist/es/themes/teams/components/SvgIcon/svgIconVariables';
import * as siteVariables from '@fluentui/react-northstar/dist/es/themes/teams/siteVariables';

const iconTheme = {
  componentStyles: {
    SvgIcon: svgIconStyles
  },
  componentVariables: {
    SvgIcon: svgIconVariables
  },
  siteVariables
};

function App(props) {

  return (
    <Provider theme={mergeThemes(iconTheme, teamsTheme)}>
        <CallingProvider
        displayName={/*Insert Display Name to be used for the user*/}
        groupId={/*Insert GUID for group call to be joined*/}
        token={/*Insert the Azure Communication Services access token*/}
        refreshTokenCallback={/*Optional, Insert refresh token call back function*/}
        >
            // Add Calling Components Here
        </CallingProvider>

        {/*Note: Make sure that the userId associated to the token has been added to the provided threadId*/}

        <ChatProvider
        token={/*Insert the Azure Communication Services access token*/}
        displayName={/*Insert Display Name to be used for the user*/}
        threadId={/*Insert id for group chat thread to be joined*/}
        endpointUrl={/*Insert the environment URL for the Azure Resource used*/}
        refreshTokenCallback={/*Optional, Insert refresh token call back function*/}
        >
            //  Add Chat Components Here
        </ChatProvider>
    </Provider>
  );
}

export default App;

```

Una vez inicializado, este proveedor le permite crear su propio diseño mediante componentes base del marco de trabajo de la interfaz de usuario y cualquier lógica de diseño adicional. El proveedor se encarga de inicializar toda la lógica subyacente y de conectar adecuadamente los distintos componentes. A continuación, usaremos varios componentes base proporcionados por el marco de trabajo de la interfaz de usuario para crear experiencias de comunicación. Puede personalizar el diseño de estos componentes y agregar cualquier otro componente personalizado que desee representar con ellos. 

## <a name="build-ui-framework-calling-component-experiences"></a>Creación de experiencias de componente de llamada en el marco de trabajo de la interfaz de usuario

Para llamar usaremos los componentes `MediaGallery` y `MediaControls`. Para más información sobre ellos, consulte [Funcionalidades del marco de trabajo de la interfaz de usuario](./../../concepts/ui-framework/ui-sdk-features.md). Para empezar, en la carpeta `src`, cree un archivo llamado `CallingComponents.js`. Aquí inicializaremos un componente de función que contendrá nuestros componentes base para importarlos en `app.js`. Puede agregar diseños y estilos adicionales a los componentes. 

`CallingComponents.js`
```javascript

import {MediaGallery, MediaControls, MapToCallConfigurationProps, connectFuncsToContext} from "@azure/acs-ui-sdk"

function CallingComponents(props) {

  if (props.isCallInitialized) {props.joinCall()}

  return (
    <div style = {{height: '35rem', width: '30rem', float: 'left'}}>
        <MediaGallery/>
        <MediaControls/>
    </div>
  );
}

export default connectFuncsToContext(CallingComponents, MapToCallConfigurationProps);

```

En la parte inferior de este archivo, exportamos los componentes de llamada mediante el método `connectFuncsToContext` desde el marco de trabajo de la interfaz de usuario para conectar los componentes de la interfaz de usuario de llamada al estado subyacente mediante la función de asignación `MapToCallingSetupProps`. Este método hará que se rellenen las propiedades del componente, que luego usamos para comprobar el estado y unirnos a la llamada. Usaremos la propiedad `isCallInitialized` para comprobar si `CallAgent` está listo y, a continuación, el método `joinCall`, para unirnos. El marco de trabajo de la interfaz de usuario admite funciones de asignación personalizadas para su uso en escenarios donde los desarrolladores deseen controlar cómo se insertan los datos en los componentes.

## <a name="build-ui-framework-chat-component-experiences"></a>Creación de experiencias de componente de chat en el marco de trabajo de la interfaz de usuario

Para el chat usaremos los componentes `ChatThread` y `SendBox`. Para más información acerca de estos componentes, consulte [Funcionalidades del marco de trabajo de la interfaz de usuario](./../../concepts/ui-framework/ui-sdk-features.md). Para empezar, en la carpeta `src`, cree un archivo llamado `ChatComponents.js`. Aquí inicializaremos un componente de función que contendrá nuestros componentes base para importarlos en `app.js`.

`ChatComponents.js`
```javascript

import {ChatThread, SendBox} from '@azure/acs-ui-sdk'

function ChatComponents() {

  return (
    <div style = {{height: '35rem', width: '30rem', float: 'left'}}>
        <ChatThread />
        <SendBox />
    </div >
  );
}

export default ChatComponents;

```

## <a name="add-calling-and-chat-components-to-the-main-application"></a>Incorporación de componentes de llamada y chat a la aplicación principal

De nuevo en el archivo `app.js`, ahora agregaremos los componentes a `CallingProvider` y `ChatProvider` como se muestra a continuación.

`App.js`
```javascript

import ChatComponents from './ChatComponents';
import CallingComponents from './CallingComponents';

<Provider ... >
    <CallingProvider .... >
        <CallingComponents/>
    </CallingProvider>

    <ChatProvider .... >
        <ChatComponents />
    </ChatProvider>
</Provider>

```

## <a name="run-quickstart"></a>Ejecución del inicio rápido

Para ejecutar el código anterior, use el comando:

```console

npm run start   

```

Para probar completamente las funcionalidades, necesitará un segundo cliente con funcionalidades de llamada y chat para unirse a los subprocesos correspondientes. Consulte nuestro [ejemplo de elementos principales de una llamada](./../../samples/calling-hero-sample.md) y el [ejemplo de elementos principales de un chat](./../../samples/chat-hero-sample.md) como posibles opciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Prueba de los componentes compuestos del marco de trabajo de la interfaz de usuario](./get-started-with-composites.md)

Para obtener más información, consulte los siguientes recursos:
- [Introducción al marco de trabajo de la interfaz de usuario](../../concepts/ui-framework/ui-sdk-overview.md)
- [Funcionalidades del marco de trabajo de la interfaz de usuario](./../../concepts/ui-framework/ui-sdk-features.md)
