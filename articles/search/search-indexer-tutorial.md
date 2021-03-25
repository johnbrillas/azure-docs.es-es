---
title: Tutorial de C# sobre la indexación de datos SQL de Azure
titleSuffix: Azure Cognitive Search
description: En este tutorial, se conectará a Azure SQL Database, extraerá datos que permiten búsquedas y los cargará en un índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 01/23/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: e2ca5f42120661b887d07e697596f41cb7a7fce4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98745773"
---
# <a name="tutorial-index-azure-sql-data-using-the-net-sdk"></a>Tutorial: Indexación de datos SQL de Azure mediante el SDK de .NET

Configure un [indexador](search-indexer-overview.md) que extraiga datos que permitan búsquedas desde Azure SQL Database y los envíe a un índice de búsqueda en Azure Cognitive Search. 

En este tutorial, usará C# y el [SDK de .NET](/dotnet/api/overview/azure/search) para realizar las tareas siguientes:

> [!div class="checklist"]
> * Creación de un origen de datos que se conecte a Azure SQL Database
> * Creación de un indexador
> * Ejecución de un indexador para cargar datos en un índice
> * Consulta de un índice como paso de comprobación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

+ [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Creación](search-create-service-portal.md) o [búsqueda de un servicio de búsqueda existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) 

> [!Note]
> Puede usar un servicio gratuito para este tutorial. Un servicio de búsqueda gratuito tiene una limitación de tres índices, tres indexadores y tres orígenes de datos. En este tutorial se crea uno de cada uno. Antes de empezar, asegúrese de que haya espacio en el servicio para aceptar los nuevos recursos.

## <a name="download-files"></a>Descarga de archivos

El código fuente de este tutorial se encuentra en la carpeta [DotNetHowToIndexer](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) del repositorio [Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) de GitHub.

## <a name="1---create-services"></a>1: Creación de servicios

En este tutorial se usa Azure Cognitive Search en la indexación y las consultas y Azure SQL Database como origen de datos externo. Si es posible, cree ambos servicios en la misma región y grupo de recursos por proximidad y capacidad de administración. En la práctica, Azure SQL Database puede estar en cualquier región.

### <a name="start-with-azure-sql-database"></a>Empezar con Azure SQL Database

En este paso creará un origen de datos externo en Azure SQL Database que se pueda rastrear con un indexador. Puede usar Azure Portal y el archivo *hotels.sql* de la descarga de ejemplo para crear el conjunto de datos en Azure SQL Database. Azure Cognitive Search usa conjuntos de filas planas, como los que se generan a partir de una vista o una consulta. El archivo SQL de la solución de ejemplo crea y rellena una sola tabla.

Si tiene un recurso de Azure SQL Database existente, puede agregarle la tabla de hoteles, a partir del paso 4.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/).

1. Busque o cree una instancia de **SQL Database**. Puede usar los valores predeterminados y el plan de tarifa más bajo. Una ventaja de la creación de un servidor es que puede especificar un nombre de usuario y una contraseña de administrador, que son necesarios para crear y cargar las tablas en un paso posterior.

   :::image type="content" source="media/search-indexer-tutorial/indexer-new-sqldb.png" alt-text="Página de base de datos nueva" border="false":::

1. Haga clic en **Revisar y crear** para implementar el nuevo servidor y la nueva base de datos. Espere a que se implementen el servidor y la base de datos.

1. En el panel de navegación, haga clic en **Editor de consultas (versión preliminar)** y escriba el nombre de usuario y la contraseña del administrador del servidor. 

   Si se le deniega el acceso, copie la dirección IP del cliente del mensaje de error y haga clic en el vínculo **Establecer el firewall del servidor** para agregar una regla que permita el acceso desde el equipo cliente, mediante la dirección IP del cliente para el intervalo. Puede que la regla tarde varios minutos en aplicarse.

1. En el editor de consultas, haga clic en **Abrir consulta** y vaya a la ubicación del archivo *hotels.sql* en el equipo local. 

1. Seleccione el archivo y haga clic en **Abrir**. El script debe tener un aspecto similar a la siguiente captura de pantalla:

   :::image type="content" source="media/search-indexer-tutorial/sql-script.png" alt-text="Script de SQL" border="false":::

1. Haga clic en **Ejecutar** para ejecutar la consulta. En el panel de resultados, verá un mensaje de consulta correcta, correspondiente a 3 filas.

1. Para devolver un conjunto de filas de esta tabla, puede ejecutar la siguiente consulta como paso de comprobación:

    ```sql
    SELECT * FROM Hotels
    ```

1. Copie la cadena de conexión ADO.NET correspondiente a la base de datos. En **Configuración** > **Cadenas de conexión**, copie la cadena de conexión ADO.NET, como en el ejemplo siguiente.

    ```sql
    Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```

Necesitará esta cadena de conexión en el ejercicio siguiente, configurando el entorno.

### <a name="azure-cognitive-search"></a>Azure Cognitive Search

