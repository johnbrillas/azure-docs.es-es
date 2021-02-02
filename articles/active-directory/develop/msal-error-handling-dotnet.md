---
title: Control de errores y excepciones en MSAL.NET
titleSuffix: Microsoft identity platform
description: Aprenda a controlar errores y excepciones, desafíos de las notificaciones del acceso condicional y reintentos en las aplicaciones de MSAL.NET.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 381416384cacd44bdb1b08801f7b3174c9504d0b
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761189"
---
# <a name="handle-errors-and-exceptions-in-msalnet"></a>Control de errores y excepciones en MSAL.NET

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msalnet"></a>Control de errores en MSAL.NET

Al procesar las excepciones de .NET, puede usar el propio tipo de excepción y el miembro `ErrorCode` para distinguir entre las excepciones. Los valores de `ErrorCode` son constantes del tipo [MsalError](/dotnet/api/microsoft.identity.client.msalerror).

También puede echar un vistazo a los campos de [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) y [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception).

Si se produce un error [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception), revise los [códigos de error de autenticación y autorización](reference-aadsts-error-codes.md) para ver si el código aparece aquí.

### <a name="common-net-exceptions"></a>Excepciones .NET comunes

Estas son las excepciones comunes que pueden producirse y algunas de las posibles mitigaciones:  

| Excepción | Código de error | Mitigación|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception) | AADSTS65001: The user or administrator has not consented to use the application with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource (Ni el usuario ni el administrador han dado consentimiento para usar la aplicación con ID "{appId}" llamada "{appName}". Envíe una solicitud de autorización interactiva para este usuario y este recurso).| Debe obtener primero el consentimiento del usuario. Si no utiliza .NET Core (que no tiene interfaz de usuario web), llame a `AcquireTokeninteractive` (solo una vez). Si lo utiliza o si no quiere realizar una operación `AcquireTokenInteractive`, el usuario puede ir a una dirección URL para dar su consentimiento: `https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read`. para llamar a `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception) | AADSTS50079: The user is required to use [multi-factor authentication (MFA)](../authentication/concept-mfa-howitworks.md) (Es necesario que el usuario utilice la autenticación multifactor [MFA]).| No hay mitigación. Si la autenticación multifactor está configurada para el inquilino y Azure Active Directory (AAD) decide aplicarla, deberá recurrir a un flujo interactivo, como `AcquireTokenInteractive` o `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) |AADSTS90010: The grant type isn't supported over the */common* or */consumers* endpoints. Use the */organizations* or tenant-specific endpoint. You used */common*. (El tipo de concesión no es compatible con los puntos de conexión /common o /consumers. Use /organizations o el punto de conexión específico del inquilino. Usó /common).| Como se ha explicado en el mensaje de Azure AD, la autoridad debe tener un inquilino o, en caso contrario, */organizations*.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) | AADSTS70002: El cuerpo de la solicitud debe contener el siguiente parámetro: `client_secret or client_assertion`.| Esta excepción puede devolverse si la aplicación no estaba registrada como aplicación cliente pública en Azure AD. En Azure Portal, edite el manifiesto de la aplicación y establezca `allowPublicClient` en `true`. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception)| `unknown_user Message`: Could not identify logged in user (No se pudo identificar al usuario que inició sesión).| La biblioteca no pudo consultar el usuario actual de Windows que ha iniciado sesión o este usuario no está unido a AD o Azure AD (los usuarios unidos a un lugar de trabajo no son compatibles). Mitigación 1: en UWP, compruebe que la aplicación tiene las siguientes funcionalidades: Autenticación empresarial, Redes privadas (cliente y servidor), Información de la cuenta de usuario. Mitigación2: Implemente su propia lógica para capturar el nombre de usuario (por ejemplo, john@contoso.com) y use el formulario `AcquireTokenByIntegratedWindowsAuth` que toma el nombre de usuario.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception)|integrated_windows_auth_not_supported_managed_user| Este método se basa en un protocolo que Active Directory (AD) expone. Si un usuario se creó en Azure AD sin respaldo de AD (usuario "administrado"), se producirá un error en este método. Los usuarios creados en AD y con respaldo de Azure AD (usuarios "federados") pueden beneficiarse de este método de autenticación no interactiva. Mitigación: Use la autenticación interactiva.|

