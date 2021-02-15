---
title: Enumerar los recursos restaurables de la API de Azure Cosmos DB para MongoDB mediante la API REST.
description: Devuelve una lista de las combinaciones de base de datos y colección que existen en la cuenta en la marca de tiempo y ubicación especificadas. Esto ayuda en escenarios para validar los recursos existentes en una marca de tiempo y ubicación determinadas.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: b5800c2e9c1f3b36a3ac9afe6cd68f706505fbe0
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537448"
---
# <a name="list-restorable-resources-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>Enumerar los recursos restaurables de la API de Azure Cosmos DB para MongoDB mediante la API REST.

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Devuelve una lista de las combinaciones de base de datos y colección que existen en la cuenta en la marca de tiempo y ubicación especificadas. Esto ayuda en escenarios para validar los recursos existentes en una marca de tiempo y ubicación determinadas. Esta API requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbResources?api-version=2020-06-01-preview
```

Con parámetros opcionales:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbResources?api-version=2020-06-01-preview&amp;restoreLocation={restoreLocation}&amp;restoreTimestampInUtc={restoreTimestampInUtc}
```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **instanceId** | path | True |string| El GUID de instanceId de una cuenta de base de datos restaurable. |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para esta operación. |
| **restoreLocation** | Query | |string| Ubicación donde se almacenan los recursos restaurables. |
| **restoreTimestampInUtc** | Query | |string| Marca de tiempo en la que existían los recursos restaurables. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableMongodbResourcesListResult](#restorablemongodbresourceslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación. |


## <a name="examples"></a>Ejemplos

### <a name="cosmosdbrestorablemongodbresourcelist"></a>CosmosDBRestorableMongodbResourceList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbResources?api-version=2020-06-01-preview&amp;restoreLocation=WestUS&amp;restoreTimestampInUtc=10/13/2020 4:56
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "databaseName": "Database1",
      "collectionNames": [
        "Collection1"
      ]
    },
    {
      "databaseName": "Database2",
      "collectionNames": [
        "Collection1",
        "Collection2"
      ]
    },
    {
      "databaseName": "Database3",
      "collectionNames": []
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definición  | Descripción | --- || --- | | [DatabaseRestoreResource](#databaserestoreresource) | Bases de datos específicas para restaurar. | | [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. | | [ErrorResponse](#errorresponse) | Respuesta de error. | | [RestorableMongodbResourcesListResult](#restorablemongodbresourceslistresult) | Respuesta de la operación de enumeración que contiene los recursos de SQL restaurables. |

### <a name="databaserestoreresource"></a><a id="databaserestoreresource"></a>DatabaseRestoreResource

Bases de datos específicas para restaurar.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| collectionNames |string[]| Nombres de las colecciones disponibles para restaurar. |
| databaseName |string| Nombre de la base de datos disponible para restaurar. |

### <a name="defaulterrorresponse"></a><a id="defaulterrorresponse"></a>DefaultErrorResponse

Respuesta de error del servicio.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| error | [ErrorResponse](#errorresponse)| Respuesta de error. |

### <a name="errorresponse"></a><a id="errorresponse"></a>ErrorResponse

Respuesta de error.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| código |string| Código de error. |
| message |string| Mensaje de error que indica por qué se produjo un error en la operación. |

### <a name="restorablemongodbresourceslistresult"></a><a id="restorablemongodbresourceslistresult"></a>RestorableMongodbResourcesListResult

Respuesta de la operación de enumeración, que contiene los recursos de la API de Azure Cosmos DB para MongoDB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[DatabaseRestoreResource](#databaserestoreresource)[]| Lista de recursos de la API de Azure Cosmos DB para MongoDB restaurables, incluidos los nombres de base de datos y colección. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de las bases de datos restaurables](restorable-mongodb-databases-list.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.