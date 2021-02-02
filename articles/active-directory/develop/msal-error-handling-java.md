---
title: Control de errores y excepciones en MSAL4J
titleSuffix: Microsoft identity platform
description: Aprenda a controlar errores y excepciones, desafíos de las notificaciones del acceso condicional y reintentos en las aplicaciones de MSAL4J.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/27/2020
ms.author: marsma
ms.reviewer: saeeda, nacanuma
ms.custom: aaddev
ms.openlocfilehash: 8563804a736c37acc9a96eb4a186933507f34200
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98760645"
---
# <a name="handle-errors-and-exceptions-in-msal-for-java"></a>Control de errores y excepciones en MSAL para Java

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-java"></a>Control de errores en MSAL para Java

En MSAL para Java, hay tres tipos de excepciones: `MsalClientException`, `MsalServiceException` y `MsalInteractionRequiredException`; todo lo cual se hereda de `MsalException`.

- `MsalClientException` se produce cuando ocurre un error que es local en la biblioteca o el dispositivo.
- `MsalServiceException` se produce cuando el servicio de token seguro (STS) devuelve una respuesta de error o tiene lugar otro error de red.
- `MsalInteractionRequiredException` se produce cuando se requiere la interacción con la interfaz de usuario para que la autenticación se realice correctamente.

### <a name="msalserviceexception"></a>MsalServiceException

`MsalServiceException` expone los encabezados HTTP devueltos en las solicitudes al STS. Acceda a ellas a través de `MsalServiceException.headers()`.

### <a name="msalinteractionrequiredexception"></a>MsalInteractionRequiredException

Uno de los códigos de estado comunes que devuelve MSAL para Java al llamar a `AcquireTokenSilently()` es `InvalidGrantError`. Esto significa que se requiere interacción adicional del usuario para emitir el token de autenticación. La aplicación debe volver a llamar a la biblioteca de autenticación, pero en modo interactivo mediante el envío de `AuthorizationCodeParameters` o `DeviceCodeParameters` para aplicaciones cliente públicas.

La mayoría de las veces en que `AcquireTokenSilently` produce un error, se debe a que la memoria caché del token no tiene tokens que coincidan con la solicitud. Los tokens de acceso expiran en una hora y `AcquireTokenSilently` intentará obtener uno nuevo en función de un token de actualización. Según OAuth2, este es el flujo del token de actualización. Este flujo también puede producir un error por diversos motivos; por ejemplo, cuando un administrador de inquilinos configura directivas de inicio de sesión más rigurosas.

Algunas de las condiciones que producen este error son fáciles de resolver para los usuarios. Por ejemplo, puede que necesiten aceptar las condiciones de uso o la solicitud no se podrá cumplir con la configuración actual porque la máquina debe conectarse a una red corporativa específica.

MSAL expone un campo de `reason`, que puede usar para proporcionar una mejor experiencia de usuario. Por ejemplo, el campo `reason` puede indicarle que debe decirle al usuario que su contraseña expiró o que necesitará dar su consentimiento para usar ciertos recursos. Los valores admitidos son parte de la enumeración `InteractionRequiredExceptionReason`:

| Motivo | Significado | Uso recomendado |
|---------|-----------|-----------------------------|
| `BasicAction` | La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a `acquireToken` con parámetros interactivos. |
| `AdditionalAction` | La interacción de corrección adicional con el sistema puede resolver la condición, independientemente del flujo de autenticación interactiva. | Llame a `acquireToken` con parámetros interactivos para mostrar un mensaje que explique la acción correctiva. La aplicación que realiza la llamada puede ocultar los flujos que requieren acciones adicionales si es poco probable que el usuario complete la acción correctiva. |
| `MessageOnly` | No se puede resolver la condición en este momento. Al iniciar el flujo de autenticación interactiva, se mostrará un mensaje que explica la condición. | Llame a `acquireToken` con parámetros interactivos para mostrar un mensaje que explique la condición. `acquireToken` devolverá el error `UserCanceled` después de que el usuario lea el mensaje y cierre la ventana. La aplicación puede optar por ocultar los flujos que dan como resultado un mensaje si es poco probable que el usuario se beneficie del mensaje. |
| `ConsentRequired`| Falta el consentimiento del usuario o se ha revocado. |Llame a `acquireToken` con parámetros interactivos para que el usuario pueda dar su consentimiento. |
| `UserPasswordExpired` | La contraseña del usuario ha expirado. | Llame a `acquireToken` con parámetros interactivos para que el usuario pueda restablecer su contraseña. |
| `None` |  No se proporcionan más detalles. La interacción del usuario puede resolver la condición durante el flujo de autenticación interactiva. | Llame a `acquireToken` con parámetros interactivos. |

### <a name="code-example"></a>Ejemplo de código

```java
        IAuthenticationResult result;
        try {
            PublicClientApplication application = PublicClientApplication
                    .builder("clientId")
                    .b2cAuthority("authority")
                    .build();

            SilentParameters parameters = SilentParameters
                    .builder(Collections.singleton("scope"))
                    .build();

            result = application.acquireTokenSilently(parameters).join();
        }
        catch (Exception ex){
            if(ex instanceof MsalInteractionRequiredException){
                // AcquireToken by either AuthorizationCodeParameters or DeviceCodeParameters
            } else{
                // Log and handle exception accordingly
            }
        }
```

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de habilitar el [registro en MSAL para Java](msal-logging-java.md) para ayudarle a diagnosticar y depurar problemas.
