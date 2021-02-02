---
title: Autenticación frente a autorización | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre los conceptos básicos de la autenticación y la autorización de la Plataforma de identidad de Microsoft.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: b81b34010736bce33085cb1ebf0faa3da6a41bd6
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98755396"
---
# <a name="authentication-vs-authorization"></a>Autenticación frente a autorización

En este artículo se define la autenticación y la autorización. También se explica brevemente cómo puede usar la plataforma de identidad de Microsoft para autenticar y autorizar a los usuarios en sus aplicaciones web, API web o aplicaciones que llaman a API web protegidas. Si ve un término con el que no está familiarizado, pruebe nuestro [glosario](developer-glossary.md) o nuestros [vídeos de la plataforma de identidad de Microsoft](identity-videos.md), que abarcan los conceptos básicos.

## <a name="authentication"></a>Authentication

La *autenticación* es el proceso de demostrar que es quien dice ser. A veces se acorta a *AuthN*. La plataforma de identidad de Microsoft usa el protocolo [OpenID Connect](https://openid.net/connect/) para administrar la autenticación.

## <a name="authorization"></a>Authorization

La *autorización* es el acto de conceder a una parte autenticada permiso para hacer algo. Especifica a qué datos se puede acceder y qué se puede hacer con ellos. A veces, la autorización se acorta a *AuthZ*. La plataforma de identidad de Microsoft usa el protocolo [OAuth 2.0](https://oauth.net/2/) para administrar la autorización.

## <a name="authentication-and-authorization-using-the-microsoft-identity-platform"></a>Autenticación y autorización con la Plataforma de identidad de Microsoft

La creación de aplicaciones que mantienen su propia información de nombre de usuario y contraseña conlleva una gran carga administrativa cuando hace falta agregar o quitar usuarios en varias aplicaciones. Sin embargo, las aplicaciones pueden delegar esa responsabilidad en un proveedor de identidades centralizado.

Azure Active Directory (Azure AD) es un proveedor de identidades centralizado en la nube. La delegación de la autenticación y la autorización en él permite escenarios como los siguientes:

- Directivas de acceso condicional que requieren que un usuario esté en una ubicación específica.
- El uso de la [autenticación multifactor](../authentication/concept-mfa-howitworks.md), que a veces se denomina autenticación en dos fases o 2FA.
- Posibilidad de que un usuario inicie la sesión una vez y luego lo haga automáticamente en todas las aplicaciones web que comparten el mismo directorio centralizado. Esta funcionalidad se denomina *inicio de sesión único (SSO)* .

La plataforma de identidad de Microsoft simplifica la autorización y la autenticación para los desarrolladores de aplicaciones al proporcionar la identidad como servicio. También, admite protocolos estándar del sector y bibliotecas de código abierto de distintas plataformas que le ayudarán a empezar a crear código rápidamente. Asimismo, permite a los desarrolladores crear aplicaciones que inician sesión en todas las identidades de Microsoft, obtener tokens para llamar a [Microsoft Graph](https://developer.microsoft.com/graph/), acceder a API de Microsoft o acceder a otras API que los desarrolladores hayan creado.

En este vídeo se describe la plataforma de identidad de Microsoft y los aspectos básicos de la autenticación moderna: 

> [!VIDEO https://www.youtube.com/embed/tkQJSHFsduY]

A continuación, se muestra una comparación de los protocolos que utiliza la plataforma de identidad de Microsoft:

* **OAuth frente a OpenID Connect**: la plataforma usa OAuth para la autorización y OpenID Connect (OIDC) para la autenticación. OpenID Connect se basa en OAuth 2.0, por lo que la terminología y el flujo son similares entre los dos. Incluso puede autenticar a un usuario (mediante OpenID Connect) y obtener autorización para acceder a un recurso protegido que posea el usuario (mediante OAuth 2.0) en una solicitud. Para más información, consulte los [Protocolos OAuth 2.0 y OpenID Connect](active-directory-v2-protocols.md) y [Protocolo OpenID Connect](v2-protocols-oidc.md).
* **OAuth frente a SAML**: la plataforma usa OAuth 2.0 para la autorización y SAML para la autenticación. Para más información sobre cómo usar estos protocolos juntos para autenticar a un usuario y obtener autorización para acceder a recursos protegidos, consulte [Plataforma de identidad de Microsoft y el flujo de aserción de token de portador SAML de OAuth 2.0](./scenario-token-exchange-saml-oauth.md).
* **OpenID Connect frente a SAML**: la plataforma usa OpenID Connect y SAML para autenticar a un usuario y habilitar el inicio de sesión único. La autenticación SAML se usa normalmente con proveedores de identidades, como Servicios de federación de Active Directory (AD FS) federados con Azure AD, por lo que se usa con frecuencia en aplicaciones empresariales. OpenID Connect se usa normalmente con las aplicaciones que se encuentran exclusivamente en la nube, como aplicaciones móviles, sitios web y API web.

## <a name="next-steps"></a>Pasos siguientes

Para ver otros temas en los que se traten aspectos básicos de la autenticación y la autorización, consulte:

* Para saber cómo se usan los tokens de acceso, los tokens de actualización y los tokens de identificación en la autorización y la autenticación, consulte [Tokens de seguridad](security-tokens.md).
* Para conocer el proceso de registrar la aplicación para que se pueda integrar con la plataforma de identidad de Microsoft, consulte [Modelo de aplicación](application-model.md).