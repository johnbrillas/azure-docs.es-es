---
title: Obtención de consentimiento para varios recursos (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo un usuario puede recibir consentimiento previo para varios recursos con la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: d22b4109cdcdc965b1b2e03aba592022c58f773b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99583797"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>El usuario obtiene consentimiento para varios recursos con MSAL.NET
La Plataforma de identidad de Microsoft no le permitirá obtener un token para varios recursos a la vez. Cuando use la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET), el parámetro de ámbitos del método de adquisición de token solo debe contener ámbitos para un recurso único. Sin embargo, puede dar su consentimiento para varios recursos por adelantado si especifica otros ámbitos con el método de generador de `.WithExtraScopeToConsent`.

> [!NOTE]
> Obtener el consentimiento para varios recursos funciona para la plataforma de identidad de Microsoft, pero no para Azure AD B2C. Azure AD B2C solo admite el consentimiento del administrador, no el consentimiento del usuario.

Por ejemplo, si tiene dos recursos que tienen dos ámbitos cada uno:

- https:\//mytenant.onmicrosoft.com/customerapi (con 2 ámbitos, `customer.read` y `customer.write`)
- https:\//mytenant.onmicrosoft.com/vendorapi (con 2 ámbitos, `vendor.read` y `vendor.write`)

Debe usar el modificador `.WithExtraScopeToConsent` que tiene el parámetro *extraScopesToConsent*, tal como se muestra en el ejemplo siguiente:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Con esto, obtendrá un token de acceso para la primera API web. Luego, para acceder a la segunda API web, podrá adquirir silenciosamente el token desde la caché de tokens:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
