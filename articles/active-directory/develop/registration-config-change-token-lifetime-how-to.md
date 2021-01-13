---
title: Cambio de los valores predeterminados de vigencia del token para aplicaciones personalizadas de Azure AD
description: Aprenda a actualizar las directivas de vigencia de los tokens para la aplicación que está desarrollando en Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2020
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ROBOTS: NOINDEX
ms.openlocfilehash: 5dec4958c3a1d955d8f8c1dce90e27696fbebfe5
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063355"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Modificación de los valores predeterminados de vigencia de los tokens en una aplicación personalizada

En este artículo se muestra cómo usar Azure AD PowerShell para establecer una directiva de vigencia del token de acceso. Azure AD Premium permite a los desarrolladores de aplicaciones y a los administradores de inquilinos configurar la vigencia de los tokens emitidos para clientes no confidenciales. Las directivas de vigencia de los tokens se establecen para todos los inquilinos o para los recursos a los que se va a acceder.

> [!IMPORTANT]
> Después de mayo de 2020, los inquilinos ya no podrán configurar la vigencia de los tokens de sesión y la actualización.  Azure Active Directory dejará de respetar la configuración existente de los tokens de sesión y la actualización en las directivas después del 30 de enero de 2021. Después, podrá seguir configurando la duración del token de acceso. Para obtener más información, consulte [Vigencia de tokens configurable en Azure AD](./active-directory-configurable-token-lifetimes.md).
> Se han implementado  [funciones de administración de sesiones de autenticación](../conditional-access/howto-conditional-access-session-lifetime.md) en el acceso condicional de Azure AD. Puede usar esta nueva característica para configurar la vigencia de los tokens de actualización mediante la configuración de la frecuencia de inicio de sesión.  

Para establecer una directiva de vigencia del token de acceso, debe descargar el [módulo de Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).
Ejecute el comando **Connect-AzureAD -Confirm**.

En este ejemplo, va a crear una directiva que requerirá que los usuarios se autentiquen con más frecuencia en la aplicación web. Esta Directiva establece la duración del acceso a la entidad de servicio de la aplicación web. Cree la directiva y asígnela a la entidad de servicio. También necesita obtener el valor de ObjectId de su entidad de servicio.

```powershell
$policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"

$sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Configurable token lifetimes in Azure AD](./active-directory-configurable-token-lifetimes.md) (Duraciones de tokens configurables) para aprender a configurar las duraciones de los tokens generadas por Azure AD, lo que incluye cómo establecer las duraciones de los tokens de todas las aplicaciones de su organización, de una aplicación multiinquilino o de una entidad de servicio concreta de su organización. 
* [Referencia de tokens de Azure AD](./id-tokens.md)
