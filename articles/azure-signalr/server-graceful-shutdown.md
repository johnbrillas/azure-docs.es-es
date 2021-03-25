---
title: Detenga correctamente el servidor de aplicaciones.
description: En este artículo se proporciona información sobre cómo detener correctamente el servidor de aplicaciones de SignalR.
author: terencefan
ms.author: tefa
ms.date: 11/12/2020
ms.service: signalr
ms.topic: conceptual
ms.openlocfilehash: d9dd7ce9cf321628598a7bb866c5d1b1a6fb0e1e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98201678"
---
# <a name="server-graceful-shutdown"></a>Apagado correcto del servidor
Microsoft Azure SignalR Service ofrece dos modos de apagar correctamente los servidores. 

La principal ventaja de usar esta característica es evitarle caídas de conexión inesperadas al cliente. 

En su lugar, puede esperar a que las conexiones del cliente se cierren según la lógica de negocios o migrar la conexión de cliente a otro servidor sin perder datos. 

## <a name="how-it-works"></a>Funcionamiento

En general, el proceso de apagado correcto consta de cuatro fases:

1. **Establezca el servidor sin conexión**.

    Significa que no se enrutarán más conexiones de cliente a este servidor.

2. **Desencadene los enlaces de `OnShutdown`** .

    Podría registrar enlaces de apagado de cada centro de conectividad que posea en el servidor.
    Se les llamará según el orden de registro justo después de que se haya producido una respuesta **FINACK** en nuestra instancia de Azure SignalR Service, lo que significa que este servidor se ha establecido sin conexión en ella.

    Puede difundir mensajes o realizar trabajos de limpieza en esta fase; una vez ejecutados todos los enlaces de apagado, pasaremos a la siguiente fase.

3. **Espere hasta que finalicen todas las conexiones de cliente**. Según el modo que elija:

    **Modo establecido en WaitForClientsToClose**

    Azure SignalR Service mantendrá los clientes existentes.

    Quizá deba diseñar una manera de dejar que los clientes decidan cuándo cerrar la conexión o volver a conectarse, por ejemplo, enviarles un mensaje de cierre a todos.

    Lea [ChatSample](https://github.com/Azure/azure-signalr/tree/dev/samples/ChatSample) para un ejemplo de uso donde se difunde un mensaje "de salida" para desencadenar el cierre del cliente en el enlace de apagado.

    **Modo establecido en MigrateClients**

    Azure SignalR Service intentará volver a enrutar la conexión de cliente de este servidor a otro servidor válido. 
    
    En este escenario, `OnConnectedAsync` y `OnDisconnectedAsync` se desencadenarán en el nuevo servidor y en el antiguo, respectivamente, con `IConnectionMigrationFeature` establecido en `HttpContext`, que se puede usar para identificar si la conexión de cliente se ha migrado en cualquiera de los sentidos. Podría ser útil especialmente para los escenarios con estado.

    La conexión del cliente se migrará inmediatamente después de que se haya entregado el mensaje actual, lo que significa que el siguiente mensaje se enrutará al nuevo servidor.

4. **Detenga las conexiones con el servidor**.

    Una vez cerradas o migradas todas las conexiones de cliente, o si se supera el tiempo de espera (30 s de manera predeterminada),

    el SDK del servidor de SignalR continuará el apagado con esta fase y cerrará todas las conexiones del servidor.

    Se anularán las conexiones de cliente aunque no se realice el cierre o la migración. Por ejemplo, no hay un servidor de destino adecuado o no ha finalizado el mensaje de cliente a servidor actual.

## <a name="sample-codes"></a>Códigos de ejemplo

Agregue las siguientes opciones cuando `AddAzureSignalR`:

```csharp
services.AddSignalR().AddAzureSignalR(option =>
{
    option.GracefulShutdown.Mode = GracefulShutdownMode.WaitForClientsClose;
    // option.GracefulShutdown.Mode = GracefulShutdownMode.MigrateClients;
    option.GracefulShutdown.Timeout = TimeSpan.FromSeconds(30);

    option.GracefulShutdown.Add<Chat>(async (c) =>
    {
        await c.Clients.All.SendAsync("exit");
    });
});
```

### <a name="configure-onconnected-and-ondisconnected-while-setting-graceful-shutdown-mode-to-migrateclients"></a>configure `OnConnected` y `OnDisconnected` al establecer el modo de apagado correcto en `MigrateClients`.

Hemos introducido una característica "IConnectionMigrationFeature" para indicar si se ha migrado una conexión en cualquier dirección.

```csharp
public class Chat : Hub {

    public override async Task OnConnectedAsync()
    {
        Console.WriteLine($"{Context.ConnectionId} connected.");

        var feature = Context.GetHttpContext().Features.Get<IConnectionMigrationFeature>();
        if (feature != null)
        {
            Console.WriteLine($"[{feature.MigrateTo}] {Context.ConnectionId} is migrated from {feature.MigrateFrom}.");
            // Your business logic.
        }

        await base.OnConnectedAsync();
    }

    public override async Task OnDisconnectedAsync(Exception e)
    {
        Console.WriteLine($"{Context.ConnectionId} disconnected.");

        var feature = Context.GetHttpContext().Features.Get<IConnectionMigrationFeature>();
        if (feature != null)
        {
            Console.WriteLine($"[{feature.MigrateFrom}] {Context.ConnectionId} will be migrated to {feature.MigrateTo}.");
            // Your business logic.
        }

        await base.OnDisconnectedAsync(e);
    }
}
```