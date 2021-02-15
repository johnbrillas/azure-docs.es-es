---
title: Enumeración de contenedores de la API de SQL restaurables de Azure Cosmos DB mediante la API de REST
description: Muestre la fuente de eventos de todas las mutaciones realizadas en todos los contenedores de SQL de Azure Cosmos DB en una base de datos específica. Esto ayuda en un escenario en el que el contenedor se eliminó accidentalmente.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: bba00b10d1a24dc29c1e986de1f0144c00350f5d
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537431"
---
# <a name="list-restorable-sql-api-containers-in-azure-cosmos-db-using-rest-api"></a>Enumeración de contenedores de la API de SQL restaurables de Azure Cosmos DB mediante la API de REST

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Muestre la fuente de eventos de todas las mutaciones realizadas en todos los contenedores de SQL de Azure Cosmos DB en una base de datos específica. Esto ayuda en un escenario en el que el contenedor se eliminó accidentalmente. Esta API requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlContainers?api-version=2020-06-01-preview
```

Con parámetros opcionales:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlContainers?api-version=2020-06-01-preview&amp;restorableSqlDatabaseRid={restorableSqlDatabaseRid}
```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **instanceId** | path | True |string| El GUID de instanceId de una cuenta de base de datos restaurable. |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para esta operación. |
| **restorableSqlDatabaseRid** | Query | |string| Identificador de recurso de la base de datos SQL. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableSqlContainersListResult](#restorablesqlcontainerslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación. |

## <a name="examples"></a>Ejemplos

### <a name="cosmosdbrestorablesqlcontainerlist"></a>CosmosDBRestorableSqlContainerList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers?api-version=2020-06-01-preview&amp;restorableSqlDatabaseRid=3fu-hg==
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers/79609a98-3394-41f8-911f-cfab0c075c86",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlContainers",
      "name": "79609a98-3394-41f8-911f-cfab0c075c86",
      "properties": {
        "resource": {
          "_rid": "zAyAPQAAAA==",
          "eventTimestamp": "2020-10-13T04:56:42Z",
          "ownerId": "Container1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Create",
          "container": {
            "id": "Container1",
            "indexingPolicy": {
              "indexingMode": "Consistent",
              "automatic": true,
              "includedPaths": [
                {
                  "path": "/*"
                },
                {
                  "path": "/\"_ts\"/?"
                }
              ],
              "excludedPaths": [
                {
                  "path": "/\"_etag\"/?"
                }
              ]
            },
            "conflictResolutionPolicy": {
              "mode": "LastWriterWins",
              "conflictResolutionPath": "/_ts",
              "conflictResolutionProcedure": ""
            },
            "_rid": "V18LoLrv-qA=",
            "_self": "dbs/V18LoA==/colls/V18LoLrv-qA=/",
            "_etag": "\"00003e00-0000-0700-0000-5f85338a0000\""
          }
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers/e85298a1-c631-4726-825e-a7ca092e9098",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlContainers",
      "name": "e85298a1-c631-4726-825e-a7ca092e9098",
      "properties": {
        "resource": {
          "_rid": "PrArcgAAAA==",
          "eventTimestamp": "2020-10-13T05:03:27Z",
          "ownerId": "Container1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Replace",
          "container": {
            "id": "Container1",
            "indexingPolicy": {
              "indexingMode": "Consistent",
              "automatic": true,
              "includedPaths": [
                {
                  "path": "/*"
                },
                {
                  "path": "/\"_ts\"/?"
                }
              ],
              "excludedPaths": [
                {
                  "path": "/\"_etag\"/?"
                }
              ]
            },
            "defaultTtl": 12345,
            "conflictResolutionPolicy": {
              "mode": "LastWriterWins",
              "conflictResolutionPath": "/_ts",
              "conflictResolutionProcedure": ""
            },
            "_rid": "V18LoLrv-qA=",
            "_self": "dbs/V18LoA==/colls/V18LoLrv-qA=/",
            "_etag": "\"00004400-0000-0700-0000-5f85351f0000\""
          }
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definición | Descripción | | --- || --- | | [Contenedor](#container) | Objeto de recurso de contenedor de SQL de Azure Cosmos DB | | [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. | | [ErrorResponse](#errorresponse) | Respuesta de error. | | [OperationType](#operationtype) | Enumeración que indica el tipo de operación del evento. | | [Recurso](#resource) | Recurso de un evento de contenedor de SQL de Azure Cosmos DB | | [RestorableSqlDatabaseGetResult](#restorablesqlcontainergetresult) | Evento de contenedor de SQL de Azure Cosmos DB | | [RestorableSqlDatabaseProperties](#restorablesqlcontainerproperties) | Propiedades de un evento de contenedor de SQL de Azure Cosmos DB | | [RestorableSqlDatabasesListResult](#restorablesqlcontainerslistresult) | Respuesta de operación de enumeración, que contiene los eventos de contenedor de SQL y sus propiedades. |

### <a name="container"></a><a id="container"></a>Contenedor

Objeto de recurso de contenedor de SQL de Azure Cosmos DB

| **Nombre** | **Tipo** | **Descripción** | | --- || --- | ---| | _etag |string| Propiedad generada por el sistema que representa el objeto etag de recursos necesaria para el control de simultaneidad optimista. | | _rid |string| Propiedad generada por el sistema. Identificador único. | | _self |cadena| Propiedad generada por el sistema que especifica la ruta de acceso direccionable del recurso de contenedor. | | _ts | | Propiedad generada por el sistema que denota la última marca de tiempo actualizada del recurso. | | ID |cadena| Nombre del contenedor de SQL de Azure Cosmos DB |

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
| Crear |string|evento de creación de contenedor|
| Eliminar |string|evento de eliminación de contenedor|
| Replace |string|evento de modificación de contenedor|
| SystemOperation |string|evento de modificación de contenedor desencadenado por el sistema. El usuario no inicia este evento.|

### <a name="resource"></a><a id="resource"></a>Resource

Recurso de un evento de contenedor de SQL de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| _rid |string| Propiedad generada por el sistema. Identificador único. |
| contenedor |[Contenedor](#container)| Objeto de recurso de contenedor de SQL de Azure Cosmos DB |
| eventTimestamp |string| Hora en la que se produjo este evento de contenedor. |
| operationType |[OperationType](#operationtype)| Tipo de operación de este evento de contenedor. |
| ownerId |string| Nombre del contenedor SQL. |
| ownerResourceId |string| Identificador de recurso del contenedor SQL. |

### <a name="restorablesqlcontainergetresult"></a><a id="restorablesqlcontainergetresult"></a>RestorableSqlContainerGetResult

Evento de contenedor de SQL de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | ---
| id |string| Identificador único del recurso de Azure Resource Manager. |
| name |string| Nombre del recurso de Azure Resource Manager. |
| properties |[RestorableSqlContainerProperties](#restorablesqlcontainerproperties)| Propiedades de un grupo de contenedor SQL. |
| type |string| Tipo de recurso de Azure. |

### <a name="restorablesqlcontainerproperties"></a><a id="restorablesqlcontainerproperties"></a>RestorableSqlContainerProperties

Propiedades de un evento de contenedor SQL de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| resource |[Recurso](#resource)| Recurso de un evento de contenedor de SQL de Azure Cosmos DB. |

### <a name="restorablesqlcontainerslistresult"></a><a id="restorablesqlcontainerslistresult"></a>RestorableSqlContainersListResult

Respuesta de la operación de enumeración, que contiene los eventos de contenedor de SQL y sus propiedades.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[RestorableSqlContainerGetResult](#restorablesqlcontainergetresult)[]| Lista de eventos de contenedor SQL y sus propiedades. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de las bases de datos restaurables](restorable-mongodb-databases-list.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.