El siguiente componente es Azure Cognitive Search, que se puede [crear en el portal](search-create-service-portal.md). Puede usar el nivel Gratis para completar este tutorial. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Obtención de una clave de API de administración y una dirección URL para Azure Cognitive Search

Las llamadas API requieren la dirección URL del servicio y una clave de acceso. Con ambos se crea un servicio de búsqueda, por lo que, si ha agregado Azure Cognitive Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

   :::image type="content" source="media/search-get-started-rest/get-url-key.png" alt-text="Obtención de una clave de acceso y un punto de conexión HTTP" border="false":::

## <a name="2---set-up-your-environment"></a>2: Configuración del entorno

1. Inicie Visual Studio y abra **DotNetHowToIndexers.sln**.

1. En el Explorador de soluciones, abra **appsettings.json** para proporcionar información de conexión.

1. Para `SearchServiceEndPoint`, si la URL completa en la página de información general del servicio es "https://my-demo-service.search.windows.net", el valor que hay que proporcionar es dicha URL.

1. Para `AzureSqlConnectionString`, el formato de cadena es similar al siguiente: `"Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"`

    ```json
    {
      "SearchServiceEndPoint": "<placeholder-search-url>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-search-service>",
      "AzureSqlConnectionString": "<placeholder-ADO.NET-connection-string",
    }
    ```

1. En la cadena de conexión, asegúrese de que la cadena de conexión contiene una contraseña válida. Mientras que los nombres de la base de datos y de usuario se copiarán, la contraseña debe escribirse manualmente.

## <a name="3---create-the-pipeline"></a>3: Creación de la canalización

Los indexadores requieren un objeto de origen de datos y un índice. El código pertinente aparece en dos archivos:

  + **hotel.cs**, que contiene un esquema que define el índice
  + **Program.cs**, que contiene las funciones para crear y administrar las estructuras del servicio

### <a name="in-hotelcs"></a>En hotel.cs

El esquema de índice define la colección de campos, incluidos los atributos que especifican las operaciones permitidas; por ejemplo, si un campo de texto completo permite búsquedas, filtros o clasificación, como se muestra en la siguiente definición de campo para HotelName. [SearchableField](/dotnet/api/azure.search.documents.indexes.models.searchablefield) es una clase de búsqueda de texto completo por definición. Otros atributos se asignan de forma explícita.

```csharp
. . . 
[SearchableField(IsFilterable = true, IsSortable = true)]
[JsonPropertyName("hotelName")]
public string HotelName { get; set; }
. . .
```

Un esquema también puede incluir otros elementos, como los perfiles de puntuación para aumentar una puntuación de búsqueda, analizadores personalizados y otras construcciones. Sin embargo, para nuestros fines, el esquema se define de forma dispersa, estando compuesto solo de los campos encontrados en los conjuntos de datos de ejemplo.

### <a name="in-programcs"></a>En Program.cs

El programa principal incluye la lógica para crear un [cliente de indizador](/dotnet/api/azure.search.documents.indexes.models.searchindexer), un índice, un origen de datos y un indizador. El código busca y elimina los recursos existentes del mismo nombre, bajo el supuesto de que este programa se podría ejecutar varias veces.

El objeto de origen de datos está configurado con valores específicos de los recursos de Azure SQL Database, [indexación parcial o incremental](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) incluida, para aprovechar las [características de detección de cambios](/sql/relational-databases/track-changes/about-change-tracking-sql-server) integradas de Azure SQL. La base de datos de demostración de hoteles de Azure SQL tiene una columna de "eliminación temporal" denominada **IsDeleted**. Cuando esta columna se establece en true en la base de datos, el indexador quita el documento correspondiente del índice de Azure Cognitive Search.

```csharp
Console.WriteLine("Creating data source...");

var dataSource =
      new SearchIndexerDataSourceConnection(
         "hotels-sql-ds",
         SearchIndexerDataSourceType.AzureSql,
         configuration["AzureSQLConnectionString"],
         new SearchIndexerDataContainer("hotels"));

indexerClient.CreateOrUpdateDataSourceConnection(dataSource);
```

Los objetos de indexador son independientes de las plataformas, donde la configuración la programación y la invocación son siempre los mismos, no importa el origen. Este indexador de ejemplo incluye un programa, una opción de restablecimiento que elimina su historial y llama un método para crear el indexador y ejecutarlo inmediatamente. Para crear o actualizar un indizador, utilice [CreateOrUpdateIndexerAsync](/dotnet/api/azure.search.documents.indexes.searchindexerclient.createorupdateindexerasync).

```csharp
Console.WriteLine("Creating Azure SQL indexer...");

var schedule = new IndexingSchedule(TimeSpan.FromDays(1))
{
      StartTime = DateTimeOffset.Now
};

var parameters = new IndexingParameters()
{
      BatchSize = 100,
      MaxFailedItems = 0,
      MaxFailedItemsPerBatch = 0
};

// Indexer declarations require a data source and search index.
// Common optional properties include a schedule, parameters, and field mappings
// The field mappings below are redundant due to how the Hotel class is defined, but 
// we included them anyway to show the syntax 
var indexer = new SearchIndexer("hotels-sql-idxr", dataSource.Name, searchIndex.Name)
{
      Description = "Data indexer",
      Schedule = schedule,
      Parameters = parameters,
      FieldMappings =
      {
         new FieldMapping("_id") {TargetFieldName = "HotelId"},
         new FieldMapping("Amenities") {TargetFieldName = "Tags"}
      }
};

await indexerClient.CreateOrUpdateIndexerAsync(indexer);
```

