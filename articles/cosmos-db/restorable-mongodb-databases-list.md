---
title: Enumeración de las bases de datos restaurables de la API de Azure Cosmos DB para MongoDB mediante la API REST
description: Muestre la fuente de eventos de todas las mutaciones realizadas en todas las bases de datos de MongoDB de Azure Cosmos DB en la cuenta restaurable. Esto ayuda en el escenario en el que la base de datos se elimina accidentalmente para obtener el tiempo de eliminación.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 5ebb0a23822074f61a16bf1d7652dba589399542
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537517"
---
# <a name="list-restorable-databases-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>Enumeración de las bases de datos restaurables de la API de Azure Cosmos DB para MongoDB mediante la API REST

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Muestre la fuente de eventos de todas las mutaciones realizadas en todas las bases de datos de MongoDB de Azure Cosmos DB en la cuenta restaurable. Esto ayuda en el escenario en el que la base de datos se elimina accidentalmente para obtener el tiempo de eliminación. Esta API requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbDatabases?api-version=2020-06-01-preview
```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **instanceId** | path | True |string| El GUID de instanceId de una cuenta de base de datos restaurable. |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para esta operación. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableMongodbDatabasesListResult](#restorablemongodbdatabaseslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación.|

## <a name="examples"></a>Ejemplos

### <a name="cosmosdbrestorablemongodbdatabaselist"></a>CosmosDBRestorableMongodbDatabaseList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbDatabases?api-version=2020-06-01-preview
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/36f09704-6be3-4f33-aa05-17b73e504c75/restorableMongodbDatabases/59c21367-b98b-4a8e-abb7-b6f46600decc",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbDatabases",
      "name": "59c21367-b98b-4a8e-abb7-b6f46600decc",
      "properties": {
        "resource": {
          "_rid": "DLB14gAAAA==",
          "eventTimestamp": "2020-09-02T19:45:03Z",
          "ownerId": "Database1",
          "ownerResourceId": "PD5DALigDgw=",
          "operationType": "Create"
        }
      }
    },
    {
      "id": "/subscriptions/2296c272-5d55-40d9-bc05-4d56dc2d7588/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbDatabases/8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbDatabases",
      "name": "8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "properties": {
        "resource": {
          "_rid": "ESXNLAAAAA==",
          "eventTimestamp": "2020-09-02T19:53:42Z",
          "ownerId": "Database1",
          "ownerResourceId": "PD5DALigDgw=",
          "operationType": "Delete"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definition  | Description| | --- || --- | | [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. | | [ErrorResponse](#errorresponse) | Respuesta de error. | | [OperationType](#operationtype) | Enumeración que indica el tipo de operación del evento. | | [Recurso](#resource) | Recurso de un evento de base de datos de API de Azure Cosmos DB para MongoDB | | [RestorableMongodbDatabaseGetResult](#restorablemongodbdatabasegetresult) | Evento de base de datos de API de Azure Cosmos DB para MongoDB | | [RestorableMongodbDatabaseProperties](#restorablemongodbdatabaseproperties) | Propiedades de un evento de base de datos de API de Azure Cosmos DB para MongoDB | | [RestorableMongodbDatabasesListResult](#restorablemongodbdatabaseslistresult) | Respuesta de operación de enumeración, que contiene los eventos de base de datos de API de Azure Cosmos DB para MongoDB. |

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
| Crear |string|evento de creación de base de datos|
| Eliminar |string|evento de eliminación de base de datos|
| Replace |string|evento de modificación de base de datos|

### <a name="resource"></a><a id="resource"></a>Resource

Recurso de un evento de base de datos de API de Azure Cosmos DB para MongoDB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| _rid |string| Propiedad generada por el sistema. Identificador único. |
| eventTimestamp |string| Hora en la que se produjo este evento de base de datos. |
| operationType |[OperationType](#operationtype)| Tipo de operación de este evento de base de datos.  |
| ownerId |string| Nombre de la base de datos de la API de Azure Cosmos DB para MongoDB.|
| ownerResourceId |string| Identificador de recurso de una base de datos de API de Azure Cosmos DB para MongoDB. |

### <a name="restorablemongodbdatabasegetresult"></a><a id="restorablemongodbdatabasegetresult"></a>RestorableMongodbDatabaseGetResult

Evento de base de datos de API de Azure Cosmos DB para MongoDB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| id |string| Identificador único del recurso de Azure Resource Manager. |
| name |string| Nombre del recurso de Resource Manager. |
| properties |[RestorableMongodbDatabaseProperties](#restorablemongodbdatabaseproperties)| Propiedades de un evento de base de datos de API de Azure Cosmos DB para MongoDB. |
| type |string| Tipo de recurso de Azure. |

### <a name="restorablemongodbdatabaseproperties"></a><a id="restorablemongodbdatabaseproperties"></a>RestorableMongodbDatabaseProperties

Propiedades de un evento de base de datos de API de Azure Cosmos DB para MongoDB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| resource |[Recurso](#resource)| Recurso de un evento de base de datos de API de Azure Cosmos DB para MongoDB. |

### <a name="restorablemongodbdatabaseslistresult"></a><a id="restorablemongodbdatabaseslistresult"></a>RestorableMongodbDatabasesListResult

Respuesta de la operación de enumeración, que contiene los eventos de base de datos y sus propiedades.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[RestorableMongodbDatabaseGetResult](#restorablemongodbdatabasegetresult)[]| Lista de eventos de base de datos y sus propiedades. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumerar los recursos restaurables](restorable-mongodb-resources-list.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Enumerar las colecciones restaurables](restorable-mongodb-collections-list.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.