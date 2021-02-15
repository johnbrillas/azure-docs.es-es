---
title: Enumeración de cuentas de base de datos restaurables por ubicación mediante la API REST de Azure Cosmos DB
description: Enumera todas las cuentas de base de datos de Azure Cosmos DB restaurables disponibles en la suscripción y en una región.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 26c07ddaf4db71961d4aeff50e482740d969f0ef
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537685"
---
# <a name="list-restorable-database-accounts-by-location-using-azure-cosmos-db-rest-api"></a>Enumeración de cuentas de base de datos restaurables por ubicación mediante la API REST de Azure Cosmos DB

> [!IMPORTANT]
> La característica de restauración a un momento dado (modo de copia de seguridad continua) de Azure Cosmos DB está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Enumera todas las cuentas de base de datos restaurables de Azure Cosmos DB restaurables disponibles en la suscripción y en una región. Esta llamada requiere el permiso `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/read`.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts?api-version=2020-06-01-preview

```

## <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | En | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| **ubicación** | path | True | string| Región de Azure Cosmos DB, con espacios entre palabras y cada palabra en mayúsculas. |
| **subscriptionId** | path | True | string| Identificador de la suscripción de destino. |
| **api-version** | Query | True | string | Versión de API que se usará para esta operación. |

## <a name="responses"></a>Respuestas

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| 200 OK | [RestorableDatabaseAccountsListResult](#restorabledatabaseaccountslistresult)| La operación se ha completado correctamente. |
| Otros códigos de estado | [DefaultErrorResponse](#defaulterrorresponse)| Respuesta de error que describe el motivo del error de la operación. |

## <a name="examples"></a>Ejemplos

### <a name="cosmosdbdatabaseaccountlist"></a>CosmosDBDatabaseAccountList

**Solicitud de ejemplo**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts?api-version=2020-06-01-preview
```

**Respuesta de muestra**

Código de estado: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d",
      "name": "d9b26648-2f53-4541-b3d8-3044f4f9810d",
      "location": "West US",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts",
      "properties": {
        "accountName": "ddb1",
        "creationTime": "2020-04-11T21:56:15Z",
        "deletionTime": "2020-06-12T22:05:09Z",
        "apiType": "Sql",
        "restorableLocations": [
          {
            "locationName": "South Central US",
            "regionalDatabaseAccountInstanceId": "d7a01f78-606f-45c6-9dac-0df32f433bb5",
            "creationTime": "2020-10-30T21:13:10Z",
            "deletionTime": "2020-10-30T21:13:35Z"
          },
          {
            "locationName": "West US",
            "regionalDatabaseAccountInstanceId": "fdb43d84-1572-4697-b6e7-2bcda0c51b2c",
            "creationTime": "2020-10-30T21:13:10Z"
          }
        ]
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/4f9e6ace-ac7a-446c-98bc-194c502a06b4",
      "name": "4f9e6ace-ac7a-446c-98bc-194c502a06b4",
      "location": "West US",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts",
      "properties": {
        "accountName": "ddb2",
        "creationTime": "2020-05-01T08:05:18Z",
        "apiType": "Sql",
        "restorableLocations": [
          {
            "locationName": "South Central US",
            "regionalDatabaseAccountInstanceId": "d7a01f78-606f-45c6-9dac-0df32f433bb5",
            "creationTime": "2020-10-30T21:13:10Z",
            "deletionTime": "2020-10-30T21:13:35Z"
          },
          {
            "locationName": "West US",
            "regionalDatabaseAccountInstanceId": "fdb43d84-1572-4697-b6e7-2bcda0c51b2c",
            "creationTime": "2020-10-30T21:13:10Z"
          }
        ]
      }
    }
  ]
}
```

## <a name="definitions"></a>Definiciones

|Definición  | Descripción|
| --- | --- |
| [ApiType](#apitype) | Enumeración para indicar el tipo de API de la cuenta de base de datos restaurable. |
| [DefaultErrorResponse](#defaulterrorresponse) | Respuesta de error del servicio. |
| [ErrorResponse](#errorresponse) | Respuesta de error. |
| [RestorableDatabaseAccountGetResult](#restorabledatabaseaccountgetresult) | Cuenta de base de datos restaurable de Azure Cosmos DB. |
| [RestorableDatabaseAccountsListResult](#restorabledatabaseaccountslistresult) | Respuesta de la operación de enumeración, que contiene las cuentas de base de datos restaurables y sus propiedades. |
| [RestorableLocationResource](#restorablelocationresource) | Propiedades de la cuenta de restauración regional. |

### <a name="apitype"></a><a id="apitype"></a>ApiType

Enumeración para indicar el tipo de API de la cuenta de base de datos restaurable.

| **Nombre** | **Tipo** |
| --- | --- |
| Cassandra |string|
| Gremlin |string|
| GremlinV2 |string|
| MongoDB |string|
| Sql |string|
| Tabla |string|

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

### <a name="restorabledatabaseaccountgetresult"></a><a id="restorabledatabaseaccountgetresult"></a>RestorableDatabaseAccountGetResult

Cuenta de base de datos restaurable de Azure Cosmos DB.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| id |string| Identificador único del recurso de Azure Resource Manager. |
| ubicación |string| Ubicación del grupo de recursos al que pertenece el recurso. |
| name |string| Nombre del recurso de Resource Manager. |
| properties.accountName |string| Nombre de la cuenta de base de datos global. |
| properties.apiType |[ApiType](#apitype)| Tipo de API de la cuenta de base de datos restaurable. |
| properties.creationTime |string| Hora de creación de la cuenta de base de datos restaurable (formato ISO-8601). |
| properties.deletionTime |string| La hora a la que se eliminó la cuenta de base de datos restaurable (formato ISO-8601). |
| properties.restorableLocations |[RestorableLocationResource](#restorablelocationresource)[]| Lista de regiones desde donde se puede restaurar la cuenta de base de datos. |
| type |string| Tipo de recurso de Azure. |

### <a name="restorabledatabaseaccountslistresult"></a><a id="restorabledatabaseaccountslistresult"></a>RestorableDatabaseAccountsListResult

Respuesta de la operación de enumeración, que contiene las cuentas de base de datos restaurables y sus propiedades.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| valor |[RestorableDatabaseAccountGetResult](#restorabledatabaseaccountgetresult)[]| Lista de cuentas de base de datos restaurables y sus propiedades. |

### <a name="restorablelocationresource"></a><a id="restorablelocationresource"></a>RestorableLocationResource

Propiedades de la cuenta de restauración regional.

| **Nombre** | **Tipo** | **Descripción** |
| --- | --- | --- |
| creationTime |string| Hora de creación de la cuenta de base de datos restaurable regional (formato ISO-8601). |
| deletionTime |string| La hora a la que se eliminó la cuenta de base de datos restaurable regional (formato ISO-8601). |
| locationName |string| Ubicación de la cuenta de restauración regional. |
| regionalDatabaseAccountInstanceId |string| Identificador de instancia de la cuenta restaurable regional. |

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de las colecciones restaurables](restorable-database-accounts-list-by-location.md) de la API de Azure Cosmos DB para MongoDB mediante la API REST.
* [Modelo de recursos](continuous-backup-restore-resource-model.md) del modo de copia de seguridad continua.