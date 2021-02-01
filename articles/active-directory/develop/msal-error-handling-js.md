---
title: Control de errores y excepciones en MSAL.js
titleSuffix: Microsoft identity platform
description: Aprenda a controlar errores y excepciones, desafíos de las notificaciones del acceso condicional y reintentos en las aplicaciones de MSAL.js.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, hahamil
ms.custom: aaddev
ms.openlocfilehash: 20d276aba2ee3260911748cbee0a16020270059a
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761341"
---
# <a name="handle-errors-and-exceptions-in-msaljs"></a>Control de errores y excepciones en MSAL.js

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msaljs"></a>Control de errores en MSAL.js

MSAL.js proporciona objetos de error que abstraen y clasifican los distintos tipos de errores comunes. También proporciona una interfaz para acceder a detalles específicos de los errores, como los mensajes de error, para controlarlos de manera adecuada.

### <a name="error-object"></a>Objeto de error

```javascript
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```

Si extiende la clase de error, tendrá acceso a las siguientes propiedades:
- `AuthError.message`:  Igual que `errorMessage`.
- `AuthError.stack`: Seguimiento de la pila de los errores que se produjeron.

### <a name="error-types"></a>Tipos de errores

Están disponibles los siguientes tipos de errores:

- `AuthError`: Clase de error básica para la biblioteca MSAL.js. También se usa para los errores inesperados.

- `ClientAuthError`: clase de error que indica un problema con la autenticación de cliente. La mayoría de los errores que proceden de la biblioteca serán del tipo ClientAuthErrors. Estos errores son consecuencia de cosas como una llamada a un método de inicio de sesión con el inicio de sesión ya en curso, cuando el usuario cancela el inicio de sesión, etc.

- `ClientConfigurationError`: clase de error que extiende `ClientAuthError` que se produce antes de que se realicen las solicitudes si los parámetros de configuración de un usuario determinado faltan o tienen un formato incorrecto.

- `ServerError`: clase de error que representa las cadenas de error que envía el servidor de autenticación. Puede tratarse de errores como formatos o parámetros de solicitud no válidos o cualquier otro error que impida al servidor autenticar o autorizar al usuario.

- `InteractionRequiredAuthError`: clase de error que extiende `ServerError` para representar errores de servidor que requieren una llamada interactiva. `acquireTokenSilent` genera este error si se solicita al usuario que interactúe con el servidor para proporcionar credenciales o consentimiento para la autenticación o autorización. Los códigos de error incluyen `"interaction_required"`, `"login_required"` y `"consent_required"`.

Para el control de errores en flujos de autenticación con métodos de redireccionamiento (`loginRedirect`, `acquireTokenRedirect`), deberá registrar la devolución de llamada a la que se llama correctamente o con error después del redireccionamiento mediante el método `handleRedirectCallback()` de la siguiente manera:

```javascript
function authCallback(error, response) {
    //handle redirect response
}

var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);
myMSALObj.acquireTokenRedirect(request);
```

Los métodos de la experiencia emergente (`loginPopup`, `acquireTokenPopup`) devuelven promesas, de forma que puede usar el patrón de promesa (.then y .catch) para controlarlas de la siguiente manera:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="errors-that-require-interaction"></a>Errores que requieren interacción

Se devuelve un error al intentar usar un método no interactivo para adquirir un token (por ejemplo, `acquireTokenSilent`) y que MSAL no pudiera hacerlo de manera silenciosa.

Los posibles motivos son:

- Debe iniciar sesión
- Debe dar su consentimiento
- Debe pasar por una experiencia de autenticación multifactor.

La solución consiste en llamar a un método interactivo como `acquireTokenPopup` o `acquireTokenRedirect`:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

Al obtener los tokens de forma automática (mediante `acquireTokenSilent`) con MSAL.js, la aplicación puede recibir errores si una API a la que está intentando acceder requiere un [desafío de notificaciones de acceso condicional](../azuread-dev/conditional-access-dev-guide.md) como, por ejemplo, una directiva de autenticación multifactor.

El patrón para controlar este error consiste en realizar una llamada interactiva para adquirir el token en MSAL.js como, por ejemplo, `acquireTokenPopup` o `acquireTokenRedirect` como se puede ver en el ejemplo siguiente:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // call API
}).catch(function(error) {
    if (error instanceof InteractionRequiredAuthError) {
    
        // extract, if exists, claims from error message
        if (error.ErrorMessage.claims) {
            accessTokenRequest.claimsRequest = JSON.stringify(error.ErrorMessage.claims);
        }
        
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // call API
        }).catch(function(error) {
            console.log(error);
        });
    }
});
```

La adquisición interactiva del token avisa al usuario y le da la oportunidad de cumplir con la directiva de acceso condicional requerida.

Al llamar a una API que requiere acceso condicional, puede recibir un desafío de notificaciones en el error de la API. En este caso, puede pasar las notificaciones devueltas en el error al campo `claimsRequest` de la clase `AuthenticationParameters.ts` para satisfacer la directiva adecuada. 

Consulte cómo [solicitar notificaciones adicionales](active-directory-optional-claims.md) para más información.


[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de habilitar el [registro en MSAL.js](msal-logging-js.md) como ayuda para diagnosticar y depurar problemas.
