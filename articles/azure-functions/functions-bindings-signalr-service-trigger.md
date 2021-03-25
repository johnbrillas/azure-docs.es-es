---
title: Enlace del desencadenador de SignalR Service para Azure Functions
description: Obtenga información sobre cómo enviar mensajes de SignalR Service desde Azure Functions.
author: chenyl
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 05/11/2020
ms.author: chenyl
ms.openlocfilehash: 2482a26987ec142880acc51bf470d844655b6e3f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97763527"
---
# <a name="signalr-service-trigger-binding-for-azure-functions"></a>Enlace del desencadenador de SignalR Service para Azure Functions

Use el enlace del desencadenador de *SignalR* para responder a mensajes enviados desde Azure SignalR Service. Cuando se desencadena la función, los mensajes pasados a la función se analizan como un objeto JSON.

En el modo sin servidor de SignalR Service, SignalR Service usa la característica [Upstream](../azure-signalr/concept-upstream.md) para enviar mensajes del cliente a la aplicación de funciones. Y la aplicación de funciones usa el enlace de desencadenador de SignalR Service para controlar estos mensajes. A continuación se muestra la arquitectura general: :::image type="content" source="media/functions-bindings-signalr-service/signalr-trigger.png" alt-text="Arquitectura de desencadenadores de SignalR":::

Para obtener información sobre los detalles de instalación y configuración, consulte la [información general](functions-bindings-signalr-service.md).

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una función que recibe un mensaje mediante el enlace del desencadenador y registra el mensaje.

# <a name="c"></a>[C#](#tab/csharp)

El enlace del desencadenador de SignalR Service para C# tiene dos modelos de programación. Modelo basado en clases y modelo tradicional. El modelo basado en clases puede ofrecer una experiencia coherente de programación del lado servidor de SignalR. El modelo tradicional proporciona más flexibilidad con otros enlaces de función.

### <a name="with-class-based-model"></a>Modelo basado en clases

