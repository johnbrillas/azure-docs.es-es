---
title: Enumerar las bases de datos de la API de SQL restaurable en Azure Cosmos DB mediante la API de REST
description: Muestre la fuente de eventos de todas las mutaciones realizadas en todas las instancias de SQL Database de Azure Cosmos DB en la cuenta restaurable. Esto ayuda en el escenario en el que la base de datos se elimina accidentalmente para obtener el tiempo de eliminación.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 218a39a2bf8e9269e43c4876c1654d94be886997
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539508"
---
# <a name="list-restorable-sql-api-databases-in-azure-cosmos-db-using-rest-api"></a>Enumerar las bases de datos de la API de SQL restaurable en Azure Cosmos DB mediante la API de REST

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Muestre la fuente de eventos de todas las mutaciones realizadas en todas las instancias de SQL Database de Azure Cosmos DB en la cuenta restaurable. Esto ayuda en el escenario en el que la base de datos se elimina accidentalmente para obtener el tiempo de eliminación. Esta API requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlDatabases?api-version=2020-06-01-preview
```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **instanceId** | path | True |string| GUID instanceId de una cuenta de base de datos restaurable. |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para la operación. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableSqlDatabasesListResult](#restorablesqldatabaseslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación. |

## <a name="examples"></a>Ejemplos

### <a name="cosmosdbrestorablesqldatabaselist"></a>CosmosDBRestorableSqlDatabaseList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases?api-version=2020-06-01-preview
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/36f09704-6be3-4f33-aa05-17b73e504c75/restorableSqlDatabases/59c21367-b98b-4a8e-abb7-b6f46600decc",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "59c21367-b98b-4a8e-abb7-b6f46600decc",
      "properties": {
        "resource": {
          "_rid": "DLB14gAAAA==",
          "eventTimestamp": "2020-09-02T19:45:03Z",
          "ownerId": "Database1",
          "ownerResourceId": "3fu-hg==",
          "operationType": "Create",
          "database": {
            "id": "Database1",
            "_rid": "3fu-hg==",
            "_self": "dbs/3fu-hg==/",
            "_etag": "\"0000c20a-0000-0700-0000-5f4ff63f0000\"",
            "_colls": "colls/",
            "_users": "users/"
          }
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases/8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "properties": {
        "resource": {
          "_rid": "ESXNLAAAAA==",
          "eventTimestamp": "2020-09-02T19:53:42Z",
          "ownerId": "Database1",
          "ownerResourceId": "3fu-hg==",
          "database": {
            "id": "Database1",
            "_rid": "3fu-hg==",
            "_self": "dbs/3fu-hg==/",
            "_etag": "\"0000c20a-0000-0700-0000-5f4ff63f0000\"",
            "_colls": "colls/",
            "_users": "users/",
            "_ts": 1599075903
          },
          "operationType": "Delete"
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases/2c07991b-9c7c-4e85-be68-b18c1f2ff326",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "2c07991b-9c7c-4e85-be68-b18c1f2ff326",
      "properties": {
        "resource": {
          "_rid": "aXFqUQAAAA==",
          "eventTimestamp": "2020-09-02T19:53:15Z",
          "ownerId": "Database2",
          "ownerResourceId": "0SziSg==",
          "database": {
            "id": "Database2",
            "_rid": "0SziSg==",
            "_self": "dbs/0SziSg==/",
            "_etag": "\"0000ca0a-0000-0700-0000-5f4ff82b0000\"",
            "_colls": "colls/",
            "_users": "users/"
          },
          "operationType": "Create"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definición | Descripción | | --- || --- | | [Base de datos](#database) | Objeto de recurso de instancia de SQL Database de Azure Cosmos DB | | [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. | | [ErrorResponse](#errorresponse) | Respuesta de error. | | [OperationType](#operationtype) | Enumeración que indica el tipo de operación del evento. | | [Recurso](#resource) | El recurso de un evento de instancia de SQL Database de Azure Cosmos DB | | [RestorableSqlDatabaseGetResult](#restorablesqldatabasegetresult) | Evento de instancia de SQL Database de Azure Cosmos DB | | [RestorableSqlDatabaseProperties](#restorablesqldatabaseproperties) | Propiedades de un evento de instancia de SQL Database de Azure Cosmos DB | | [RestorableSqlDatabasesListResult](#restorablesqldatabaseslistresult) | Respuesta de operación de lista, que contiene los eventos de la instancia de SQL Database y sus propiedades. |

### <a name="database"></a><a id="database"></a>Base de datos

Objeto de recurso de SQL Database de Azure Cosmos DB

| **Nombre** | **Tipo** | **Descripción** | | --- || --- | ---| | _colls |cadena| Propiedad generada por el sistema que especificó la ruta de acceso direccionable del recurso de colecciones. | | _etag |cadena| Propiedad generada por el sistema que representa el objeto etag de recursos necesario para el control de simultaneidad optimista. | | _rid |cadena| Propiedad generada por el sistema. Identificador único. | | _self |cadena| Propiedad generada por el sistema que especifica la ruta de acceso direccionable del recurso de base de datos. | | _ts | | Propiedad generada por el sistema que denota la última marca de tiempo actualizada del recurso. | | _users |cadena| Propiedad generada por el sistema que especifica la ruta de acceso direccionable del recurso del usuario. | | ID |cadena| Nombre de la instancia de SQL Database de Azure Cosmos DB |

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
| SystemOperation |string|evento de modificación de base de datos desencadenado por el sistema. El usuario no ha iniciado este evento.|

### <a name="resource"></a><a id="resource"></a>Resource

Recurso de un evento de instancia de SQL Database de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| _rid |string| Propiedad generada por el sistema. Identificador único. |
| database |[Base de datos](#database)| Objeto de recurso de SQL Database de Azure Cosmos DB |
| eventTimestamp |string| Hora en la que se produjo este evento de base de datos. |
| operationType |[OperationType](#operationtype)| Tipo de operación de este evento de base de datos. |
| ownerId |string| Nombre de la instancia de SQL Database. |
| ownerResourceId |string| Identificador de recurso de la instancia de SQL Database. |

### <a name="restorablesqldatabasegetresult"></a><a id="restorablesqldatabasegetresult"></a>RestorableSqlDatabaseGetResult

Evento de instancia de SQL Database de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| id |string| Identificador único del recurso de Azure Resource Manager. |
| name |string| Nombre del recurso de Azure Resource Manager. |
| properties | [RestorableSqlDatabaseProperties](#restorablesqldatabaseproperties)| Propiedades de un evento de SQL Database. |
| type |string| Tipo de recurso de Azure. |

### <a name="restorablesqldatabaseproperties"></a><a id="restorablesqldatabaseproperties"></a>RestorableSqlDatabaseProperties

Propiedades de un evento de instancia de SQL Database de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| resource |[Recurso](#resource)| Recurso de un evento de instancia de SQL Database de Azure Cosmos DB. |

### <a name="restorablesqldatabaseslistresult"></a><a id="restorablesqldatabaseslistresult"></a>RestorableSqlDatabasesListResult

Respuesta de la operación de lista, que contiene los eventos de la instancia de SQL Database y sus propiedades.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[RestorableSqlDatabaseGetResult](#restorablesqldatabasegetresult)[]| Lista de eventos de base de datos SQL y sus propiedades. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumerar contenedores restaurables](restorable-sql-containers-list.md) en la API de SQL de Azure Cosmos DB mediante la API de REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.