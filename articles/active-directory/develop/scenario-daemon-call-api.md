---
title: Llamada a una API web desde una aplicación de demonio | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo crear una aplicación de demonio que llama a una API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bd0d53049c68843a6fd2cb6128c473d7c4f8d639
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582798"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Aplicación de demonio que llama a las API web: llamada a una API web desde la aplicación

Las aplicaciones de demonio de .NET pueden llamar a una API web. Las aplicaciones de demonio de .NET también pueden llamar a varias API web aprobadas previamente.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Llamada a una API web desde una aplicación de demonio

Aquí se muestra cómo usar el token para llamar a una API:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Llamada a varias API

Para las aplicaciones de demonio, las API web a las que llame deben estar aprobadas previamente. No hay ningún consentimiento incremental con las aplicaciones de demonio. (No hay interacción del usuario). El administrador de inquilinos debe dar el consentimiento previo a la aplicación y a todos los permisos de la API. Si quiere llamar a varias API, adquiera un token para cada recurso, cada vez llamando a `AcquireTokenForClient`. MSAL usará la caché de tokens de aplicación para evitar las llamadas de servicio innecesarias.

## <a name="next-steps"></a>Pasos siguientes

# <a name="net"></a>[.NET](#tab/dotnet)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=dotnet).

# <a name="python"></a>[Python](#tab/python)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=python).

# <a name="java"></a>[Java](#tab/java)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=java).

---
