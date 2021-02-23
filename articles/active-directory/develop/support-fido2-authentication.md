---
title: Compatibilidad con la autenticación sin contraseña con claves FIDO2 en aplicaciones desarrolladas por el usuario | Azure
titleSuffix: Microsoft identity platform
description: En esta guía de implementación se explica cómo admitir la autenticación sin contraseña con claves de seguridad FIDO2 en aplicaciones desarrolladas por el usuario.
services: active-directory
author: knicholasa
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 1/29/2021
ms.author: nichola
ms.custom: aaddev
ms.openlocfilehash: 5abece0e272d4b72ba6f787ad44b091df5d45226
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416762"
---
# <a name="support-passwordless-authentication-with-fido2-keys-in-apps-you-develop"></a>Compatibilidad con la autenticación sin contraseña con claves FIDO2 en aplicaciones desarrolladas por el usuario

Estas configuraciones y procedimientos recomendados le ayudarán a evitar escenarios comunes que impiden que la [autenticación sin contraseñas FIDO2](../../active-directory/authentication/concept-authentication-passwordless.md) esté disponible para los usuarios de sus aplicaciones.

## <a name="general-best-practices"></a>Procedimientos recomendados generales

### <a name="domain-hints"></a>Sugerencias de dominio

No use una sugerencia de dominio para omitir la [detección del dominio de inicio](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md). Esta característica está pensada para simplificar los inicios de sesión, pero es posible que el proveedor de identidades federado no admita la autenticación sin contraseña.

### <a name="requiring-specific-credentials"></a>Requerimiento de credenciales específicas

Si está utilizando SAML, no especifique que se requiera una contraseña [mediante el elemento RequestedAuthnContext](single-sign-on-saml-protocol.md#requestauthncontext).

El elemento RequestedAuthnContext es opcional, por lo que para resolverlo puede quitarlo de las solicitudes de autenticación SAML. Este es un procedimiento recomendado general, ya que el uso de este elemento también puede impedir que otras opciones de autenticación, como la autenticación multifactor, funcionen correctamente.

### <a name="using-the-most-recently-used-authentication-method"></a>Uso del método de autenticación usado más recientemente

En primer lugar, se les presentará el método de inicio de sesión usado más recientemente por un usuario. Esto puede causar confusión cuando los usuarios creen que deben usar la primera opción presentada. Sin embargo, para elegir otra opción pueden seleccionar "Otras formas de iniciar sesión", como se muestra a continuación.

:::image type="content" source="./media/support-fido2-authentication/most-recently-used-method.png" alt-text="Imagen de la experiencia de autenticación de usuario que resalta el botón que permite al usuario cambiar el método de autenticación.":::

## <a name="platform-specific-best-practices"></a>Procedimientos recomendados específicos de la plataforma

### <a name="desktop"></a>Escritorio

Las opciones recomendadas para implementar la autenticación son, en orden:

- Las aplicaciones de escritorio de .NET que usan la biblioteca de autenticación de Microsoft (MSAL) deben usar el administrador de autenticación de Windows (WAM). Esta integración y sus ventajas están [documentadas en GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/wam).
- Use [WebView2](https://docs.microsoft.com/microsoft-edge/webview2/) para admitir FIDO2 en un explorador incrustado.
- Utilice el explorador del sistema. Las bibliotecas MSAL para plataformas de escritorio usan este método de forma predeterminada. Puede consultar nuestra página sobre la compatibilidad de exploradores de FIDO2 para asegurarse de que el explorador que usa admite la autenticación FIDO2.

### <a name="mobile"></a>Móvil

A partir de febrero de 2020, FIDO2 no se admite actualmente para aplicaciones de iOS o Android nativas, pero está en desarrollo.

Para preparar las aplicaciones para su disponibilidad y, como procedimiento recomendado general, las aplicaciones de iOS y Android deben usar MSAL con la configuración predeterminada de uso del explorador web del sistema.

Si no usa MSAL, debe utilizar igualmente el explorador web del sistema para la autenticación. Características como el inicio de sesión único y el acceso condicional dependen de una superficie web compartida proporcionada por el explorador web del sistema. Esto significa el uso de [pestañas personalizadas de Chrome](https://developer.chrome.com/docs/multidevice/android/customtabs/) (Android) o la [autenticación de un usuario a través de un servicio web | Documentación para desarrolladores de Apple](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service) (iOS).

### <a name="web-and-single-page-apps"></a>Aplicaciones web y de una página

La disponibilidad de la autenticación sin contraseña FIDO2 para las aplicaciones que se ejecutan en un explorador web dependerá de la combinación de explorador y plataforma. Puede consultar nuestra [matriz de compatibilidad de FIDO2](../authentication/fido2-compatibility.md) para comprobar si se admite la combinación que encontrarán los usuarios.

## <a name="next-steps"></a>Pasos siguientes

[Opciones de autenticación sin contraseña de Azure Active Directory](../../active-directory/authentication/concept-authentication-passwordless.md)
