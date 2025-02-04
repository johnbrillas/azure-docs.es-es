---
title: Configuración de Remote Rendering para Unity
description: Inicialización de Azure Remote Rendering en un proyecto de Unity
author: jakrams
ms.author: jakras
ms.date: 02/27/2020
ms.topic: how-to
ms.custom: devx-track-csharp
ms.openlocfilehash: 48f01058d8e879a9610e76638215214c059982fa
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99594221"
---
# <a name="set-up-remote-rendering-for-unity"></a>Configuración de Remote Rendering para Unity

Para habilitar Azure Remote Rendering (ARR) en Unity, proporcionamos métodos dedicados que se encargan de algunos aspectos específicos de Unity.

## <a name="startup-and-shutdown"></a>Inicio y apagado

Para inicializar Remote Rendering, use `RemoteManagerUnity`. Esta clase llama al elemento `RenderingConnection` genérico, pero implementa detalles específicos de Unity automáticamente. Por ejemplo, Unity usa un sistema de coordenadas específico. Al llamar a `RemoteManagerUnity.Initialize`, se configurará la convención adecuada. La llamada también requiere que proporcione la cámara de Unity que se debe usar para mostrar el contenido representado de forma remota.

```cs
// initialize Azure Remote Rendering for use in Unity:
// it needs to know which camera is used for rendering the scene
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);
```

Para apagar Remote Rendering, llame a `RemoteManagerStatic.ShutdownRemoteRendering()`.

Una vez que se ha creado un elemento `RenderingSession` y se ha elegido como sesión de representación principal, se debe registrar con `RemoteManagerUnity`:

```cs
RemoteManagerUnity.CurrentSession = ...
```

### <a name="full-example-code"></a>Ejemplo de código completo

En el código siguiente se muestran todos los pasos necesarios para inicializar Azure Remote Rendering en Unity:

```cs
// initialize Remote Rendering
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);

// create a frontend
SessionConfiguration sessionConfig = new SessionConfiguration();
// ... fill out sessionConfig ...
RemoteRenderingClient client = new RemoteRenderingClient(sessionConfig);

// start a session
CreateRenderingSessionResult result = await client.CreateNewRenderingSessionAsync(new RenderingSessionCreationOptions(RenderingSessionVmSize.Standard, 0, 30));
RenderingSession session = result.Session;

// let RemoteManagerUnity know about the session we want to use
RemoteManagerUnity.CurrentSession = session;

await session.ConnectAsync(new RendererInitOptions());

/// When connected, load and modify content

RemoteManagerStatic.ShutdownRemoteRendering();
```

## <a name="convenience-functions"></a>Funciones de conveniencia

### <a name="session-state-events"></a>Eventos de estado de sesión

`RemoteManagerUnity.OnSessionUpdate` emite eventos para cuando su estado de sesión cambie. Consulte la documentación del código para obtener más información.

### <a name="arrserviceunity"></a>ARRServiceUnity

`ARRServiceUnity` es un componente opcional para simplificar la configuración y la administración de sesiones. Contiene opciones para detener automáticamente la sesión cuando la aplicación se está cerrando o se sale del modo de reproducción en el editor, así como para renovar automáticamente la concesión de la sesión cuando sea necesario. Almacena en la memoria caché datos como las propiedades de sesión (vea su variable `LastProperties`) y expone eventos para los cambios de estado de sesión y los errores de sesión.

No puede haber más de una instancia de `ARRServiceUnity` a la vez. Está pensada para que pueda empezar a trabajar más rápidamente implementando cierta funcionalidad común. Sin embargo, para una aplicación de mayor tamaño, puede ser preferible hacer esas cosas por sí mismo.

Para obtener un ejemplo de configuración y uso de `ARRServiceUnity`, consulte [Tutorial: Visualización de modelos representados de forma remota](../../tutorials/unity/view-remote-models/view-remote-models.md).

## <a name="next-steps"></a>Pasos siguientes

* [Instalación del paquete de Remote Rendering para Unity](install-remote-rendering-unity-package.md)
* [Tutorial: Visualización de modelos representados de forma remota](../../tutorials/unity/view-remote-models/view-remote-models.md)