Vea [Modelo basado en clases](../azure-signalr/signalr-concept-serverless-development-config.md#class-based-model) para más información.

```cs
public class SignalRTestHub : ServerlessHub
{
    [FunctionName("SignalRTest")]
    public async Task SendMessage([SignalRTrigger]InvocationContext invocationContext, string message, ILogger logger)
    {
        logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
    }
}
```

### <a name="with-traditional-model"></a>Modelo tradicional

El modelo tradicional obedece la convención de Azure Functions desarrollada por C#. Si no está familiarizado con ella, puede consultar diversos [documentos](./functions-dotnet-class-library.md).

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage", parameterNames: new string[] {"message"})]InvocationContext invocationContext, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

#### <a name="use-attribute-signalrparameter-to-simplify-parameternames"></a>Uso del atributo `[SignalRParameter]` para simplificar `ParameterNames`

Dado que es complicado usar `ParameterNames`, se proporciona `SignalRParameter` para lograr el mismo propósito.

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage")]InvocationContext invocationContext, [SignalRParameter]string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using System;
using Microsoft.Azure.WebJobs.Extensions.SignalRService;
using Microsoft.Extensions.Logging;

public static void Run(InvocationContext invocation, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, invocation) {
    context.log(`Receive ${context.bindingData.message} from ${invocation.ConnectionId}.`)
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Este es el código de Python:

```python
import logging
import json
import azure.functions as func

def main(invocation) -> None:
    invocation_json = json.loads(invocation)
    logging.info("Receive {0} from {1}".format(invocation_json['Arguments'][0], invocation_json['ConnectionId']))
```

---

## <a name="configuration"></a>Configuración

### <a name="signalrtrigger"></a>SignalRTrigger

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `SignalRTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type**| N/D | Se debe establecer en `SignalRTrigger`.|
|**direction**| N/D | Se debe establecer en `in`.|
|**name**| N/D | Nombre de variable utilizado en el código de función para el objeto de contexto de invocación del desencadenador. |
|**hubName**|**HubName**| Este valor debe establecerse en el nombre del centro de SignalR para la función que se va a desencadenar.|
|**category**|**Categoría**| Este valor debe establecerse como la categoría de mensajes para la función que se va a desencadenar. La categoría puede tener uno de estos valores: <ul><li>**conexiones**: que incluye eventos *conectados* y *desconectados*.</li><li>**mensajes**: que incluye todos los demás eventos, excepto los de la categoría *conexiones*.</li></ul> |
|**event**|**Evento**| Este valor debe establecerse como el evento de mensajes para la función que se va a desencadenar. En la categoría *mensajes*, el evento es el *objetivo* en el [mensaje de invocación](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) que envían los clientes. En la categoría *conexiones*, solo se usan *conectados* y *desconectados*. |
|**parameterNames**|**parameterNames**| (Opcional) Lista de nombres que se enlaza a los parámetros. |
|**connectionStringSetting**|**ConnectionStringSetting**| El nombre de la configuración de la aplicación que contiene la cadena de conexión de SignalR Service (el valor predeterminado es "AzureSignalRConnectionString") |

## <a name="payload"></a>Carga

El tipo de entrada del desencadenador se declara como `InvocationContext` o como un tipo personalizado. Si elige `InvocationContext`, obtiene acceso completo al contenido de la solicitud. En un tipo personalizado, el runtime intenta analizar el cuerpo de la solicitud JSON para establecer las propiedades del objeto.

### <a name="invocationcontext"></a>InvocationContext

InvocationContext incluye todo el contenido del mensaje enviado desde SignalR Service.

|Propiedad en InvocationContext | Descripción|
|------------------------------|------------|
|Argumentos| Disponible para la categoría *mensajes*. Contiene *argumentos* en el [mensaje de invocación](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding).|
|Error| Disponible para evento *desconectado*. Puede estar vacío si la conexión se cerró sin errores o puede contener los mensajes de error.|
|Hub| El nombre del centro al que pertenece el mensaje.|
|Category| Categoría del mensaje.|
|Evento| Evento del mensaje.|
|ConnectionId| El identificador de conexión del cliente que envía el mensaje.|
|UserId| La identidad del usuario del cliente que envía el mensaje.|
|encabezados| los encabezados de la solicitud.|
|Consultar| La consulta de la solicitud cuando los clientes se conectan al servicio.|
|Notificaciones| Las notificaciones del cliente.|

## <a name="using-parameternames"></a>Usar `ParameterNames`

La propiedad `ParameterNames` en `SignalRTrigger` permite enlazar argumentos de mensajes de invocación con los parámetros de funciones. El nombre que ha definido se puede usar como parte de [expresiones de enlace](../azure-functions/functions-bindings-expressions-patterns.md) en otro enlace o como parámetros en el código. Esto proporciona una manera más práctica de acceder a los argumentos de `InvocationContext`.

Imagine que tiene un cliente de SignalR de JavaScript que intenta invocar el método `broadcast` en Azure Functions con dos argumentos: `message1`, `message2`.

```javascript
await connection.invoke("broadcast", message1, message2);
```

Después de establecer `parameterNames`, el nombre definido se corresponderá respectivamente con los argumentos enviados en el lado cliente. 

```cs
[SignalRTrigger(parameterNames: new string[] {"arg1, arg2"})]
```

Después, `arg1` incluirá el contenido de `message1` y `arg2` el de `message2`.


### <a name="remarks"></a>Observaciones

Al enlazar parámetros, el orden es importante. Si está utilizando `ParameterNames`, el orden de `ParameterNames` coincide con el orden de los argumentos que se invocan en el cliente. Si usa el atributo `[SignalRParameter]` en C#, el orden de los argumentos en los métodos de Azure Functions coincide con el orden de los argumentos en los clientes.

`ParameterNames` y el atributo `[SignalRParameter]` **no se pueden** usar al mismo tiempo o se producirá una excepción.

## <a name="signalr-service-integration"></a>Integración de SignalR Service

SignalR Service necesita una dirección URL para acceder a la aplicación de funciones cuando se usa el enlace de desencadenador de SignalR Service. La dirección URL se debe configurar en **Configuración ascendente** en el lado de SignalR Service. 

:::image type="content" source="../azure-signalr/media/concept-upstream/upstream-portal.png" alt-text="Portal de Upstream":::

Al usar el desencadenador de SignalR Service, la dirección URL puede ser sencilla y tener el formato que se muestra a continuación:

```http
<Function_App_URL>/runtime/webhooks/signalr?code=<API_KEY>
```

`Function_App_URL` se puede encontrar en la página Información general de la aplicación de funciones y la función de Azure la genera `API_KEY`. Puede obtener `API_KEY` de `signalr_extension` en la hoja **Claves de la aplicación** de la aplicación de funciones.
:::image type="content" source="media/functions-bindings-signalr-service/signalr-keys.png" alt-text="Clave de API":::

Si quiere usar más de una aplicación de funciones junto con una instancia de SignalR Service, Upstream también puede admitir reglas de enrutamiento complejas. Encuentre más información en [Configuración ascendente](../azure-signalr/concept-upstream.md).

## <a name="step-by-step-sample"></a>Ejemplo paso a paso

Puede seguir el ejemplo de GitHub para implementar un salón de chat en una aplicación de funciones con la característica de enlace de desencadenador y Upstream de SignalR Service: [Ejemplo de salón de chat bidireccional](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BidirectionChat)

## <a name="next-steps"></a>Pasos siguientes

* [Desarrollo y configuración de Azure Functions con Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md)
* [Ejemplo de enlace del desencadenador de SignalR Service](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BidirectionChat)
