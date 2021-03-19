---
title: Guía de procesos aislados de .NET para .NET 5.0 en Azure Functions
description: Aprenda a usar un proceso aislado de .NET para ejecutar las funciones de C# en .NET 5.0 fuera de proceso en Azure.
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/01/2021
ms.custom: template-concept
ms.openlocfilehash: dacf3436ce98d839ad5b45361f1573c98c62d3e7
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563649"
---
# <a name="guide-for-running-functions-on-net-50-in-azure"></a>Guía para la ejecución de funciones en .NET 5.0 en Azure

Este artículo es una introducción al uso de C# para desarrollar funciones de proceso aislado de .NET, que se ejecutan fuera de proceso en Azure Functions. La ejecución fuera de proceso le permite desacoplar el código de función de tiempo de ejecución de Azure Functions. También proporciona una manera de crear y ejecutar funciones que tienen como destino la versión actual de .NET 5.0. 

| Introducción | Conceptos| Ejemplos |
|--|--|--| 
| <ul><li>[Uso de Visual Studio Code](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vscode)</li><li>[Uso de herramientas de línea de comandos](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-cli)</li><li>[Uso de Visual Studio](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vs)</li></ul> | <ul><li>[Opciones de hospedaje](functions-scale.md)</li><li>[Supervisión](functions-monitoring.md)</li> | <ul><li>[Ejemplos de referencia](https://github.com/Azure/azure-functions-dotnet-worker/tree/main/samples)</li></ul> |

Si no necesita admitir .NET 5.0 o ejecutar las funciones fuera de proceso, es posible que en su lugar quiera [desarrollar funciones de la biblioteca de clases C#](functions-dotnet-class-library.md).

## <a name="why-net-isolated-process"></a>¿Por qué proceso aislado de .NET?

Anteriormente, Azure Functions solo admitía un modo totalmente integrado para las funciones de .NET, que se ejecutan [como una biblioteca de clases](functions-dotnet-class-library.md) en el mismo proceso que el host. Este modo proporciona una integración profunda entre el proceso de host y las funciones. Por ejemplo, las funciones de la biblioteca de clases .NET pueden compartir tipos y API de enlace. Sin embargo, esta integración también requiere un acoplamiento más estrecho entre el proceso de host y la función de .NET. Por ejemplo, las funciones de .NET que se ejecutan en proceso deben ejecutarse en la misma versión de .NET que el entorno de ejecución de Functions. Para que pueda ejecutarse fuera de estas restricciones, ahora puede optar por realizar la ejecución en un proceso aislado. Este aislamiento de procesos también permite desarrollar funciones que usan versiones actuales de .NET (como .NET 5.0), no admitidas de forma nativa por el entorno de ejecución de Functions.

Dado que estas funciones se ejecutan en un proceso independiente, existen algunas [diferencias entre las características y las funciones](#differences-with-net-class-library-functions) de las aplicaciones de funciones aisladas de .NET y las de la biblioteca de clases .NET.

### <a name="benefits-of-running-out-of-process"></a>Ventajas de la ejecución fuera de proceso

Al ejecutarse fuera de proceso, las funciones de .NET pueden aprovechar las ventajas siguientes: 

+ Menos conflictos: dado que las funciones se ejecutan en un proceso independiente, los ensamblados usados en la aplicación no entrarán en conflicto con la versión diferente de los mismos ensamblados que usa el proceso de host.  
+ Control total del proceso: el usuario controla el inicio de la aplicación y puede controlar las configuraciones usadas y el middleware iniciado.
+ Inserción de dependencias: dado que tiene control total del proceso, puede usar los comportamientos actuales de .NET para la inserción de dependencias e incorporar middleware a la aplicación de funciones. 

## <a name="supported-versions"></a>Versiones compatibles

La única versión de .NET que se admite actualmente para ejecutarse fuera de proceso es .NET 5.0.

## <a name="net-isolated-project"></a>Proyecto aislado de .NET

Un proyecto de función aislada de .NET es básicamente un proyecto de aplicación de consola de .NET que tiene como destino .NET 5.0. A continuación, se muestran los archivos básicos necesarios en cualquier proyecto aislado de .NET:

+ Archivo [host.json](functions-host-json.md).
+ Archivo [local.settings.json](functions-run-local.md#local-settings-file).
+ Archivo del proyecto de C# (. csproj) que define el proyecto y las dependencias.
+ Archivo program.cs que es el punto de entrada de la aplicación.

## <a name="package-references"></a>Referencias de paquete

Al ejecutarse fuera de proceso, el proyecto de .NET usa un conjunto único de paquetes, que implementan la funcionalidad básica y las extensiones de enlace. 

### <a name="core-packages"></a>Paquetes base 

Los siguientes paquetes son necesarios para ejecutar las funciones de .NET en un proceso aislado:

+ [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)
+ [Microsoft.Azure.Functions.Worker.Sdk](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk/)

### <a name="extension-packages"></a>Paquetes de extensión

Dado que las funciones que se ejecutan en un proceso aislado de .NET usan tipos de enlace diferentes, requieren un conjunto único de paquetes de extensión de enlace. 

Encontrará estos paquetes de extensión en [Microsoft.Azure.Functions.Worker.Extensions](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions).

## <a name="start-up-and-configuration"></a>Inicio y configuración 

Cuando se usan funciones aisladas de .NET, se tiene acceso al inicio de la aplicación de funciones, que normalmente se encuentra en Program.cs. El usuario es responsable de crear e iniciar su propia instancia de host. Como tal, también tiene acceso directo a la canalización de configuración de la aplicación. Puede insertar dependencias con mayor facilidad y ejecutar middleware cuando se ejecuta fuera de proceso. 

El código siguiente muestra un ejemplo de una canalización de `HostBuilder`:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_startup":::

`HostBuilder` se usa para compilar y devolver una instancia de `IHost` totalmente inicializada, que se ejecuta de forma asincrónica para iniciar la aplicación de funciones. 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_host_run":::

### <a name="configuration"></a>Configuración

El acceso a la canalización del generador de host significa que puede establecer cualquier configuración específica de la aplicación durante la inicialización. Estas configuraciones se aplican a la aplicación de funciones que se ejecuta en un proceso independiente. Para realizar cambios en el host de funciones o en la configuración del enlace y el desencadenador, todavía necesitará usar el [archivo host.json](functions-host-json.md).      

En el ejemplo siguiente se muestra cómo agregar la configuración `args`, que se lee como argumentos de la línea de comandos: 
 
:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_configure_app" :::

El método `ConfigureAppConfiguration` se usa para configurar el resto del proceso de compilación y la aplicación. En este ejemplo también se usa un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder?view=dotnet-plat-ext-5.0&preserve-view=true), que facilita la adición de varios elementos de configuración. Dado que `ConfigureAppConfiguration` devuelve la misma instancia de [`IConfiguration`](/dotnet/api/microsoft.extensions.configuration.iconfiguration?view=dotnet-plat-ext-5.0&preserve-view=true), también puede llamarlo varias veces para agregar varios elementos de configuración. Puede tener acceso al conjunto completo de configuraciones desde [`HostBuilderContext.Configuration`](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration?view=dotnet-plat-ext-5.0&preserve-view=true) y [`IHost.Services`](/dotnet/api/microsoft.extensions.hosting.ihost.services?view=dotnet-plat-ext-5.0&preserve-view=true).

Para más información, consulte [Configuración en ASP.NET Core](/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0&preserve-view=true). 

### <a name="dependency-injection"></a>Inserción de dependencias

La inserción de dependencias se simplifica, en comparación con las bibliotecas de clases .NET. En lugar de tener que crear una clase de inicio para registrar los servicios, solo tiene que llamar a `ConfigureServices` en el generador de host y usar los métodos de extensión en [`IServiceCollection`](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection?view=dotnet-plat-ext-5.0&preserve-view=true) para insertar servicios específicos. 

En el ejemplo siguiente se inserta una dependencia del servicio singleton:  
 
:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_dependency_injection" :::

Para obtener más información, consulte [Inserción de dependencias en ASP.NET Core](/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0&preserve-view=true).

<!--### Middleware

.NET isolated also supports middleware registration, again by using a model similar to what exists in ASP.NET. This model gives you the ability to inject logic into the invocation pipeline, and before and after functions execute.

While the full middleware registration set of APIs is not yet exposed, we do support middleware registration and have added an example to the sample application under the Middleware folder.

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_middleware" :::-->

## <a name="execution-context"></a>Contexto de ejecución

.NET aislado pasa un objeto `FunctionContext` en los métodos de función. Este objeto permite obtener una instancia de [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) para escribir en los registros llamando al método `GetLogger` y proporcionando una cadena `categoryName`. Para más información, consulte [Registro](#logging). 

## <a name="bindings"></a>Enlaces 

Los enlaces se definen mediante atributos en métodos, parámetros y tipos de valor devuelto. Un método de función es un método con una `Function` y un atributo de desencadenador aplicados a un parámetro de entrada, tal como se muestra en el ejemplo siguiente:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Queue/QueueFunction.cs" id="docsnippet_queue_trigger" :::

El atributo desencadenador especifica el tipo de desencadenador y enlaza los datos de entrada a un parámetro del método. La función de ejemplo anterior se desencadena mediante un mensaje de cola, y este último se pasa al método en el parámetro `myQueueItem`.

El atributo `Function` marca el método como punto de entrada de una función. El nombre debe ser único dentro de un proyecto, debe empezar por una letra y solo puede incluir letras, números, `_` y `-`, y hasta 127 caracteres. Las plantillas de proyecto suelen crear un método denominado `Run`, pero el nombre de método puede ser cualquier nombre de método de C# válido.

Dado que los proyectos aislados de .NET se ejecutan en un proceso de trabajo independiente, los enlaces no pueden aprovechar las clases de enlace enriquecidas, como `ICollector<T>`, `IAsyncCollector<T>` y `CloudBlockBlob`. Tampoco hay compatibilidad directa con los tipos heredados de los SDK de servicio subyacentes, como [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient) y [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). En su lugar, los enlaces se basan en cadenas, matrices y tipos serializables, como los objetos de clase anterior sin formato (POCO). 

En el caso de los desencadenadores HTTP, debe usar `HttpRequestData` y `HttpResponseData` para tener acceso a los datos de solicitud y respuesta. Esto se debe a que no tiene acceso a los objetos de solicitud y respuesta HTTP originales cuando se ejecuta fuera de proceso. 

### <a name="input-bindings"></a>Enlaces de entrada

Una función puede tener cero o más enlaces de entrada que pueden pasar datos a una función. Al igual que los desencadenadores, los enlaces de entrada se definen mediante la aplicación de un atributo de enlace a un parámetro de entrada. Cuando se ejecuta la función, el entorno de ejecución intenta obtener los datos especificados en el enlace. Los datos que se solicitan suelen depender de la información proporcionada por el desencadenador mediante parámetros de enlace.  

### <a name="output-bindings"></a>Enlaces de salida

Para escribir en un enlace de salida, debe aplicar un atributo de enlace de salida al método de función, que define cómo escribir en el servicio enlazado. El valor devuelto por el método se escribe en el enlace de salida. Por ejemplo, en el ejemplo siguiente se escribe un valor de cadena en una cola de mensajes denominada `functiontesting2` mediante un enlace de salida:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Queue/QueueFunction.cs" id="docsnippet_queue_output_binding" :::

### <a name="multiple-output-bindings"></a>Varios enlaces de salida

Los datos que se escriben en un enlace de salida siempre son el valor devuelto de la función. Si tiene que escribir en más de un enlace de salida, debe crear un tipo de valor devuelto personalizado. Este tipo de valor devuelto debe tener el atributo de enlace de salida aplicado a una o más propiedades de la clase. En el ejemplo siguiente se escribe en una respuesta HTTP y un enlace de salida de cola:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Function1/Function1.cs" id="docsnippet_multiple_outputs":::

### <a name="http-trigger"></a>Desencadenador HTTP

Los desencadenadores HTTP traducen el mensaje de solicitud HTTP entrante en un objeto `HttpRequestData` que se pasa a la función. Este objeto proporciona datos de la solicitud, incluidos `Headers`, `Cookies`, `Identities`, `URL` y un mensaje opcional `Body`. Este objeto es una representación del objeto de solicitud HTTP y no la propia solicitud. 

Del mismo modo, la función devuelve un objeto `HttpReponseData`, que proporciona los datos usados para crear la respuesta HTTP, incluido el mensaje `StatusCode`, `Headers` y, opcionalmente, un mensaje `Body`.  

El código siguiente es un desencadenador HTTP. 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Http/HttpFunction.cs" id="docsnippet_http_trigger" :::

## <a name="logging"></a>Registro

En .NET aislado, puede escribir en los registros mediante una instancia de [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) obtenida de un objeto `FunctionContext` pasado a la función. Llame al método `GetLogger`, pasando un valor de cadena que es el nombre de la categoría en la que se escriben los registros. La categoría suele ser el nombre de la función específica desde la que se escriben los registros. Para obtener más información sobre las categorías, consulte el [artículo sobre supervisión](functions-monitoring.md#log-levels-and-categories). 

En el ejemplo siguiente se muestra cómo obtener `ILogger` y escribir registros dentro de una función:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Http/HttpFunction.cs" id="docsnippet_logging" ::: 

Use diferentes métodos de `ILogger` para escribir varios niveles de registro, como `LogWarning` o `LogError`. Para obtener más información sobre los niveles de registro, consulte el [artículo sobre supervisión](functions-monitoring.md#log-levels-and-categories).

También se proporciona [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) al usar la [inserción de dependencias](#dependency-injection).

## <a name="differences-with-net-class-library-functions"></a>Diferencias con las funciones de la biblioteca de clases .NET

En esta sección se describe el estado actual de las diferencias funcionales y de comportamiento que se ejecutan en .NET 5.0 fuera de proceso en comparación con las funciones de la biblioteca de clases .NET que se ejecutan en proceso:

| Característica/comportamiento |  En proceso (.NET Core 3.1) | Fuera de proceso (.NET 5.0) |
| ---- | ---- | ---- |
| Versiones de .NET | LTS (.NET Core 3.1) | Actual (.NET 5.0) |
| Paquetes base | [Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) | [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)<br/>[Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk) | 
| Paquetes de extensión de enlace | [`Microsoft.Azure.WebJobs.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.WebJobs.Extensions)  | En [`Microsoft.Azure.Functions.Worker.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions) | 
| Registro | [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) se pasa a la función | [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) se obtiene de `FunctionContext` |
| Tokens de cancelación | [Compatible](functions-dotnet-class-library.md#cancellation-tokens) | No compatible |
| Enlaces de salida | Parámetros de salida | Valores devueltos |
| Tipos de enlaces de salida |  `IAsyncCollector`, [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet&preserve-view=true), [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet&preserve-view=true) y otros tipos específicos del cliente | Tipos simples, tipos serializables de JSON y matrices. |
| Varios enlaces de salida | Compatible | [Compatible](#multiple-output-bindings) |
| Desencadenador HTTP | [`HttpRequest`](/dotnet/api/microsoft.aspnetcore.http.httprequest?view=aspnetcore-5.0&preserve-view=true)/[`ObjectResult`](/dotnet/api/microsoft.aspnetcore.mvc.objectresult?view=aspnetcore-5.0&preserve-view=true) | `HttpRequestData`/`HttpResponseData` |
| Funciones duraderas | [Compatible](durable/durable-functions-overview.md) | No compatible | 
| Enlaces imperativos | [Compatible](functions-dotnet-class-library.md#binding-at-runtime) | No compatible |
| Artefacto function.json | Generado | No se han generado |
| Configuración | [host.json](functions-host-json.md) | [host.js](functions-host-json.md) e [inicialización personalizada](#configuration) |
| Inserción de dependencias | [Compatible](functions-dotnet-dependency-injection.md)  | [Compatible](#dependency-injection) |
| Software intermedio | No compatible | Compatible |
| Tiempo de arranque en frío | Habitual | Más tiempo, debido al inicio Just-in-Time. Ejecute en Linux en lugar de en Windows para reducir los posibles retrasos. |
| ReadyToRun | [Compatible](functions-dotnet-class-library.md#readytorun) | _TBD_ |

## <a name="known-issues"></a>Problemas conocidos

Para obtener información sobre soluciones alternativas a la ejecución de funciones de proceso aislado de .NET, vea [esta página de problemas conocidos](https://aka.ms/AAbh18e). Para notificar problemas, [cree un problema en este repositorio de GitHub](https://github.com/Azure/azure-functions-dotnet-worker/issues/new/choose).  

## <a name="next-steps"></a>Pasos siguientes

+ [Más información sobre los desencadenadores y los enlaces](functions-triggers-bindings.md)
+ [Más información sobre procedimientos recomendados para Azure Functions](functions-best-practices.md)
