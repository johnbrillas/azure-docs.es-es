---
title: 'Suborquestaciones en Durable Functions: Azure'
description: Aprenda a llamar orquestaciones desde otras orquestaciones en la extensión Durable Functions para Azure Functions.
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 5625bc2ddfa4b6f527ca16f19f33d257a1834d4b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85340815"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Suborquestaciones en Durable Functions (Azure Functions)

Además de llamar a funciones de actividad, las funciones de orquestador pueden llamar a otras funciones de orquestador. Por ejemplo, puede crear una orquestación mayor a partir de una biblioteca de funciones de orquestador más pequeñas. También puede ejecutar varias instancias de una función de orquestador en paralelo.

Una función de orquestador puede llamar a otra función de orquestador con los métodos `CallSubOrchestratorAsync` o `CallSubOrchestratorWithRetryAsync` en .NET o los métodos `callSubOrchestrator` o `callSubOrchestratorWithRetry` en JavaScript. En el artículo [Control de errores y compensación](durable-functions-error-handling.md#automatic-retry-on-failure) se ofrece más información sobre los reintentos automáticos.

Las funciones de suborquestador se comportan como funciones de actividad desde la perspectiva del llamador. Pueden devolver un valor, producir una excepción y ser esperadas por la función de orquestador primaria. 

> [!NOTE]
> Actualmente, las suborquestaciones se admiten en .NET y JavaScript.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra un escenario de IoT ("Internet de las cosas") donde hay varios dispositivos que deben aprovisionarse. La siguiente función representa el flujo de trabajo de aprovisionamiento que se hay que ejecutar para cada dispositivo:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

---

Esta función de orquestador puede utilizarse tal cual para un aprovisionamiento de dispositivos único o puede ser parte de una orquestación mayor. En el último caso, la función de orquestador primaria puede programar instancias de `DeviceProvisioningOrchestration` con la API `CallSubOrchestratorAsync` (.NET) o `callSubOrchestrator` (JavaScript).

El siguiente es un ejemplo que muestra cómo ejecutar varias funciones de orquestador en paralelo.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

> [!NOTE]
> Los ejemplos de C# anteriores corresponden a Durable Functions 2.x. En el caso de Durable Functions 1.x, debe usar `DurableOrchestrationContext` en lugar de `IDurableOrchestrationContext`. Para obtener más información sobre las diferencias entre versiones, vea el artículo [Versiones de Durable Functions](durable-functions-versions.md).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    var id = 0;
    for (const deviceId of deviceIds) {
        const child_id = context.df.instanceId+`:${id}`;
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId, child_id);
        provisioningTasks.push(provisionTask);
        id++;
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

---

> [!NOTE]
> Las suborquestaciones se deben definir en la misma aplicación de funciones que la orquestación primaria. Si necesita llamar a y esperar a las orquestaciones en otra aplicación de funciones, considere la posibilidad de usar la compatibilidad integrada para las API HTTP y el patrón de consumidor de sondeo HTTP 202. Para más información, consulte el tema [Características HTTP](durable-functions-http-features.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener información acerca de cómo establecer el estado de una orquestación personalizada](durable-functions-custom-orchestration-status.md)
