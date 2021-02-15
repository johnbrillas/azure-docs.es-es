---
title: Paso a producción de una aplicación de demonio que llama a las API web | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a pasar a producción una aplicación de demonio que llama a las API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8dc9bff86a07f3d4a0ec6fd224de6d5633165a6d
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582848"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Aplicación de demonio que llama a las API web: paso a producción

Ahora que sabe cómo adquirir y usar un token para una llamada de servicio a servicio, aprenda a pasar la aplicación a producción.

## <a name="deployment---multitenant-daemon-apps"></a>Implementación: aplicaciones de demonio multiinquilino

Si es un ISV que crea una aplicación de demonio que se pueda ejecutar en varios inquilinos, asegúrese de que el administrador del inquilino:

- Aprovisione una entidad de servicio para la aplicación.
- Dé su consentimiento a la aplicación.

Deberá explicar a sus clientes cómo realizar estas operaciones. Para más información, consulte [Solicitud de consentimiento para un inquilino al completo](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="code-samples"></a>Ejemplos de código

# <a name="net"></a>[.NET](#tab/dotnet)

- Documentación de referencia para:
  - Creación de instancias de [ConfidentialClientApplication](/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder).
  - Llamada a [AcquireTokenForClient](/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder).
- Otros ejemplos y tutoriales:
  - [microsoft-identity-platform-console-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) incluye una aplicación de consola de demonio de .NET Core pequeña que muestra los usuarios de un inquilino que consultan Microsoft Graph.

    ![Ejemplo de topología de aplicación de demonio](media/scenario-daemon-app/daemon-app-sample.svg)

    El ejemplo también ilustra una variación con certificados:

    ![Ejemplo de topología de aplicación de demonio: certificados](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [microsoft-identity-platform-aspnet-webapp-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) incluye una aplicación web de ASP.NET MVC que sincroniza los datos de Microsoft Graph mediante la identidad de la aplicación, en lugar hacerlo en nombre de un usuario. El ejemplo también ilustra el proceso de consentimiento del administrador.

    ![Topología](media/scenario-daemon-app/damon-app-sample-web.svg)

---

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos vínculos para ayudarle a obtener más información:

# <a name="python"></a>[Python](#tab/python)

Pruebe la guía de inicio rápido [Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola en Python mediante la identidad de la aplicación](./quickstart-v2-python-daemon.md).

# <a name="java"></a>[Java](#tab/java)

Pruebe la guía de inicio rápido [Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola de Java mediante la identidad de la aplicación](./quickstart-v2-java-daemon.md).

---
