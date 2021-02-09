---
title: Azure Cache for Redis como origen de Event Grid
description: Describe las propiedades que se proporcionan para los eventos de Azure Cache for Redis con Azure Event Grid.
ms.topic: conceptual
ms.date: 12/21/2020
author: curib
ms.author: cauribeg
ms.openlocfilehash: f446f3f469a7404e6e74ba67ee24bf32578fe9d8
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99056930"
---
# <a name="azure-cache-for-redis-as-an-event-grid-source"></a>Azure Cache for Redis como un origen de Event Grid

En este artículo se proporcionan las propiedades y los esquemas de los eventos de Azure Cache for Redis.  Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md). 

## <a name="event-grid-event-schema"></a>Esquema de eventos de Event Grid

### <a name="list-of-events-for-azure-cache-for-redis-rest-apis"></a>Lista de eventos para las API REST de Azure Cache for Redis

Estos eventos se desencadenan cuando un cliente exporta, importa o escala mediante una llamada a las API REST de Azure Cache for Redis. La actualización de Redis desencadena el evento de revisión.

 |Nombre del evento |Descripción|
 |----------|-----------|
 |**Microsoft.Cache.ExportRDBCompleted** |Se desencadena cuando se exportan los datos de la memoria caché. |
 |**Microsoft.Cache.ImportRDBCompleted** |Se desencadena cuando se importan los datos de la memoria caché. |
 |**Microsoft.Cache.PatchingCompleted** |Se desencadena cuando se completa la revisión. |
 |**Microsoft.Cache.ScalingCompleted** |Se desencadena cuando se completa el escalado. |

<a name="example-event"></a>
### <a name="the-contents-of-an-event-response"></a>El contenido de una respuesta de evento

Cuando se desencadena un evento, el servicio Event Grid envía datos sobre ese evento al punto de conexión correspondiente.

Esta sección contiene un ejemplo del aspecto que deben tener los datos para cada evento de Azure Cache for Redis.

### <a name="microsoftcachepatchingcompleted-event"></a>Evento Microsoft.Cache.PatchingCompleted

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.PatchingCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"PatchingCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"PatchingCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcacheimportrdbcompleted-event"></a>Evento Microsoft.Cache.ImportRDBCompleted

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ImportRDBCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ImportRDBCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ImportRDBCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcacheexportrdbcompleted-event"></a>Evento Microsoft.Cache.ExportRDBCompleted

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ExportRDBCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ExportRDBCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ExportRDBCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcachescalingcompleted"></a>Microsoft.Cache.ScalingCompleted

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ScalingCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ScalingCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ScalingCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| datos | object | Datos de evento de Azure Cache for Redis. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto data tiene las siguientes propiedades:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| timestamp | string | La hora a la que se produjo el evento. |
| name | string | Nombre del evento. |
| status | string | Estado del evento. Erróneo o correcto. |


## <a name="quickstarts"></a>Guías de inicio rápido

Si quiere probar eventos de Azure Cache for Redis, consulte cualquiera de estos artículos de inicio rápido:

|Si desea utilizar esta herramienta:    |Consulte este artículo: |
|--|-|
|Azure portal    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con Azure Portal](../azure-cache-for-redis/cache-event-grid-quickstart-portal.md)|
|PowerShell    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web con PowerShell](../azure-cache-for-redis/cache-event-grid-quickstart-powershell.md)|
|Azure CLI    |[Inicio rápido: Enrutamiento de eventos de Azure Cache for Redis al punto de conexión web mediante la CLI de Azure](../azure-cache-for-redis/cache-event-grid-quickstart-cli.md)|

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).

