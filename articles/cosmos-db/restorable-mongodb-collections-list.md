---
title: 'Enumeración de las colecciones restaurables de la API de Azure Cosmos DB para MongoDB: API REST.'
description: Muestre la fuente de eventos de todas las mutaciones realizadas en todas las colecciones de MongoDB de Azure Cosmos DB en una base de datos específica. Esto ayuda en un escenario en el que el contenedor se eliminó accidentalmente.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: c19de134f40c58a687dcf6bac6ac156e46cef7da
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537550"
---
# <a name="list-restorable-collections-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>Enumeración de las colecciones restaurables de la API de Azure Cosmos DB para MongoDB mediante la API REST.

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Muestre la fuente de eventos de todas las mutaciones realizadas en todas las colecciones de la API de Azure Cosmos DB para MongoDB en una base de datos específica. Esto ayuda en un escenario en el que el contenedor se eliminó accidentalmente. Esta API requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbCollections?api-version=2020-06-01-preview
```

Con parámetros opcionales:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbCollections?api-version=2020-06-01-preview&amp;restorableMongodbDatabaseRid={restorableMongodbDatabaseRid}
```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **instanceId** | path | True |string| El GUID de instanceId de una cuenta de base de datos restaurable. |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para esta operación. |
| **restorableMongodbDatabaseRid** | Query | |string| Identificador de recurso de la base de datos de API de Azure Cosmos DB para MongoDB. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableMongodbCollectionsListResult](#restorablemongodbcollectionslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación.|

## <a name="examples"></a>Ejemplos

### <a name="cosmosdbrestorablemongodbcollectionlist"></a>CosmosDBRestorableMongodbCollectionList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableMongodbCollections?api-version=2020-06-01-preview&amp;restorableMongodbDatabaseRid=PD5DALigDgw=
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableMongodbCollections/79609a98-3394-41f8-911f-cfab0c075c86",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbCollections",
      "name": "79609a98-3394-41f8-911f-cfab0c075c86",
      "properties": {
        "resource": {
          "_rid": "zAyAPQAAAA==",
          "eventTimestamp": "2020-10-13T04:56:42Z",
          "ownerId": "Collection1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Create"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definición  | Descripción|
| --- | --- |
| [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. |
| [ErrorResponse](#errorresponse) | Respuesta de error. |
| [OperationType](#operationtype) | Enumeración que indica el tipo de operación del evento. |
| [Recurso](#resource) | Recurso de un evento de colección de API de Azure Cosmos DB para MongoDB. |
| [RestorableMongodbCollectionGetResult](#restorablemongodbcollectiongetresult) | Evento de colección de API de Azure Cosmos DB para MongoDB. |
| [RestorableMongodbCollectionProperties](#restorablemongodbcollectionproperties) | Propiedades de un evento de colección de API de Azure Cosmos DB para MongoDB. |
| [RestorableMongodbCollectionsListResult](#restorablemongodbcollectionslistresult) | Respuesta de la operación de enumeración, que contiene los eventos de colección y sus propiedades. |

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

### <a name="operationtype"></a><a id="operationtype"></a>OperationType

Enumeración que indica el tipo de operación del evento.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| Crear |string|evento de creación de colección|
| Eliminar |string|evento de eliminación de colección|
| Replace |string|evento de modificación de colección|

### <a name="resource"></a><a id="resource"></a>Resource

Recurso de un evento de colección de Azure Cosmos DB para MongoDB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| _rid |string| Propiedad generada por el sistema. Identificador único. |
| eventTimestamp |string| Hora en la que se produjo este evento de colección. |
| operationType |[OperationType](#operationtype)|  Tipo de operación de este evento de colección. |
| ownerId |string| Nombre de la colección de MongoDB.|
| ownerResourceId |string| Identificador de recurso de la colección de MongoDB. |

### <a name="restorablemongodbcollectiongetresult"></a><a id="restorablemongodbcollectiongetresult"></a>RestorableMongodbCollectionGetResult

Evento de colección de MongoDB de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| id |string| Identificador único del recurso de Azure Resource Manager. |
| name |string| Nombre del recurso de Resource Manager. |
| properties |[RestorableMongodbCollectionProperties](#restorablemongodbcollectionproperties)| Propiedades de un evento de colección. |
| type |string| Tipo de recurso de Azure. |

### <a name="restorablemongodbcollectionproperties"></a><a id="restorablemongodbcollectionproperties"></a>RestorableMongodbCollectionProperties

Propiedades de un evento de colección de MongoDB de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| resource | [Recurso](#resource)| Recurso de un evento de colección de API de Azure Cosmos DB para MongoDB. |

### <a name="restorablemongodbcollectionslistresult"></a><a id="restorablemongodbcollectionslistresult"></a>RestorableMongodbCollectionsListResult

Respuesta de la operación de enumeración, que contiene los eventos de colección y sus propiedades.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[RestorableMongodbCollectionGetResult](#restorablemongodbcollectiongetresult)[]| Lista de eventos de colección de API de Azure Cosmos DB para MongoDB y sus propiedades. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de las bases de datos restaurables](restorable-mongodb-databases-list.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.