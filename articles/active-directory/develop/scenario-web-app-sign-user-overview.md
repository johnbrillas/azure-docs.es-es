---
title: Inicio de sesión de usuarios desde una aplicación web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar sesión a los usuarios (introducción).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 0bbc799f946d318c305a96d9cb8c6831d9242ff6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104578301"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Escenario: Aplicación web que permite iniciar sesión a los usuarios

Aprenda todo lo que necesita para crear una aplicación web que use la Plataforma de identidad de Microsoft para que los usuarios inicien sesión.

## <a name="getting-started"></a>Introducción

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Si quiere crear sus primera aplicación web portátil (ASP.NET Core) que permita iniciar sesión a los usuarios, siga este inicio rápido:

[Inicio rápido: Aplicación web ASP.NET Core que permite iniciar sesión a los usuarios](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Si desea saber cómo agregar el inicio de sesión a una aplicación web ASP.NET existente, pruebe el siguiente inicio rápido:

[Inicio rápido: Aplicación web ASP.NET que inicia la sesión de los usuarios](quickstart-v2-aspnet-webapp.md)

# <a name="java"></a>[Java](#tab/java)

Si es un desarrollador de Java, pruebe el siguiente inicio rápido:

[Inicio rápido: Adición de inicio de sesión con Microsoft a una aplicación web de Java](quickstart-v2-java-webapp.md)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Si es un desarrollador de Node.js, pruebe el siguiente inicio rápido:

[Inicio rápido: Inicio de sesión de los usuarios y obtención de un token de acceso en una aplicación web de Node mediante el flujo de código de autorización](quickstart-v2-nodejs-webapp-msal.md)

# <a name="python"></a>[Python](#tab/python)

Si desarrolla con Python, pruebe el siguiente inicio rápido:

[Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de Python](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Información general

Agregue autenticación a la aplicación web para que permita iniciar sesión a los usuarios. La adición de autenticación permite a la aplicación web tener acceso a información de perfil limitada para personalizar la experiencia para los usuarios.

Las aplicaciones web autentican un usuario en un explorador web. En este escenario, la aplicación web dirige el explorador del usuario para que inicie sesión en Azure Active Directory (Azure AD). Azure AD devuelve una respuesta de inicio de sesión a través el explorador del usuario, que contiene notificaciones sobre el usuario en un token de seguridad. El inicio de sesión de los usuarios aprovecha el protocolo estándar [Open ID Connect](./v2-protocols-oidc.md) simplificado mediante el uso de [bibliotecas](scenario-web-app-sign-user-app-configuration.md#microsoft-libraries-supporting-web-apps) de middleware.

![Aplicación web que inicia sesión de usuarios](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

En una segunda fase, puede habilitar la aplicación para llamar a las API web en nombre del usuario con sesión iniciada. Esta fase siguiente es un escenario diferente, que encontrará en [Aplicación web que llama a las API web](scenario-web-app-call-api-overview.md).

> [!NOTE]
> Agregar inicio de sesión a una aplicación web consiste en proteger la aplicación web y validar un token de usuario, que es lo que hacen las bibliotecas de **middleware**. En el caso de .NET, este escenario aún no requiere la biblioteca de autenticación de Microsoft (MSAL), que se refiere a la adquisición de un token para llamar a las API protegidas. Las bibliotecas de autenticación para .NET se incluirán en el escenario de seguimiento cuando la aplicación web necesite llamar a las API web.

## <a name="specifics"></a>Características específicas

- Durante el registro de la aplicación, deberá proporcionar uno o varios (si implementa la aplicación en varias ubicaciones) URI de respuesta. En algunos casos (ASP.NET y ASP.NET Core), deberá habilitar el token del identificador. Por último, deberá configurar un URI de cierre de sesión para que la aplicación reaccione ante los usuarios que cierran sesión.
- En el código de la aplicación, deberá proporcionar la autoridad a la que la aplicación web delega el inicio de sesión. Es posible que quiera personalizar la validación del token (en concreto, en escenarios de asociado).
- Las aplicaciones web admiten cualquier tipo de cuenta. Para más información, consulte [Tipos de cuenta admitidos](v2-supported-account-types.md).

## <a name="recommended-reading"></a>Lecturas recomendadas

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Pasos siguientes

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Avance al siguiente artículo de este escenario, [Registro de la aplicación](./scenario-web-app-sign-user-app-registration.md?tabs=aspnetcore).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Avance al siguiente artículo de este escenario, [Registro de la aplicación](./scenario-web-app-sign-user-app-registration.md?tabs=aspnet).

# <a name="java"></a>[Java](#tab/java)

Avance al siguiente artículo de este escenario, [Registro de la aplicación](./scenario-web-app-sign-user-app-registration.md?tabs=java).

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Avance al siguiente artículo de este escenario, [Registro de la aplicación](./scenario-web-app-sign-user-app-registration.md?tabs=nodejs).

# <a name="python"></a>[Python](#tab/python)

Avance al siguiente artículo de este escenario, [Registro de la aplicación](./scenario-web-app-sign-user-app-registration.md?tabs=python).

---
