---
title: Creación del propio componente del marco de la interfaz de usuario
titleSuffix: An Azure Communication Services quickstart
description: En este inicio rápido, aprenderá a crear un componente personalizado compatible con el marco de trabajo de la interfaz de usuario.
author: ddematheu2
ms.author: dademath
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 093fcfd95d291d959ed49cc39a227a99f14a0383
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103488251"
---
# <a name="quickstart-create-your-own-ui-framework-component"></a>Inicio rápido: Creación de su propio componente del marco de trabajo de la interfaz de usuario

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Comience a usar Azure Communication Services mediante el marco de trabajo de la interfaz de usuario para integrar rápidamente las experiencias de comunicación en sus aplicaciones.

En este inicio rápido, aprenderá a crear sus propios componentes mediante la interfaz de estado predefinida que ofrece el marco de trabajo de la interfaz de usuario. Este enfoque es idóneo para los desarrolladores que necesitan más personalización y desean usar sus propios recursos de diseño para la experiencia. 

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/), versiones Active LTS y Maintenance LTS (se recomienda Node 12).
- Recurso activo de Communication Services. [Creación de un recurso de Communication Services](./../create-communication-resource.md).
- Token de acceso de usuario para crear una instancia del cliente de llamada. Aprenda cómo [crear y administrar token de acceso de usuarios](./../access-tokens.md).

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
| CallingProvider| Proveedor de llamada que crea una instancia de llamada. Necesario para agregar componentes básicos|
| ChatProvider | Proveedor de chat que crea una instancia de un subproceso de chat. Necesario para agregar componentes básicos|
| connectFuncsToContext | Método para conectar los componentes del marco de trabajo de la interfaz de usuario con los proveedores subyacentes mediante asignadores |
| MapToChatMessageProps | Asignador de capas de datos de mensajes de chat que proporciona componentes con propiedades de mensajes de chat |


## <a name="initialize-chat-providers-using-azure-communication-services-credentials"></a>Inicialización de proveedores de chat mediante las credenciales de Azure Communication Services

En este inicio rápido, se usará un chat como ejemplo. Para más información sobre las llamadas, consulte [Inicio rápido de componentes básicos](./get-started-with-components.md) e [Inicio rápido de componentes compuestos](./get-started-with-composites.md).

Vaya a la carpeta `src` dentro de `my-app` y busque el archivo `app.js`. Aquí se colocará el siguiente código para inicializar el proveedor de chat. Este proveedor se encarga del mantenimiento del contexto de las experiencias de llamada y chat. Para inicializar los componentes, necesitará un token de acceso que se recupera de Azure Communication Services. Para más información sobre cómo obtener un token de acceso, consulte: [Creación y administración de tokens de acceso](./../access-tokens.md).

Los componentes del marco de trabajo de la interfaz de usuario siguen la misma arquitectura general en el resto del servicio. Los componentes no generan tokens de acceso, identificadores de grupo ni identificadores de subprocesos. Estos elementos provienen de servicios que realizan los pasos adecuados para generar estos identificadores y pasarlos a la aplicación cliente. Para más información, consulte [Arquitectura de cliente y servidor](./../../concepts/client-and-server-architecture.md).

`App.js`
```javascript

import {CallingProvider, ChatProvider} from "@azure/acs-ui-sdk"

function App(props) {

  return (
    <ChatProvider
    token={/*Insert the Azure Communication Services access token*/}
    userId={/*Insert the Azure Communication Services user id*/}
    displayName={/*Insert Display Name to be used for the user*/}
    threadId={/*Insert id for group chat thread to be joined*/}
    endpointUrl={/*Insert the environment URL for the Azure Resource used*/}
    refreshTokenCallback={/*Optional, Insert refresh token call back function*/}
    >
        //  Add Chat Components Here
    </ChatProvider>
  );
}

export default App;

```

Una vez inicializado, este proveedor le permite crear su propio diseño mediante el componente del marco de trabajo de la interfaz de usuario y cualquier lógica de diseño adicional. El proveedor se encarga de inicializar toda la lógica subyacente y de conectar adecuadamente los distintos componentes. A continuación, vamos a crear un componente personalizado con los asignadores del marco de trabajo de la interfaz de usuario para conectarse a nuestro proveedor de chat.


## <a name="create-a-custom-component-using-mappers"></a>Creación de un componente personalizado mediante asignadores

Vamos a crear primero un nuevo archivo llamado `SimpleChatThread.js` en el que se creará el componente. Importaremos los componentes del marco de trabajo de la interfaz de usuario que necesitemos. En este caso, se usará HTML predefinido y React para crear un componente totalmente personalizado para un sencillo subproceso de chat. Con el método `connectFuncsToContext`, usaremos el asignador `MapToChatMessageProps` para asignar propiedades a los componentes personalizados de `SimpleChatThread`. Estas propiedades nos darán acceso a los mensajes de chat que se envían y se reciben para rellenarlos en nuestro subproceso sencillo.

`SimpleChatThread.js`
```javascript

import {connectFuncsToContext, MapToChatMessageProps} from "@azure/acs-ui-sdk"

function SimpleChatThread(props) {

    return (
        <div>
            {props.chatMessages?.map((message) => (
                <div key={message.id ?? message.clientMessageId}> {`${message.senderDisplayName}: ${message.content}`}</div>
            ))}
        </div>
    );
}

export default connectFuncsToContext(SimpleChatThread, MapToChatMessageProps);

```

## <a name="add-your-custom-component-to-your-application"></a>Incorporación del componente personalizado a la aplicación

Ahora que tenemos nuestro componente personalizado listo, lo importaremos y lo agregaremos al diseño.

```javascript

import {CallingProvider, ChatProvider} from "@azure/acs-ui-sdk"
import SimpleChatThread from "./SimpleChatThread"

function App(props) {

  return (
        <ChatProvider ... >
            <SimpleChatThread />
        </ChatProvider>
  );
}

export default App;

```

## <a name="run-quickstart"></a>Ejecución del inicio rápido

Para ejecutar el código anterior, use el comando:

```console

npm run start   

```

Para probar completamente las funcionalidades, necesitará un segundo cliente con funcionalidad de chat para enviar los mensajes que recibirá nuestro sencillo subproceso de chat. Consulte nuestro [ejemplo de elementos principales de una llamada](./../../samples/calling-hero-sample.md) y el [ejemplo de elementos principales de un chat](./../../samples/chat-hero-sample.md) como posibles opciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Prueba de los componentes compuestos del marco de trabajo de la interfaz de usuario](./get-started-with-composites.md)

Para obtener más información, consulte los siguientes recursos:
- [Introducción al marco de trabajo de la interfaz de usuario](../../concepts/ui-framework/ui-sdk-overview.md)
- [Funcionalidades del marco de trabajo de la interfaz de usuario](./../../concepts/ui-framework/ui-sdk-features.md)
- [Inicio rápido de componentes básicos del marco de trabajo de la interfaz de usuario](./get-started-with-components.md)
