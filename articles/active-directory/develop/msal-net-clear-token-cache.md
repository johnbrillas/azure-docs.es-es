---
title: Borrado de la caché de tokens (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a borrar la caché del token usando Microsoft Authentication Library para .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 3049a1213b8b92153fc0fce96b2dadace01a4ca8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98064766"
---
# <a name="clear-the-token-cache-using-msalnet"></a>Borrado de la caché de tokens mediante MSAL.NET

Al [adquirir un token de acceso](msal-acquire-cache-tokens.md) mediante la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET), el token se almacena en caché. Cuando la aplicación necesita un token, primero debe llamar al método `AcquireTokenSilent` para comprobar si hay un token aceptable en la caché. 

Para borrar la caché, es necesario quitar las cuentas de la caché. No obstante, esto no elimina la cookie de sesión que se encuentra en el explorador.  En el ejemplo siguiente se crea una instancia de una aplicación cliente pública, se obtienen las cuentas de la aplicación y se quitan las cuentas.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Para obtener más información sobre la adquisición y el almacenamiento en caché de los tokens, vea [Adquirir un token de acceso](msal-acquire-cache-tokens.md).