### `MsalUiRequiredException`

Uno de los códigos de estado comunes que devuelve MSAL.NET al llamar a `AcquireTokenSilent()` es `MsalError.InvalidGrantError`. Este código de estado implica que la aplicación debe volver a llamar a la biblioteca de autenticación, aunque en modo interactivo (AcquireTokenInteractive o AcquireTokenByDeviceCodeFlow para las aplicaciones cliente públicas, tienen un desafío en las aplicaciones web). Esto se debe a que se requiere interacción adicional del usuario para emitir el token de autenticación.

La mayoría de las veces en que `AcquireTokenSilent` produce un error, se debe a que la memoria caché del token no tiene tokens que coincidan con la solicitud. Los tokens de acceso expiran en 1 hora y `AcquireTokenSilent` intentará capturar uno nuevo en función de un token de actualización (en términos de OAuth2, este es el flujo de "actualización de token"). Este flujo también puede producir un error por diversos motivos; por ejemplo, si un administrador de inquilinos configura directivas de inicio de sesión más rigurosas. 

La interacción tiene el objetivo de hacer que el usuario realice una acción. Algunas de esas condiciones son fáciles de solucionar para los usuarios (por ejemplo, aceptar las condiciones de uso con un solo clic) y otras no se pueden solucionar con la configuración actual (por ejemplo, el equipo en cuestión necesita conectarse a una red corporativa específica). Algunas ayudan al usuario a configurar la autenticación multifactor o a instalar Microsoft Authenticator en su dispositivo.

### <a name="msaluirequiredexception-classification-enumeration"></a>Enumeración de la clasificación `MsalUiRequiredException`

MSAL expone un campo de `Classification`, que puede leer para proporcionar una mejor experiencia de usuario. Por ejemplo, para indicar al usuario que su contraseña expiró o que necesitará dar su consentimiento para usar ciertos recursos. Los valores admitidos son parte de la enumeración `UiRequiredExceptionClassification`:

| clasificación    | Significado           | Uso recomendado |
|-------------------|-------------------|----------------------|
| BasicAction | La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a AcquireTokenInteractively(). |
| AdditionalAction | La interacción de corrección con el sistema puede resolver la condición, independientemente del flujo de autenticación interactiva. | Llame a AcquireTokenInteractively() para mostrar un mensaje que explique la acción correctiva. La aplicación que realiza la llamada puede ocultar los flujos que requieren acciones adicionales si es poco probable que el usuario complete la acción correctiva. |
| MessageOnly      | No se puede resolver la condición en este momento. Al iniciar el flujo de autenticación interactiva, se mostrará un mensaje que explica la condición. | Llame a AcquireTokenInteractively() para mostrar un mensaje que explique la condición. AcquireTokenInteractively() devolverá el error UserCanceled después de que el usuario lea el mensaje y cierre la ventana. La aplicación que realiza la llamada puede optar por ocultar los flujos que dan como resultado solo un mensaje si es poco probable que el usuario se beneficie del mensaje.|
| ConsentRequired  | Falta el consentimiento del usuario o se ha revocado. | Llame a AcquireTokenInteractively() para que el usuario dé su consentimiento. |
| UserPasswordExpired | La contraseña del usuario ha expirado. | Llame a AcquireTokenInteractively() para que el usuario pueda restablecer su contraseña. |
| PromptNeverFailed| Se llamó a la autenticación interactiva con el parámetro prompt=never; esto obliga a MSAL a basarse en las cookies del explorador y no mostrar el explorador. Se produjo un error. | Llame a AcquireTokenInteractively() sin Prompt.None. |
| AcquireTokenSilentFailed | El SDK de MSAL no tiene suficiente información para capturar un token de la memoria caché. Esto puede deberse a que no hay tokens en la memoria caché o a que no se encontró una cuenta. El mensaje de error contiene más detalles.  | Llame a AcquireTokenInteractively(). |
| None    | No se proporcionan más detalles. La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a AcquireTokenInteractively(). |

