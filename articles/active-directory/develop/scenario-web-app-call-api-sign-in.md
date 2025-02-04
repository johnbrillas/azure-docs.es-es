---
title: Eliminación de cuentas de la memoria caché de tokens durante el cierre de sesión | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo quitar una cuenta de la memoria caché de tokens en el cierre de sesión
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9fc271dfa9edbedac8527009dd2b2180b7c5e7cd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98756253"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Aplicación web que llama a API web: Eliminación de cuentas de la caché de tokens durante el cierre de sesión global

Ha aprendido cómo agregar el inicio de sesión a la aplicación web en [Aplicación web que inicia sesión de usuarios: Inicio y cierre de sesión](scenario-web-app-sign-user-sign-in.md).

El cierre de sesión es diferente en el caso de una aplicación web que llama a API web. Cuando el usuario cierra la sesión en la aplicación, o en cualquier otra aplicación, debe quitar los tokens asociados a ese usuario de la caché de tokens.

## <a name="intercept-the-callback-after-single-sign-out"></a>Interceptación de la devolución de llamada tras un cierre de sesión único

Para borrar la entrada de la caché de tokens asociada a la cuenta que ha cerrado la sesión, la aplicación puede interceptar el evento posterior a `logout`. Las aplicaciones web almacenan tokens de acceso de cada usuario en una caché de tokens. Al interceptar la devolución de llamada posterior a `logout`, la aplicación web puede quitar el usuario de la caché.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Microsoft.Identity.Web se encarga de implementar el cierre de sesión automáticamente. Para obtener información detallada, consulte [Código fuente de Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web/blob/c29f1a7950b940208440bebf0bcb524a7d6bee22/src/Microsoft.Identity.Web/WebAppExtensions/WebAppCallsWebApiAuthenticationBuilderExtensions.cs#L168-L176).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

El ejemplo de ASP.NET no quita las cuentas de la caché durante el cierre de sesión global.

# <a name="java"></a>[Java](#tab/java)

El ejemplo de Java no quita las cuentas de la caché durante el cierre de sesión global.

# <a name="python"></a>[Python](#tab/python)

El ejemplo de Python no quita las cuentas de la caché durante el cierre de sesión global.

---

## <a name="next-steps"></a>Pasos siguientes

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación web](./scenario-web-app-call-api-acquire-token.md?tabs=aspnetcore).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación web](./scenario-web-app-call-api-acquire-token.md?tabs=aspnet).

# <a name="java"></a>[Java](#tab/java)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación web](./scenario-web-app-call-api-acquire-token.md?tabs=java).

# <a name="python"></a>[Python](#tab/python)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación web](./scenario-web-app-call-api-acquire-token.md?tabs=python).

---