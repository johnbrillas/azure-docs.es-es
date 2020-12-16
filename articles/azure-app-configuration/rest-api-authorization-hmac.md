---
title: 'API REST de Azure App Configuration: autorización de HMAC'
description: Usar HMAC para la autorización en Azure App Configuration mediante la API REST
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 3746fcf06aea48c9c80696b634d6323b9cb21822
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96932666"
---
# <a name="hmac-authorization---rest-api-reference"></a>Autorización de HMAC: referencia de API REST

Cuando se utiliza la autenticación HMAC, las operaciones se encuentran en una de estas dos categorías: lectura o escritura. Las claves de acceso de lectura y escritura conceden permiso para llamar a todas las operaciones. Las claves de acceso de solo lectura conceden permiso para llamar a las operaciones de solo lectura. El hecho de que una clave de acceso sea de solo lectura o de lectura y escritura viene determinado por su propiedad `readOnly`. Cualquier intento de hacer una solicitud de escritura con una clave de acceso de solo lectura dará lugar a que la solicitud no sea autorizada.

## <a name="obtaining-access-keys"></a>Obtención de teclas de acceso

La especificación que describe las claves de acceso y la API usada para obtenerlas se detalla en el proveedor de recursos Azure App Configuration especificado [aquí](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/appconfiguration/resource-manager/Microsoft.AppConfiguration/stable/2019-10-01/appconfiguration.json). Las claves de acceso se obtienen a través de la operación "ConfigurationStores_ListKeys".

## <a name="errors"></a>Errors

```http
HTTP/1.1 403 Forbidden
```

**Motivo:** La clave de acceso usada para autenticar la solicitud no proporciona los permisos necesarios para realizar la operación solicitada.
**Solución:** Obtener una clave de acceso que proporcione permiso para realizar la operación solicitada y utilizarla para autenticar la solicitud.