## <a name="net-code-example"></a>Ejemplo de código .NET

```csharp
AuthenticationResult res;
try
{
 res = await application.AcquireTokenSilent(scopes, account)
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex) when (ex.ErrorCode == MsalError.InvalidGrantError)
{
 switch (ex.Classification)
 {
  case UiRequiredExceptionClassification.None:
   break;
  case UiRequiredExceptionClassification.MessageOnly:
  // You might want to call AcquireTokenInteractive(). Azure AD will show a message
  // that explains the condition. AcquireTokenInteractively() will return UserCanceled error
  // after the user reads the message and closes the window. The calling application may choose
  // to hide features or data that result in message_only if the user is unlikely to benefit 
  // from the message
  try
  {
      res = await application.AcquireTokenInteractive(scopes).ExecuteAsync();
  }
  catch (MsalClientException ex2) when (ex2.ErrorCode == MsalError.AuthenticationCanceledError)
  {
   // Do nothing. The user has seen the message
  }
  break;

  case UiRequiredExceptionClassification.BasicAction:
  // Call AcquireTokenInteractive() so that the user can, for instance accept terms
  // and conditions

  case UiRequiredExceptionClassification.AdditionalAction:
  // You might want to call AcquireTokenInteractive() to show a message that explains the remedial action. 
  // The calling application may choose to hide flows that require additional_action if the user 
  // is unlikely to complete the remedial action (even if this means a degraded experience)

  case UiRequiredExceptionClassification.ConsentRequired:
  // Call AcquireTokenInteractive() for user to give consent.
  
  case UiRequiredExceptionClassification.UserPasswordExpired:
  // Call AcquireTokenInteractive() so that user can reset their password
  
  case UiRequiredExceptionClassification.PromptNeverFailed:
  // You used WithPrompt(Prompt.Never) and this failed
  
  case UiRequiredExceptionClassification.AcquireTokenSilentFailed:
  default:
  // May be resolved by user interaction during the interactive authentication flow.
  res = await application.AcquireTokenInteractive(scopes)
                         .ExecuteAsync(); break;
 }
}
```
[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

Cuando llama a una API que requiere acceso condicional desde MSAL.NET, la aplicación debe controlar las excepciones del desafío de notificaciones. Esto aparecerá como [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) donde la propiedad [Claims](/dotnet/api/microsoft.identity.client.msalserviceexception.claims) no estará vacía.

Para controlar el desafío de notificaciones, deberá usar el método `.WithClaim()` de la clase `PublicClientApplicationBuilder`.

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

### <a name="http-error-codes-500-600"></a>Códigos de error HTTP 500-600

MSAL.NET implementa un único mecanismo de un solo reintento para aquellos errores con códigos de error 500-600.

[MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) muestra `System.Net.Http.Headers.HttpResponseHeaders` como propiedad `namedHeaders`. Puede usar la información adicional del código de error para mejorar la confiabilidad de las aplicaciones. En el caso que acabamos de describir, puede usar `RetryAfterproperty` (del tipo `RetryConditionHeaderValue`) y calcular cuándo se debe volver a intentar.

Este es un ejemplo de una aplicación de demonio que usa el flujo de credenciales de cliente. Puede adaptarlo a cualquiera de los métodos de adquisición de un token.

```csharp

bool retry = false;
do
{
    TimeSpan? delay;
    try
    {
         result = await publicClientApplication.AcquireTokenForClient(scopes, account).ExecuteAsync();
    }
    catch (MsalServiceException serviceException)
    {
         if (serviceException.ErrorCode == "temporarily_unavailable")
         {
             RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
             if (retryAfter.Delta.HasValue)
             {
                 delay = retryAfter.Delta;
             }
             else if (retryAfter.Date.HasValue)
             {
                 delay = retryAfter.Date.Value.Offset;
             }
         }
    }
    // . . .
    if (delay.HasValue)
    {
        Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
        retry = true;
    }
} while (retry);
```

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de habilitar el [registro en MSAL.NET](msal-logging-dotnet.md) para ayudarle a diagnosticar y depurar problemas.
