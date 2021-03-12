---
title: 'Persistencia y serialización de datos en Durable Functions: Azure'
description: Aprenda de qué manera la extensión Durable Functions para Azure Functions permite la persistencia de datos.
author: ConnorMcMahon
ms.topic: conceptual
ms.date: 02/11/2021
ms.author: azfuncdf
ms.openlocfilehash: ea4aaa1cdbe10e2db9cf619452558d104a2293ab
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449380"
---
# <a name="data-persistence-and-serialization-in-durable-functions-azure-functions"></a>Persistencia y serialización de datos en Durable Functions (Azure Functions)

Durable Functions persiste automáticamente los parámetros de una función, los valores devueltos y otros estados a un back-end duradero para proporcionar una ejecución de confianza. Sin embargo, la cantidad y la frecuencia de los datos que persisten en el almacenamiento duradero pueden afectar al rendimiento de las aplicaciones y a los costos de las transacciones de almacenamiento. En función del tipo de datos que almacene una aplicación, es posible que también sea necesario tener en cuenta la retención de datos y las directivas de privacidad.

## <a name="azure-storage"></a>Azure Storage

De manera predeterminada, Durable Functions persiste los datos en colas, tablas y blobs de una cuenta de [Azure Storage](https://azure.microsoft.com/services/storage/) que especifique.

### <a name="queues"></a>Colas

Durable Functions usa colas de Azure Storage para programar todas las ejecuciones de una función de manera confiable. Estos mensajes de cola contienen entradas o salidas de función, según si el mensaje se usa para programar una ejecución o devolver un valor a una función de llamada. Estos mensajes de cola también incluyen metadatos adicionales que Durable Functions usa con fines internos, como el enrutamiento y la correlación de un extremo a otro. Una vez finalizada la ejecución de una función en respuesta a un mensaje recibido, se elimina ese mensaje y el resultado de la ejecución también puede persistir en tablas de Azure Storage o en blobs de Azure Storage.

Dentro de un único [centro de tareas](durable-functions-task-hubs.md), Durable Functions crea y agrega mensajes a una cola de *elementos de trabajo* denominada `<taskhub>-workitem` para las funciones de actividad de programación, y a una o varias *colas de control* denominadas `<taskhub>-control-##` para programar o reanudar las funciones de orquestador y entidad. El número de colas de control es igual al número de particiones configuradas para la aplicación. Para obtener más información sobre las colas y las particiones, consulte la [documentación de rendimiento y escalabilidad](durable-functions-perf-and-scale.md).

### <a name="tables"></a>Tablas

Una vez que las orquestaciones procesan los mensajes correctamente, los registros de sus acciones resultantes se conservan en la tabla del *Historial* denominada `<taskhub>History`. Las entradas y salidas de orquestación, y los datos de estado personalizados también se conservan en la tabla de *Instancias* denominada `<taskhub>Instances`.

### <a name="blobs"></a>Datos BLOB

En la mayoría de los casos, Durable Functions no usa blobs de Azure Storage para conservar los datos. Sin embargo, las colas y las tablas tienen [límites de tamaño](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-queue-storage-limits) que pueden impedir que Durable Functions persista todos los datos necesarios en una fila de almacenamiento o en un mensaje de cola. Por ejemplo, cuando un dato que es necesario conservar en una cola sea superior a 45 KB cuando se serializa, Durable Functions comprimirá los datos y los almacenará en un blob en su lugar. Cuando los datos persisten en el almacenamiento de blobs de esta manera, Durable Functions almacena una referencia a ese BLOB en la fila de la tabla o en el mensaje de la cola. Cuando Durable Functions tenga que recuperar los datos, los recuperará automáticamente del blob. Estos blobs se almacenan en el contenedor de blobs `<taskhub>-largemessages`.

> [!NOTE]
> Los pasos adicionales de compresión y de operación de blobs para los mensajes de gran tamaño pueden ser caros en relación con los costos de CPU y de latencia de E/S. Además, Durable Functions tiene que cargar los datos persistentes en la memoria y puede hacerlo para muchas ejecuciones de función diferentes al mismo tiempo. Como consecuencia, la persistencia de las cargas de datos de gran tamaño también puede provocar un uso elevado de la memoria. Para minimizar la sobrecarga de memoria, considere la posibilidad de conservar manualmente las cargas de datos de gran tamaño (por ejemplo, en almacenamiento de blobs) y, en su lugar, pasar las referencias a estos datos. De este modo, el código puede cargar los datos solo cuando sea necesario para evitar cargas redundantes durante la [reproducción de la función de orquestador](durable-functions-orchestrations.md#reliability). Sin embargo, *no* se recomienda almacenar cargas en el disco, ya que no se garantiza que el estado en disco esté disponible, dado que las funciones pueden ejecutarse en VM diferentes a lo largo de su duración.

### <a name="types-of-data-that-is-serialized-and-persisted"></a>Tipos de datos que se serializan y persisten
A continuación se muestra una lista de los diferentes tipos de datos que se serializarán y persistirán al usar las características de Durable Functions:

- Todas las entradas y salidas de las funciones de orquestador, actividad y entidad, incluidos los identificadores y las excepciones no controladas
- Nombres de las funciones de orquestador, actividad y entidad
- Nombres y cargas de eventos externos
- Cargas de estado de la orquestación personalizada
- Mensajes de finalización de orquestación
- Cargas de temporizador duraderas
- Direcciones URL, encabezados y cargas de solicitud y respuesta HTTP duraderas
- Cargas de llamadas y señales de entidades
- Cargas de estado de entidades

### <a name="working-with-sensitive-data"></a>Trabajo con datos confidenciales
Al usar Azure Storage, todos los datos se cifran automáticamente en reposo. Sin embargo, cualquier usuario con acceso a la cuenta de almacenamiento puede leer los datos en su formato no cifrado. Si necesita una protección más segura para datos confidenciales, considere la posibilidad de cifrar primero los datos con sus propias claves de cifrado para que Durable Functions conserve los datos en un formato previamente cifrado.

Como alternativa, los usuarios de .NET tienen la opción de implementar proveedores de serialización personalizados que proporcionan cifrado automático. Puede encontrar un ejemplo de serialización personalizada con cifrado en [este ejemplo de GitHub](https://github.com/charleszipp/azure-durable-entities-encryption).

> [!NOTE]
> Si decide implementar el cifrado a nivel de aplicación, tenga en cuenta que las orquestaciones y entidades pueden existir durante un período de tiempo indefinido. Esto es importante a la hora de rotar las claves de cifrado, ya que una orquestación o las entidades pueden ejecutarse durante más tiempo que la directiva de rotación de claves. Si se produce una rotación de claves, es posible que la clave usada para cifrar los datos ya no esté disponible para descifrarlos la próxima vez que se ejecute la orquestación o entidad. Por lo tanto, se recomienda el cifrado del cliente solo cuando se espera que las orquestaciones y entidades se ejecuten durante períodos de tiempo relativamente cortos.

## <a name="customizing-serialization-and-deserialization"></a>Personalización de la serialización y deserialización

# <a name="c"></a>[C#](#tab/csharp)

### <a name="default-serialization-logic"></a>Lógica de serialización predeterminada

Durable Functions usa internamente [Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para serializar los datos de orquestación y de entidad en JSON. Los valores predeterminados que Durable Functions usa para Json.NET son:

**Entradas, salidas y estado:**

```csharp
JsonSerializerSettings
{
    TypeNameHandling = TypeNameHandling.None,
    DateParseHandling = DateParseHandling.None,
}
```

**Excepciones:**

```csharp
JsonSerializerSettings
{
    ContractResolver = new ExceptionResolver(),
    TypeNameHandling = TypeNameHandling.Objects,
    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
}
```

Lea la documentación más detallada sobre `JsonSerializerSettings` [aquí](https://www.newtonsoft.com/json/help/html/SerializationSettings.htm).

## <a name="customizing-serialization-with-net-attributes"></a>Personalización de la serialización con atributos de .NET

Al serializar datos, Json.NET busca [varios atributos](https://www.newtonsoft.com/json/help/html/SerializationAttributes.htm) en las clases y propiedades que controlan cómo se serializan y deserializan los datos desde JSON. Si es el propietario del código fuente para el tipo de datos pasado a las API de Durable Functions, considere la posibilidad de agregar estos atributos al tipo para personalizar la serialización y deserialización.

## <a name="customizing-serialization-with-dependency-injection"></a>Personalización de la serialización con inserción de dependencias

Las aplicaciones de funciones que tienen como destino .NET y que se ejecutan en el entorno en tiempo de ejecución de Functions V3 pueden usar la [inserción de dependencias](../functions-dotnet-dependency-injection.md) para personalizar cómo se serializan los datos y las excepciones. En el código de ejemplo siguiente se muestra cómo usar la inserción de dependencias para invalidar los valores de serialización de Json.NET predeterminados mediante implementaciones personalizadas de las interfaces de servicio `IMessageSerializerSettingsFactory` y `IErrorSerializerSettingsFactory`.

```csharp
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Azure.WebJobs.Extensions.DurableTask;
using Microsoft.Extensions.DependencyInjection;
using Newtonsoft.Json;
using System.Collections.Generic;

[assembly: FunctionsStartup(typeof(MyApplication.Startup))]
namespace MyApplication
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddSingleton<IMessageSerializerSettingsFactory, CustomMessageSerializerSettingsFactory>();
            builder.Services.AddSingleton<IErrorSerializerSettingsFactory, CustomErrorSerializerSettingsFactory>();
        }

        /// <summary>
        /// A factory that provides the serialization for all inputs and outputs for activities and
        /// orchestrations, as well as entity state.
        /// </summary>
        internal class CustomMessageSerializerSettingsFactory : IMessageSerializerSettingsFactory
        {
            public JsonSerializerSettings CreateJsonSerializerSettings()
            {
                // Return your custom JsonSerializerSettings here
            }
        }

        /// <summary>
        /// A factory that provides the serialization for all exceptions thrown by activities
        /// and orchestrations
        /// </summary>
        internal class CustomErrorSerializerSettingsFactory : IErrorSerializerSettingsFactory
        {
            public JsonSerializerSettings CreateJsonSerializerSettings()
            {
                // Return your custom JsonSerializerSettings here
            }
        }
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

### <a name="serialization-and-deserialization-logic"></a>Lógica de serialización y deserialización

Las aplicaciones de Azure Functions para Node usan [`JSON.stringify()` para la serialización](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) y [`JSON.Parse()` para la deserialización](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse). La mayoría de los tipos se deberían serializar y deserializar sin problemas. En los casos en los que la lógica predeterminada sea insuficiente, la definición de un método `toJSON()` en el objeto secuestrará la lógica de serialización. Sin embargo, no existe ningún análogo para la deserialización de objetos.

Para la personalización completa de la canalización de serialización o deserialización, considere la posibilidad de controlar la serialización y deserialización con su propio código y pasar los datos como cadenas.


# <a name="python"></a>[Python](#tab/python)

### <a name="serialization-and-deserialization-logic"></a>Lógica de serialización y deserialización

Se recomienda encarecidamente usar anotaciones de tipo para asegurarse de que Durable Functions serialice y deserialice los datos correctamente. Aunque muchos tipos integrados se controlan automáticamente, algunos tipos de datos integrados requieren anotaciones de tipo para conservar el tipo durante la deserialización.

En el caso de los tipos de datos personalizados, debe definir la serialización y deserialización de JSON de un tipo de datos mediante la exportación de un método estático `to_json` y `from_json` desde la clase.

---
