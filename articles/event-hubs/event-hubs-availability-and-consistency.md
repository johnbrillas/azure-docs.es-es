---
title: 'Disponibilidad y coherencia: Azure Event Hubs | Microsoft Docs'
description: Cómo proporcionar el máximo nivel de disponibilidad y coherencia con Azure Event Hubs mediante el uso de particiones.
ms.topic: article
ms.date: 01/25/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 2fdb62e953230a38a26d22e136789fea52c8ee8c
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882202"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Disponibilidad y coherencia en Event Hubs
En este artículo se proporciona información sobre la disponibilidad y la coherencia que admite Azure Event Hubs. 

## <a name="availability"></a>Disponibilidad
Azure Event Hubs extiende el riesgo de errores catastróficos de máquinas individuales o incluso de bastidores completos en clústeres que abarcan varios dominios de error dentro de un centro de recursos. Implementa la detección transparente de errores y los mecanismos de conmutación por error para que el servicio siga funcionando dentro de los niveles de servicio garantizados y, normalmente, sin interrupciones perceptibles cuando se producen estos errores. Si se ha creado un espacio de nombres de Event Hubs con la opción habilitada para [zonas de disponibilidad](../availability-zones/az-overview.md), el riesgo de interrupción se reparte entre tres instalaciones separadas físicamente, y el servicio tiene suficientes reservas de capacidad para hacer frente de inmediato a la pérdida completa y catastrófica de toda la instalación. Para más información, consulte [Azure Event Hubs: recuperación ante desastres geográfica](event-hubs-geo-dr.md).

Cuando una aplicación cliente envía eventos a un centro de eventos, los eventos se distribuyen automáticamente entre las particiones del centro de eventos. Si una partición no está disponible por algún motivo, los eventos se distribuyen entre las particiones restantes. Este comportamiento permite disfrutar del máximo tiempo de actividad. En los casos de uso que requieren el tiempo de actividad máximo, se prefiere este modelo en lugar de enviar eventos a una partición específica. Para más información consulte [Particiones](event-hubs-scalability.md#partitions).

## <a name="consistency"></a>Coherencia
En algunos escenarios, el orden de los eventos puede ser importante. Por ejemplo, puede que prefiera el sistema back-end para procesar un comando de actualización antes que un comando de eliminación. En este escenario, una aplicación cliente envía eventos a una partición específica para que se conserve el orden. Cuando una aplicación de consumidor consume estos eventos de la partición, se leen por orden. 

Con esta configuración, tenga en cuenta que si la partición concreta a la que se realiza el envío no se encuentra disponible, recibirá una respuesta de error. Como punto de comparación, si no tiene una afinidad para una sola partición, el servicio Event Hubs envía el evento a la siguiente partición disponible.

Una posible solución para garantizar el orden, mientras también se maximiza el tiempo de actividad, sería agregar eventos como parte de la aplicación de procesamiento de eventos. La manera más fácil de lograrlo es marcar el evento con una propiedad de número de secuencia personalizada.

En este escenario, el cliente productor envía el evento a una de las particiones disponibles en el centro de eventos y establece el número de secuencia correspondiente a partir de la aplicación. Esta solución requiere que la aplicación de procesamiento conserve el estado, pero proporciona a los remitentes un punto de conexión con más probabilidades de estar disponible.

## <a name="appendix"></a>Apéndice

### <a name="net-examples"></a>Ejemplos de .NET

#### <a name="send-events-to-a-specific-partition"></a>Envío de eventos a una partición específica
Establezca la clave de partición en un evento o use un objeto `PartitionSender` (si usa la biblioteca antigua Microsoft.Azure.Messaging) para enviar eventos solo a una determinada partición. De esta forma, se garantiza que, cuando se lean eventos de la partición, la lectura siga un orden. 

Si usa la biblioteca **Azure.Messaging.EventHubs** más reciente, consulte [Migración de código de PartitionSender a EventHubProducerClient para publicar eventos en una partición](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition).

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 o posterior)](#tab/latest)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    var batchOptions = new CreateBatchOptions() { PartitionId = "my-partition-id" };
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync(batchOptions);
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));
    
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 o anterior)](#tab/old)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(connectionString){ EntityPath = eventHubName }; 
var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
PartitionSender partitionSender = eventHubClient.CreatePartitionSender("my-partition-id");
try
{
    EventDataBatch eventBatch = partitionSender.CreateBatch();
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));

    await partitionSender.SendAsync(eventBatch);
}
finally
{
    await partitionSender.CloseAsync();
    await eventHubClient.CloseAsync();
}
```

---

### <a name="set-a-sequence-number"></a>Establecimiento de un número de secuencia
En el ejemplo siguiente se sella el evento con una propiedad de número de secuencia personalizada. 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 o posterior)](#tab/latest)

```csharp
// create a producer client that you can use to send events to an event hub
await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    // get the latest sequence number from your application
    var sequenceNumber = GetNextSequenceNumber();

    // create a batch of events 
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

    // create a new EventData object by encoding a string as a byte array
    var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

    // set a custom sequence number property
    data.Properties.Add("SequenceNumber", sequenceNumber);

    // add events to the batch. An event is a represented by a collection of bytes and metadata. 
    eventBatch.TryAdd(data);

    // use the producer client to send the batch of events to the event hub
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 o anterior)](#tab/old)
```csharp
// Create an Event Hubs client
var client = new EventHubClient(connectionString, eventHubName);

//Create a producer to produce events
EventHubProducer producer = client.CreateProducer();

// Get the latest sequence number from your application 
var sequenceNumber = GetNextSequenceNumber();

// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);

// Send single message async
await producer.SendAsync(data);
```
---

Este ejemplo envía el evento a una de las particiones disponibles en el centro de eventos y establece el número de secuencia correspondiente a partir de la aplicación. Esta solución requiere que la aplicación de procesamiento conserve el estado, pero proporciona a los remitentes un punto de conexión con más probabilidades de estar disponible.

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de Event Hubs, visite los vínculos siguientes:

* [Información general sobre el servicio Event Hubs](./event-hubs-about.md)
* [Creación de un centro de eventos](event-hubs-create.md)
