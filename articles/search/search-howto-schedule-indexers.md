---
title: Programar la ejecución del indexador
titleSuffix: Azure Cognitive Search
description: Programe los indizadores de Azure Cognitive Search para indexar el contenido periódicamente o en horas específicas.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/28/2021
ms.openlocfilehash: dfd8526a035d4eef4d07539e541e37c88023b500
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99063220"
---
# <a name="how-to-schedule-indexers-in-azure-cognitive-search"></a>Programación de indizadores de Azure Cognitive Search

Los indexadores normalmente se ejecutan una vez, inmediatamente después de que se crean. Después, puede volver a ejecutarlos a petición mediante Azure Portal, [Ejecutar indizador (REST)](/rest/api/searchservice/run-indexer) o un SDK de Azure. También puede configurar un indexador para que se ejecute según una programación. Estas son algunas situaciones en las que resulta útil programar el indexador:

* Los datos de origen cambiarán con el tiempo, y desea que el indexador de búsqueda procese automáticamente el archivo delta.
* Los datos de origen son muy grandes y desea expandir el procesamiento del indexador a lo largo del tiempo. Para más información acerca de la indexación de grandes volúmenes de datos, consulte [Indexación de grandes conjuntos de datos en Azure Cognitive Search](search-howto-large-index.md).
* Un índice de búsqueda se rellenará desde varios orígenes de datos y quiere que los indexadores se ejecuten a horas distintas para reducir los conflictos.

De manera visual, una programación podría ser similar a la siguiente: a partir del 1 de enero y en ejecución cada 50 minutos.

```json
{
    "dataSourceName" : "myazuresqldatasource",
    "targetIndexName" : "target index name",
    "schedule" : { "interval" : "PT50M", "startTime" : "2021-01-01T00:00:00Z" }
}
```

> [!NOTE]
> El programador es una característica integrada de Azure Cognitive Search. No se admiten programadores externos.

## <a name="schedule-property"></a>Propiedad schedule

La programación forma parte de la definición del indexador. Si la propiedad **schedule** se omite, el indexador se ejecutará una sola vez inmediatamente después de su creación. Si agrega una propiedad **schedule**, especificará dos elementos.

| Propiedad | Descripción |
|----------|-------------|
|**Intervalo** | (Obligatorio) Se refiere al tiempo entre el inicio de dos ejecuciones consecutivas de indexador. El intervalo más breve que se permite es 5 minutos, mientras que el mayor es 1440 minutos (24 horas). Debe tener el formato de un valor "dayTimeDuration" XSD (subconjunto restringido de un valor de [duración ISO 8601](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). El patrón de este es: `P(nD)(T(nH)(nM))`. <br/><br/>Ejemplos: `PT15M` para cada 15 minutos, `PT2H` para cada 2 horas.|
| **Hora de inicio (UTC)** | (Opcional) Indica cuándo deben comenzar las ejecuciones programadas. Si se omite, se usará la hora UTC actual. Este tiempo puede estar en el pasado, en cuyo caso la primera ejecución se programa como si el indexador se hubiera ejecutado continuamente desde el valor de **starTime**.<br/><br/>Ejemplos: `2021-01-01T00:00:00Z` a partir de la medianoche del 1 de enero, `2021-01-05T22:28:00Z` a partir de las 10:28 p.m. del 5 de enero.|

## <a name="scheduling-behavior"></a>Comportamiento de la programación

Solo se puede ejecutar a la vez una ejecución de un indexador. Si se está ejecutando un indexador cuando está programada su próxima ejecución, esta se pospone hasta la próxima hora programada.

Veamos un ejemplo entenderlo concretamente. Supongamos que se configura una programación de indexador con un **intervalo** de cada hora y su **hora de inicio** es el 1 de junio de 2019 a las 8:00:00 A. M. UTC. Esto es lo que puede suceder cuando la ejecución de un indexador tarda más de una hora:

* La primera ejecución del indexador se inicia el 1 de marzo de 2019 a las 8:00 A. M. Suponga que esta ejecución tarda 20 minutos (o un período inferior a 1 hora).
* La segunda ejecución se inicia el 1 de junio de 2019 a las 9:00 A. M UTC. Suponga que esta ejecución tarda 70 minutos (más de una hora) y no se completará hasta 10:10 A. M. UTC.
* La tercera ejecución está programada para empezar a 10:00 A. M. UTC, pero en ese momento todavía se está ejecutando la ejecución anterior. Luego esta ejecución programada se omite. La siguiente ejecución del indexador no se iniciará hasta las 11:00 A. M. UTC.

> [!NOTE]
> Si un indexador se establece según una programación determinada pero se produce repetidamente un error en el mismo documento cada vez, el indexador comenzará a ejecutarse en un intervalo menos frecuente (hasta el intervalo máximo de al menos una vez cada 24 horas) hasta que vuelva a avanzar correctamente. Si cree que corrigió el problema subyacente, puede ejecutar el indexador de manera manual y, si la indexación se realiza correctamente, el indexador volverá a su programación normal.

## <a name="schedule-using-rest"></a>Programación mediante REST

Especifique la propiedad **schedule** al crear o actualizar el indexador.

```http
    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2021-01-01T00:00:00Z" }
    }
```

## <a name="schedule-using-net"></a>Programación mediante .NET

En el ejemplo de C# siguiente se crea un indexador de base de datos de Azure SQL, para lo que se usa un origen de datos y un índice predefinidos, y se establece su programación para que se ejecute una vez al día a partir de este momento:

```csharp
var schedule = new IndexingSchedule(TimeSpan.FromDays(1))
{
    StartTime = DateTimeOffset.Now
};

var indexer = new SearchIndexer("hotels-sql-idxr", dataSource.Name, searchIndex.Name)
{
    Description = "Data indexer",
    Schedule = schedule
};

await indexerClient.CreateOrUpdateIndexerAsync(indexer);
```

La programación se define mediante la clase [IndexingSchedule](/dotnet/api/azure.search.documents.indexes.models.indexingschedule), al crear o actualizar un indexador mediante [SearchIndexerClient](/dotnet/api/azure.search.documents.indexes.searchindexerclient). El constructor **IndexingSchedule** requiere que se especifique un parámetro **Interval** mediante un objeto **TimeSpan**. Como se indicó anteriormente, el valor de intervalo más pequeño que se permite es 5 minutos, mientras que el mayor es 24 horas. El segundo parámetro **StartTime**, que se especifica como un objeto **DateTimeOffset**, es opcional.

## <a name="next-steps"></a>Pasos siguientes

En el caso de los indexadores que se ejecutan según una programación, puede supervisar las operaciones recuperando el estado del servicio de búsqueda, o bien obtener información detallada habilitando el registro de diagnóstico.

* [Supervisión del estado del indexador de búsqueda](search-howto-monitor-indexers.md)
* [Recopilación y análisis de datos de registro](search-monitor-logs.md)