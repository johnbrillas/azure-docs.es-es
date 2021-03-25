---
title: Bibliotecas de autenticación de la Plataforma de identidad de Microsoft | Azure
description: Lista de bibliotecas de cliente y middleware compatible con la Plataforma de identidad de Microsoft. Use estas bibliotecas para agregar compatibilidad con el inicio de sesión de usuario (autenticación) y acceso a API web protegida (autorización) a las aplicaciones.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 01/29/2021
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 590e57d587c8e6e254811892b5c5e740b511c302
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104690660"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Bibliotecas de autenticación de la Plataforma de identidad de Microsoft

En las tablas siguientes se muestra la compatibilidad de la Biblioteca de autenticación de Microsoft con varios tipos de aplicaciones. Incluyen vínculos al código fuente de la biblioteca, dónde obtener el paquete para el proyecto de la aplicación y si la biblioteca admite el inicio de sesión de usuario (autenticación), el acceso a API web protegidas (autorización) o ambos.

OpenID Foundation ha certificado la Plataforma de identidad de Microsoft como un [proveedor certificado de OpenID](https://openid.net/certification/). Si prefiere usar una biblioteca que no sea la biblioteca de autenticación de Microsoft (MSAL) u otra biblioteca compatible con Microsoft, elija una con una [implementación certificada de OpenID Connect](https://openid.net/developers/certified/).

Si decide codificar manualmente su propia implementación de nivel de protocolo de [OAuth 2.0 u OpenID Connect 1.0](active-directory-v2-protocols.md), preste especial atención a las consideraciones de seguridad de cada especificación estándar y siga una metodología de ciclo de vida de desarrollo de software (SDL), como [Microsoft SDL][Microsoft-SDL].

## <a name="single-page-application-spa"></a>Aplicación de página única (SPA)

Una aplicación de una sola página se ejecuta completamente en la superficie del explorador y captura los datos de la página (HTML, CSS y JavaScript) dinámicamente o en el momento de la carga de la aplicación. Puede llamar a las API web para interactuar con los orígenes de datos de back-end.

Dado que el código de SPA se ejecuta completamente en el explorador, se considera un *cliente público* que no puede almacenar secretos de forma segura.

[!INCLUDE [active-directory-develop-libraries-spa](../../../includes/active-directory-develop-libraries-spa.md)]

## <a name="web-application"></a>Aplicación web

Una aplicación web ejecuta código en un servidor que genera y envía HTML, CSS y JavaScript al explorador web de un usuario que se va a representar. La identidad del usuario se mantiene como una sesión entre el explorador del usuario (el front-end) y el servidor web (el back-end).

Dado que el código de una aplicación web se ejecuta en el servidor web, se considera un *cliente confidencial* que puede almacenar secretos de forma segura.

[!INCLUDE [active-directory-develop-libraries-webapp](../../../includes/active-directory-develop-libraries-webapp.md)]

## <a name="desktop-application"></a>Aplicación de escritorio

Una aplicación de escritorio suele ser código binario (compilado) que muestra una interfaz de usuario y que se ha diseñado para ejecutarse en el escritorio del usuario.

Dado que una aplicación de escritorio se ejecuta en el escritorio del usuario, se considera un *cliente público* que no puede almacenar secretos de forma segura.

[!INCLUDE [active-directory-develop-libraries-desktop](../../../includes/active-directory-develop-libraries-desktop.md)]

## <a name="mobile-application"></a>Aplicación para dispositivos móviles

Una aplicación móvil suele ser código binario (compilado) que muestra una interfaz de usuario y que se ha diseñado para ejecutarse en un dispositivo móvil del usuario.

Dado que una aplicación móvil se ejecuta en el dispositivo móvil del usuario, se considera un *cliente público* que no puede almacenar secretos de forma segura.

[!INCLUDE [active-directory-develop-libraries-mobile](../../../includes/active-directory-develop-libraries-mobile.md)]

## <a name="service--daemon"></a>Servicio o demonio

Los servicios y los demonios suelen usarse para la comunicación de servidor a servidor y otras comunicaciones desatendidas (a veces denominados *equipos sin periféricos*). Dado que no hay ningún usuario en el teclado para escribir las credenciales o dar su consentimiento para el acceso a los recursos, estas aplicaciones se autentican a sí mismas, no a un usuario, al solicitar acceso autorizado a los recursos de una API web.

Un servicio o demonio que se ejecuta en un servidor se considera un *cliente confidencial* que puede almacenar sus secretos de forma segura.

[!INCLUDE [active-directory-develop-libraries-daemon](../../../includes/active-directory-develop-libraries-daemon.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la biblioteca de autenticación de Microsoft, consulte [Introducción a la Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md).

<!--Image references-->
[y]: ./media/common/yes.png
[n]: ./media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
