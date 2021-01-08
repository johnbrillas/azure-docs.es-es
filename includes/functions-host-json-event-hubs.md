---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: f5101233f7995fb58fc530e613ba3235a55c783c
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97628755"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x y superiores

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            },
            "initialOffsetOptions": {
                "type": "fromStart",
                "enqueuedTime": ""
            }
        }
    }
}  
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------|
|batchCheckpointFrequency|1|Número de lotes de eventos que se va a procesar antes de crear un punto de comprobación de cursor de EventHub.|
|eventProcessorOptions/maxBatchSize|10|Número máximo de eventos recibido por cada bucle de recepción.|
|eventProcessorOptions/prefetchCount|300|Número predeterminado de capturas previas utilizado por el elemento `EventProcessorHost` subyacente. El valor mínimo permitido es 10.|
|initialOffsetOptions/type|fromStart|Ubicación en el flujo de eventos desde la que se inicia el procesamiento cuando no existe un punto de control en el almacenamiento. Las opciones son `fromStart`, `fromEnd` o `fromEnqueuedTime`. `fromEnd` procesa los nuevos eventos que se pusieron en cola después de iniciar la ejecución de la aplicación de funciones. Se aplica a todas las particiones.  Para más información, consulte la documentación de [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider?view=azure-dotnet).|
|initialOffsetOptions/enqueuedTime|N/D| Especifica la hora de puesta en cola del evento en la secuencia a partir de la que se va a iniciar el procesamiento. Cuando `initialOffsetOptions/type` se configura como `fromEnqueuedTime`, este valor es obligatorio. Admite la hora en cualquier formato admitido por [DateTime.Parse()](/dotnet/standard/base-types/parsing-datetime), como `2020-10-26T20:31Z`. Para mayor claridad, también debe especificar una zona horaria. Cuando no se especifica una zona horaria, Functions asume la zona horaria local del equipo que ejecuta la aplicación de funciones, que es la hora UTC cuando se ejecuta en Azure. Para más información, consulte la documentación de [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider?view=azure-dotnet).|
> [!NOTE]
> Para una referencia de host.json en Azure Functions 2.x y versiones posteriores, consulte [Referencia de host.json para Azure Functions](../articles/azure-functions/functions-host-json.md).

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------| 
|maxBatchSize|64|Número máximo de eventos recibido por cada bucle de recepción.|
|prefetchCount|N/D|Número predeterminado de capturas previas que utilizará el elemento `EventProcessorHost` subyacente.| 
|batchCheckpointFrequency|1|Número de lotes de eventos que se va a procesar antes de crear un punto de comprobación de cursor de EventHub.| 

> [!NOTE]
> Para una referencia de host.json en Azure Functions 1.x, consulte [Referencia de host.json para Azure Functions 1.x](../articles/azure-functions/functions-host-json-v1.md).
