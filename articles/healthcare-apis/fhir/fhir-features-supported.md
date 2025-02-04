---
title: 'Características de FHIR admitidas en Azure: Azure API for FHIR'
description: En este artículo se explica qué características de la especificación de FHIR se implementan en Azure API for FHIR
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 1/30/2021
ms.author: cavoeg
ms.openlocfilehash: 9bd61d65d6d64dac6081d3491deb8a15efc4a45b
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105048426"
---
# <a name="features"></a>Características

Azure API for FHIR proporciona una implementación totalmente administrada Microsoft FHIR Server para Azure. El servidor es una implementación del estándar [FHIR](https://hl7.org/fhir). En este documento se enumeran las características principales de FHIR Server.

## <a name="fhir-version"></a>Versión de FHIR

Versión más reciente admitida: `4.0.1`

Entre las versiones anteriores también admitidas actualmente se incluye: `3.0.2`

## <a name="rest-api"></a>API DE REST

| API                            | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| leer                           | Sí       | Sí       | Sí       |                                                     |
| vread                          | Sí       | Sí       | Sí       |                                                     |
| update                         | Sí       | Sí       | Sí       |                                                     |
| update with optimistic locking | Sí       | Sí       | Sí       |                                                     |
| update (conditional)           | Sí       | Sí       | Sí       |                                                     |
| patch                          | No        | No        | No        |                                                     |
| delete                         | Sí       | Sí       | Sí       |  Vea la nota siguiente.                                   |
| delete (conditional)           | No        | No        | No        |                                                     |
| history                        | Sí       | Sí       | Sí       |                                                     |
| create                         | Sí       | Sí       | Sí       | Admite POST y PUT                               |
| create (conditional)           | Sí       | Sí       | Sí       | Problema [n.º 1382](https://github.com/microsoft/fhir-server/issues/1382) |
| búsqueda                         | Parcial   | Parcial   | Parcial   | Vea la sección de búsqueda a continuación.                           |
| búsqueda encadenada                 | Sí       | Sí       | Parcial   | Vea la nota 2 a continuación.                                   |
| búsqueda encadenada inversa         | Sí       | Sí       | Parcial   | Vea la nota 2 a continuación.                                   |
| capabilities                   | Sí       | Sí       | Sí       |                                                     |
| proceso por lotes                          | Sí       | Sí       | Sí       |                                                     |
| transaction                    | No        | Sí       | No        |                                                     |
| paging                         | Parcial   | Parcial   | Parcial   | `self` y `next` se admiten                     |
| intermediaries                 | No        | No        | No        |                                                     |

> [!Note]
> La eliminación definida por la especificación FHIR requiere que, después de la eliminación, las lecturas posteriores no específicas de la versión de un recurso devuelvan un código de estado HTTP 410 y que el recurso ya no se encuentre en la búsqueda. La API de Azure para FHIR también le permite eliminar completamente (incluido todo el historial) el recurso. Para eliminar completamente el recurso, puede pasar una configuración de parámetros `hardDelete` a true (`DELETE {server}/{resource}/{id}?hardDelete=true`). Si no pasa este parámetro o establece `hardDelete` en false, las versiones históricas del recurso seguirán estando disponibles.


 **Nota 2**
* Agrega compatibilidad con MVP para la búsqueda encadenada y encadenada de FHIR en CosmosDB. 

  En la API de Azure para FHIR y el servidor FHIR de código abierto con el respaldo de Cosmos, la búsqueda encadenada y la búsqueda encadenada inversa es una implementación de MVP. Para realizar una búsqueda encadenada en Cosmos DB, la implementación recorre la expresión de búsqueda y emite subconsultas para resolver los recursos coincidentes. Esto se hace para cada nivel de la expresión. Si alguna consulta devuelve más de 100 resultados, se producirá un error. De forma predeterminada, la búsqueda encadenada está detrás de una marca de características. Para usar la búsqueda encadenada en Cosmos DB, use el encabezado `x-ms-enable-chained-search: true` . Para obtener más información, consulte [PR 1695](https://github.com/microsoft/fhir-server/pull/1695).

## <a name="search"></a>Search

Se admiten todos los tipos de parámetro de búsqueda. 

| Tipo de parámetro de búsqueda | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario |
|-----------------------|-----------|-----------|-----------|---------|
| Number                | Sí       | Sí       | Sí       |         |
| Date/DateTime         | Sí       | Sí       | Sí       |         |
| String                | Sí       | Sí       | Sí       |         |
| Token                 | Sí       | Sí       | Sí       |         |
| Referencia             | Sí       | Sí       | Sí       |         |
| Compuesto             | Sí       | Sí       | Sí       |         |
| Cantidad              | Sí       | Sí       | Sí       |         |
| URI                   | Sí       | Sí       | Sí       |         |
| Especial               | No        | No        | No        |         |


| Modificadores             | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | Sí       | Sí       | Sí       |         |
|`:exact`               | Sí       | Sí       | Sí       |         |
|`:contains`            | Sí       | Sí       | Sí       |         |
|`:text`                | Sí       | Sí       | Sí       |         |
|`:[type]` (referencia)  | Sí       | Sí       | Sí       |         |
|`:not`                 | Sí       | Sí       | Sí       |         |
|`:below` (uri)         | Sí       | Sí       | Sí       |         |
|`:above` (uri)         | No        | No        | No        | Problema [#158](https://github.com/Microsoft/fhir-server/issues/158) |
|`:in` (token)          | No        | No        | No        |         |
|`:below` (token)       | No        | No        | No        |         |
|`:above` (token)       | No        | No        | No        |         |
|`:not-in` (token)      | No        | No        | No        |         |

| Parámetro de búsqueda común | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Sí       | Sí       | Sí       |         |
| `_lastUpdated`          | Sí       | Sí       | Sí       |         |
| `_tag`                  | Sí       | Sí       | Sí       |         |
| `_list`                 | Sí       | Sí       | Sí       |         |
| `_type`                 | Sí       | Sí       | Sí       | Problema [n.º 1562](https://github.com/microsoft/fhir-server/issues/1562)        |
| `_security`             | Sí       | Sí       | Sí       |         |
| `_profile`              | Parcial   | Parcial   | Parcial   | Compatible con STU3. Si creó la base de datos **después** del 20 de febrero de 2021, también tendrá compatibilidad en R4. Estamos trabajando para habilitar _profile en las bases de datos creadas antes del 20 de febrero de 2021. |
| `_text`                 | No        | No        | No        |         |
| `_content`              | No        | No        | No        |         |
| `_has`                  | No        | No        | No        |         |
| `_query`                | No        | No        | No        |         |
| `_filter`               | No        | No        | No        |         |

| Parámetros de resultados de la búsqueda | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario |
|-------------------------|-----------|-----------|-----------|---------|
| `_elements`             | Sí       | Sí       | Sí       | Problema [n.º 1256](https://github.com/microsoft/fhir-server/issues/1256)        |
| `_count`                | Sí       | Sí       | Sí       | `_count` tiene un límite de 1000 caracteres. Si se establece en un valor superior a 1000, solo se devolverán 1000 y una advertencia en el conjunto. |
| `_include`              | Sí       | Sí       | Sí       |Los elementos incluidos se limitan a 100. La inclusión en PaaS y OSS de Cosmos DB no incluye compatibilidad con :iterate.|
| `_revinclude`           | Sí       | Sí       | Sí       | Los elementos incluidos se limitan a 100. La inclusión en PaaS y OSS de Cosmos DB [no incluye compatibilidad con :iterate](https://github.com/microsoft/fhir-server/issues/1313). Problema [n.º 1319](https://github.com/microsoft/fhir-server/issues/1319)|
| `_summary`              | Parcial   | Parcial   | Parcial   | `_summary=count` se admite |
| `_total`                | Parcial   | Parcial   | Parcial   | `_total=none` y `_total=accurate`      |
| `_sort`                 | Parcial   | Parcial   | Parcial   |   `_sort=_lastUpdated` se admite       |
| `_contained`            | No        | No        | No        |         |
| `containedType`         | No        | No        | No        |         |
| `_score`                | No        | No        | No        |         |

## <a name="extended-operations"></a>Operaciones extendidas

Todas las operaciones que se admiten y que extienden la API RESTful.

| Tipo de parámetro de búsqueda | Admitida: PaaS | Admitida: OSS (SQL) | Admitida: OSS (Cosmos DB) | Comentario |
|------------------------|-----------|-----------|-----------|---------|
| $export (todo el sistema) | Sí       | Sí       | Sí       |         |
| Patient/$export        | Sí       | Sí       | Sí       |         |
| Group/$export          | Sí       | Sí       | Sí       |         |
| $convert-data          | Sí       | Sí       | Sí       |         |


## <a name="persistence"></a>Persistencia

Microsoft FHIR Server tiene un módulo de persistencia conectable (consulte [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Actualmente, el código fuente abierto de FHIR Server incluye una implementación para [Azure Cosmos DB](../../cosmos-db/index-overview.md) y [SQL Database](https://azure.microsoft.com/services/sql-database/).

Cosmos DB es una base de datos (SQL API, MongoDB API, etc.) de varios modelos distribuida de forma global. Admite diferentes [niveles de coherencia](../../cosmos-db/consistency-levels.md). La plantilla de implementación predeterminada configura FHIR Server con una coherencia de `Strong`, pero la directiva de coherencia puede modificarse (generalmente de forma más relajada) en función de cada solicitud mediante el encabezado de solicitud `x-ms-consistency-level`.

## <a name="role-based-access-control"></a>Control de acceso basado en rol

FHIR Server utiliza [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) para el control de acceso. En concreto, se aplica el control de acceso basado en roles (RBAC) si el parámetro de configuración `FhirServer:Security:Enabled` se establece en `true`, y todas las solicitudes (excepto `/metadata`) a FHIR Server deben tener el encabezado de solicitud `Authorization` establecido en `Bearer <TOKEN>`. El token debe contener uno o varios roles, tal como se define en la notificación `roles`. Se permitirá una solicitud si el token contiene un rol que permite la acción especificada en el recurso especificado.

Actualmente, las acciones permitidas para un rol determinado se aplican *globalmente* en la API.

## <a name="service-limits"></a>Límites de servicio

* [**Unidades de solicitud (RU)**](../../cosmos-db/concepts-limits.md): puede configurar hasta 10 000 unidades de solicitud en el portal de Azure API for FHIR. Necesitará un mínimo de 400 RU o 10 RU/GB, lo que sea mayor. Si necesita más de 10 000 RU, puede crear una incidencia de soporte técnico para solicitar un aumento. El máximo disponible es 1 000 000.

* **Conexiones simultáneas** e **instancias**: de manera predeterminada, tiene cinco conexiones simultáneas en dos instancias del clúster (para un total de diez solicitudes simultáneas). Si cree que necesita un mayor número solicitudes simultáneas, abra una incidencia de soporte técnico con los detalles de lo que necesita.

* **Tamaño de conjunto**: cada conjunto está limitado a 500 elementos.

* **Tamaño de los datos**: los datos y documentos deben ser ligeramente inferiores a 2 MB.

## <a name="performance-expectations"></a>Expectativas de rendimiento

El rendimiento del sistema depende del número de RU, las conexiones simultáneas y el tipo de operaciones que se van a llevar a cabo (PUT, POST, etc.). A continuación se muestran algunos intervalos generales de lo que puede esperar en función del número de RU configurado. En general, el rendimiento se escala de manera lineal con un aumento en RU:

| N.º de RU | Recursos/segundo |    Almacenamiento máximo (GB)*    |
|----------|---------------|--------|                 
| 400      | 5-10          |     40   |
| 1,000    | 100-150       |      100  |
| 10 000   | 225-400       |      1,000  |
| 100 000  | 2500-4000   |      10 000  |

Nota: Para Cosmos DB, existe un requisito de un rendimiento mínimo de 10 RU/s por GB de almacenamiento. Para obtener más información, consulte [Cuotas de servicio de Cosmos DB](../../cosmos-db/concepts-limits.md).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha leído sobre las características admitidas de FHIR en Azure API for FHIR. Después, implemente Azure API for FHIR.
 
>[!div class="nextstepaction"]
>[Implementación de Azure API for FHIR](fhir-paas-portal-quickstart.md)
