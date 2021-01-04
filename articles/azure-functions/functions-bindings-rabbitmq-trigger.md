---
title: Desencadenador de RabbitMQ para Azure Functions
description: Aprenda a ejecutar una instancia de Azure Functions al crear un mensaje de RabbitMQ.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/13/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: e7095c08c385457bddf6d70d345c4f47073b4adb
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505677"
---
# <a name="rabbitmq-trigger-for-azure-functions-overview"></a>Introducción al desencadenador de RabbitMQ para Azure Functions

> [!NOTE]
> Los enlaces de RabbitMQ solo se admiten por completo en los planes **Premium de Windows**. Consumo y Linux no se admiten actualmente.

Use el desencadenador de RabbitMQ para responder a los mensajes de una cola de RabbitMQ.

Para obtener información sobre los detalles de instalación y configuración, vea la [información general](functions-bindings-rabbitmq.md).

## <a name="example"></a>Ejemplo

# <a name="c"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee y registra el mensaje de RabbitMQ como un [evento de RabbitMQ](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html):

```cs
[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] BasicDeliverEventArgs args,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(args.Body)}");
}
```

En el ejemplo siguiente se muestra cómo leer el mensaje como POCO.

```cs
public class TestClass
{
    public string x { get; set; }
}

[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] TestClass pocObj,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(pocObj)}");
}
```

Al igual que con los objetos JSON, se producirá un error si el mensaje no tiene el formato correcto como objeto de C#. Si lo tiene, se enlaza a la variable pocObj, la cual se puede usar para lo que sea necesario.

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de desencadenador de RabbitMQ en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función Lee y registra el mensaje de RabbitMQ.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

Este es el código de script de C#:

```csx
using System;

public static void Run(string myQueueItem, ILogger log)
{
    log.LogInformation($"C# Script RabbitMQ trigger function processed: {myQueueItem}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de RabbitMQ en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función lee y registra un mensaje de RabbitMQ.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

Este es el código del script de JavaScript:

```javascript
module.exports = async function (context, myQueueItem) {
    context.log('JavaScript RabbitMQ trigger function processed work item', myQueueItem);
};
```

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra cómo leer un mensaje de cola de RabbitMQ a través de un desencadenador.

Un enlace de RabbitMQ se define en *function.json*, donde *type* se establece en `RabbitMQTrigger`.

```json
{
    "scriptFile": "__init__.py",
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "",
            "connectionStringSetting": ""
        }
    ]
}
```

El código de *_\_init_\_.py* declara un parámetro como `func.RabbitMQMessage`, que permite leer el mensaje en la función.

```python
import logging
import azure.functions as func

def main(myQueueItem) -> None:
    logging.info('Python RabbitMQ trigger function processed a queue item: %s', myQueueItem)
```

# <a name="java"></a>[Java](#tab/java)

La siguiente función de Java usa la anotación `@RabbitMQTrigger` de los [tipos de RabbitMQ para Java](https://mvnrepository.com/artifact/com.microsoft.azure.functions/azure-functions-java-library-rabbitmq) para describir la configuración de un desencadenador de cola RabbitMQ. La función toma el mensaje colocado en la cola y lo agrega a los registros.

```java
@FunctionName("RabbitMQTriggerExample")
public void run(
    @RabbitMQTrigger(connectionStringSetting = "rabbitMQConnection", queueName = "queue") String input,
    final ExecutionContext context)
{
    context.getLogger().info("Java HTTP trigger processed a request." + input);
}
```

---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [RabbitMQTrigger](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/src/Trigger/RabbitMQTriggerAttribute.cs).

A continuación, se muestra un atributo `RabbitMQTrigger` en una signatura de método:

```csharp
[FunctionName("RabbitMQTest")]
public static void RabbitMQTest([RabbitMQTrigger("queue")] string message, ILogger log)
{
    ...
}
```

Para un ejemplo completo, consulte el ejemplo de C#.

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="java"></a>[Java](#tab/java)

La anotación `RabbitMQTrigger` permite crear una función que se ejecuta al crear un mensaje de RabbitMQ. Las opciones de configuración disponibles incluyen el nombre de la cola y el nombre de la cadena de conexión. Para obtener más detalles sobre los parámetros, visite [Anotaciones Java RabbitMQTrigger](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/binding-library/java/src/main/java/com/microsoft/azure/functions/rabbitmq/annotation/RabbitMQTrigger.java).

Consulte el [ejemplo](#example) de desencadenador para más detalles.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `RabbitMQTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Debe establecerse en "RabbitMQTrigger".|
|**direction** | N/D | Debe establecerse en "in".|
|**name** | N/D | Nombre de la variable que representa la cola en el código de la función. |
|**queueName**|**QueueName**| Nombre de la cola desde la que se reciben los mensajes. |
|**hostName**|**HostName**|(opcional si se usa ConnectStringSetting) <br>Nombre de host de la cola (por ejemplo: 10.26.45.210)|
|**userNameSetting**|**UserNameSetting**|(opcional si se usa ConnectionStringSetting) <br>Nombre para acceder a la cola |
|**passwordSetting**|**PasswordSetting**|(opcional si se usa ConnectionStringSetting) <br>Contraseña para acceder la cola|
|**connectionStringSetting**|**ConnectionStringSetting**|El nombre de la configuración de la aplicación que contiene la cadena de conexión de la cola de mensajes RabbitMQ. Tenga en cuenta que si especifica la cadena de conexión directamente y no a través de una configuración de la aplicación en local.settings.json, el desencadenador no funcionará. (Por ejemplo: En *function.json*: connectionStringSetting: "rabbitMQConnection" <br> En *local.settings.json*: "rabbitMQConnection" : "< ActualConnectionstring >")|
|**port**|**Puerto**|Obtiene o establece el puerto usado. El valor predeterminado es 0.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Uso

