---
title: Definición de claves únicas para un contenedor de Azure Cosmos
description: Aprenda a definir claves únicas para un contenedor de Azure Cosmos mediante Azure Portal, PowerShell, .NET, Java y otros SDK.
author: ThomasWeiss
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 12/02/2019
ms.author: thweiss
ms.custom: devx-track-python, devx-track-js, devx-track-csharp
ms.openlocfilehash: 55fc5222c1c245c56ba0a26caa816c5c845147c1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93336629"
---
# <a name="define-unique-keys-for-an-azure-cosmos-container"></a>Definición de claves únicas para un contenedor de Azure Cosmos
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

En este artículo se presentan las distintas formas de definir [claves únicas](unique-keys.md) al crear un contenedor de Azure Cosmos. Actualmente, es posible realizar esta operación mediante Azure Portal o uno de los SDK.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. [Cree una cuenta de Azure Cosmos](create-sql-api-dotnet.md#create-account) o seleccione una ya existente.

1. Abra el panel **Data Explorer** y seleccione el contenedor en el que quiere trabajar.

1. Haga clic en **Nuevo contenedor**.

1. En el cuadro de diálogo **Agregar contenedor**, haga clic en **+ Add unique key** (+ Agregar clave única) para agregar una única entrada de clave.

1. Escriba la ruta o rutas de la restricción de clave única

1. Si es necesario, puede agregar más entradas de clave única si hace clic en **+ Add unique key** (+ Agregar clave única).

    :::image type="content" source="./media/how-to-define-unique-keys/unique-keys-portal.png" alt-text="Captura de pantalla de una entrada de restricción de clave única en Azure Portal":::

## <a name="use-powershell"></a>Uso de PowerShell

Para crear un contenedor con claves únicas, consulte [Creación de un contenedor de Azure Cosmos con una clave única y TTL](manage-with-powershell.md#create-container-unique-key-ttl).

## <a name="use-the-net-sdk"></a>Uso del SDK de .NET

# <a name="net-sdk-v2"></a>[SDK de .NET V2](#tab/dotnetv2)

Al crear un contenedor mediante [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/), se puede usar un objeto `UniqueKeyPolicy` para definir las restricciones de clave única.

```csharp
client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("database"), new DocumentCollection
{
    Id = "container",
    PartitionKey = new PartitionKeyDefinition { Paths = new Collection<string>(new List<string> { "/myPartitionKey" }) },
    UniqueKeyPolicy = new UniqueKeyPolicy
    {
        UniqueKeys = new Collection<UniqueKey>(new List<UniqueKey>
        {
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/firstName", "/lastName", "/emailAddress" }) },
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/address/zipCode" }) }
        })
    }
});
```

# <a name="net-sdk-v3"></a>[SDK de .NET V3](#tab/dotnetv3)

Al crear un nuevo contenedor mediante [.NET SDK v3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/), use la API fluida del SDK para declarar claves únicas de forma concisa y legible.

```csharp
await client.GetDatabase("database").DefineContainer(name: "container", partitionKeyPath: "/myPartitionKey")
    .WithUniqueKey()
        .Path("/firstName")
        .Path("/lastName")
        .Path("/emailAddress")
    .Attach()
    .WithUniqueKey()
        .Path("/address/zipCode")
    .Attach()
    .CreateIfNotExistsAsync();
```
---

## <a name="use-the-java-sdk"></a>Uso del SDK de Java

Al crear un contenedor mediante el [SDK de Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb), se puede usar un objeto `UniqueKeyPolicy` para definir las restricciones de clave única.

```java
// create a new DocumentCollection object
DocumentCollection container = new DocumentCollection();
container.setId("container");

// create array of strings and populate them with the unique key paths
Collection<String> uniqueKey1Paths = new ArrayList<String>();
uniqueKey1Paths.add("/firstName");
uniqueKey1Paths.add("/lastName");
uniqueKey1Paths.add("/emailAddress");
Collection<String> uniqueKey2Paths = new ArrayList<String>();
uniqueKey2Paths.add("/address/zipCode");

// create UniqueKey objects and set their paths
UniqueKey uniqueKey1 = new UniqueKey();
UniqueKey uniqueKey2 = new UniqueKey();
uniqueKey1.setPaths(uniqueKey1Paths);
uniqueKey2.setPaths(uniqueKey2Paths);

// create a new UniqueKeyPolicy object and set its unique keys
UniqueKeyPolicy uniqueKeyPolicy = new UniqueKeyPolicy();
Collection<UniqueKey> uniqueKeys = new ArrayList<UniqueKey>();
uniqueKeys.add(uniqueKey1);
uniqueKeys.add(uniqueKey2);
uniqueKeyPolicy.setUniqueKeys(uniqueKeys);

// set the unique key policy
container.setUniqueKeyPolicy(uniqueKeyPolicy);

// create the container
client.createCollection(String.format("/dbs/%s", "database"), container, null);
```

## <a name="use-the-nodejs-sdk"></a>Uso del SDK de Node.js

Al crear un contenedor mediante el [SDK de Node.js](https://www.npmjs.com/package/@azure/cosmos), se puede usar un objeto `UniqueKeyPolicy` para definir las restricciones de clave única.

```javascript
client.database('database').containers.create({
    id: 'container',
    uniqueKeyPolicy: {
        uniqueKeys: [
            { paths: ['/firstName', '/lastName', '/emailAddress'] },
            { paths: ['/address/zipCode'] }
        ]
    }
});
```

## <a name="use-the-python-sdk"></a>Uso del SDK de Python

Al crear un nuevo contenedor mediante el [SDK de Python](https://pypi.org/project/azure-cosmos/), se pueden especificar restricciones de clave única como parte del diccionario pasado como parámetro.

```python
client.CreateContainer('dbs/' + config['DATABASE'], {
    'id': 'container',
    'uniqueKeyPolicy': {
        'uniqueKeys': [
            {'paths': ['/firstName', '/lastName', '/emailAddress']},
            {'paths': ['/address/zipCode']}
        ]
    }
})
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [creación de particiones](partitioning-overview.md).
- Explore [cómo funciona la indexación](index-overview.md).