Las ejecuciones de indizador normalmente están programadas, pero en la fase de desarrollo tal vez desee ejecutar el indizador inmediatamente con [RunIndexerAsync](/dotnet/api/azure.search.documents.indexes.searchindexerclient.runindexerasync).

```csharp
Console.WriteLine("Running Azure SQL indexer...");

try
{
      await indexerClient.RunIndexerAsync(indexer.Name);
}
catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
{
      Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
}
```

## <a name="4---build-the-solution"></a>4: Compilación de la solución

Presione F5 para compilar y ejecutar la solución. El programa se ejecuta en modo de depuración. Una ventana de consola informa del estado de cada operación.

   :::image type="content" source="media/search-indexer-tutorial/console-output.png" alt-text="Salida de consola" border="false":::

El código se ejecuta localmente en Visual Studio y se conecta al servicio de búsqueda en Azure, que, a su vez, se conecta a Azure SQL Database y recupera el conjunto de datos. Con estas muchas operaciones, hay varios puntos de error posibles. Si recibe un error, compruebe primero las condiciones siguientes:

+ La información de conexión del servicio de búsqueda que proporcione es la URL completa. Si solo especificó el nombre del servicio, las operaciones se detendrán al crearse el índice y se generará un error de conexión.

+ La información de conexión de la base de datos **appsettings.json**. Debe ser la cadena de conexión de ADO.NET obtenida del portal, modificada para incluir un nombre de usuario y una contraseña que sean válidos para la base de datos. La cuenta de usuario debe tener permiso para recuperar datos. Hay que permitir acceso de entrada a la dirección IP del cliente local a través del firewall.

+ Los límites de recursos. Recuerde que el nivel Gratis tiene un límite de tres índices, tres indexadores y tres orígenes de datos. Un servicio en el límite máximo no puede crear nuevos objetos.

## <a name="5---search"></a>5: Búsqueda

Use Azure Portal para comprobar la creación de objetos y use **Explorador de búsqueda** para consultar el índice.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Información general** del servicio de búsqueda, abra cada lista correspondiente para comprobar que el objeto se ha creado. **Índices**, **Indexadores** y **Orígenes de datos** contendrán "hotels", "azure-sql-indexer" y "azure-sql", respectivamente.

   :::image type="content" source="media/search-indexer-tutorial/tiles-portal.png" alt-text="Iconos de indexador y origen de datos" border="false":::

1. Seleccione el índice hotels. En la página de hoteles, **Explorador de búsqueda** es la primera pestaña. 

1. Haga clic en **Buscar** para emitir una consulta vacía. 

   Las tres entradas del índice se devuelven como documentos JSON. El Explorador de búsqueda devuelve documentos en JSON para que pueda ver la estructura completa.

   :::image type="content" source="media/search-indexer-tutorial/portal-search.png" alt-text="Consulta de un índice" border="false":::
   
1. A continuación, escriba una cadena de búsqueda: `search=river&$count=true`. 

   Esta consulta invoca la búsqueda de texto completo en el término `river`, y el resultado incluye un recuento de los documentos coincidentes. Devolver el número de documentos coincidencias es útil en escenarios de pruebas cuando se tiene un índice grande con miles o millones de documentos. En este caso, solo un documento coincide con la consulta.

1. Por último, escriba una cadena de búsqueda que limite la salida JSON a los campos de interés: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   La respuesta de la consulta se reduce a los campos seleccionados, lo que da lugar a una salida más concisa.

## <a name="reset-and-rerun"></a>Restablecer y volver a ejecutar

En las primeras etapas experimentales de desarrollo, el enfoque más práctico para las iteraciones del diseño es eliminar los objetos de Azure Cognitive Search y permitir que el código vuelva a generarlos. Los nombres de los recursos son únicos. La eliminación de un objeto permite volver a crearlo con el mismo nombre.

En el código de ejemplo de este tutorial se comprueban los objetos existentes y se eliminan para que pueda volver a ejecutar el código.

También puede usar el portal para eliminar los índices, indexadores y orígenes de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando trabaje con su propia suscripción, al final de un proyecto, es recomendable eliminar los recursos que ya no necesite. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Puede eliminar los recursos de forma individual o eliminar el grupo de recursos para eliminar todo el conjunto de recursos.

Puede buscar y administrar los recursos en el portal, mediante el vínculo Todos los recursos o Grupos de recursos en el panel de navegación izquierdo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con los aspectos básicos de la indexación de SQL Database, podemos abordar en detalle la configuración del indexador.

> [!div class="nextstepaction"]
> [Configuración del indexador de SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)