# <a name="c"></a>[C#](#tab/csharp)

Los siguientes tipos de parámetro están disponibles para el mensaje:

* [Evento de RabbitMQ](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html): el formato predeterminado de los mensajes de RabbitMQ.
  * `byte[]`: a través de la propiedad "Body" del evento de RabbitMQ.
* `string`: el mensaje es texto.
* `An object serializable as JSON`: el mensaje se entrega como una cadena JSON válida.
* `POCO`: el mensaje tiene el formato de un objeto de C#. Para ver un ejemplo completo, consulte el [ejemplo](#example) de C#.

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

Los siguientes tipos de parámetro están disponibles para el mensaje:

* [Evento de RabbitMQ](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html): el formato predeterminado de los mensajes de RabbitMQ.
  * `byte[]`: a través de la propiedad "Body" del evento de RabbitMQ.
* `string`: el mensaje es texto.
* `An object serializable as JSON`: el mensaje se entrega como una cadena JSON válida.
* `POCO`: el mensaje tiene el formato de un objeto de C#.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

El mensaje de RabbitMQ se pasa a la función como una cadena o como un objeto JSON.

# <a name="python"></a>[Python](#tab/python)

El mensaje de RabbitMQ se pasa a la función como una cadena o como un objeto JSON.

# <a name="java"></a>[Java](#tab/java)

Consulte los [atributos y anotaciones](#attributes-and-annotations) de Java.

---

## <a name="dead-letter-queues"></a>Colas de mensajes fallidos
Las colas de mensajes fallidos y los intercambios no se pueden controlar ni configurar desde el desencadenador de RabbitMQ.  Para usar colas de mensajes fallidos, configure previamente la cola utilizada por el desencadenador en RabbitMQ. Consulte la [documentación de RabbitMQ](https://www.rabbitmq.com/dlx.html).

## <a name="hostjson-settings"></a>configuración de host.json

En esta sección se describen las opciones de configuración globales disponibles para este enlace en las versiones 2.x y posteriores. El siguiente archivo *host.json* de ejemplo contiene solo la configuración para este enlace. Para más información acerca de las opciones de configuración globales, consulte [Referencia de host.json para Azure Functions](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            "prefetchCount": 100,
            "queueName": "queue",
            "connectionString": "amqp://user:password@url:port",
            "port": 10
        }
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------|
|prefetchCount|30|Obtiene o establece el número de mensajes que el destinatario del mensaje puede solicitar simultáneamente y lo almacena en caché.|
|queueName|N/D| Nombre de la cola desde la que se reciben los mensajes. |
|connectionString|N/D|El nombre de la configuración de la aplicación que contiene la cadena de conexión de la cola de mensajes RabbitMQ. Tenga en cuenta que si especifica la cadena de conexión directamente y no a través de una configuración de la aplicación en local.settings.json, el desencadenador no funcionará.|
|port|0|Número máximo de sesiones que se puede administrar simultáneamente por instancia con escala.|

## <a name="local-testing"></a>Pruebas locales

> [!NOTE]
> connectionString tiene prioridad sobre "hostName", "userName" y "password". Si todos están establecidos, connectionString invalidará a los otros dos.

Si está realizando una prueba de forma local sin una cadena de conexión, debería configurar los valores "hostName", "username" y "password" si es aplicable en la sección "rabbitMQ" de *host.json*:

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            ...
            "hostName": "localhost",
            "username": "<your username>",
            "password": "<your password>"
        }
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------|
|hostName|N/D|(opcional si se usa ConnectStringSetting) <br>Nombre de host de la cola (por ejemplo: 10.26.45.210)|
|userName|N/D|(opcional si se usa ConnectionStringSetting) <br>Nombre para acceder a la cola |
|password|N/D|(opcional si se usa ConnectionStringSetting) <br>Contraseña para acceder la cola|

## <a name="monitoring-rabbitmq-endpoint"></a>Supervisión del punto de conexión de RabbitMQ
Para supervisar las colas e intercambios de un determinado punto de conexión de RabbitMQ:

* Habilite el [complemento de administración de RabbitMQ](https://www.rabbitmq.com/management.html)
* Vaya a http://{node-hostname}:15672 e inicie sesión con su nombre de usuario y contraseña.

## <a name="next-steps"></a>Pasos siguientes

- [Envío de mensajes de RabbitMQ desde Azure Functions (enlace de salida)](./functions-bindings-rabbitmq-output.md)
