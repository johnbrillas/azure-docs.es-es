---
title: Preparación de la aplicación móvil que llama a las API web para producción | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web. (Preparación de aplicaciones para producción).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 08243fd06de289941d8e6a9197ccb349614af056
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104675964"
---
# <a name="prepare-mobile-apps-for-production"></a>Preparación de aplicaciones móviles para producción

En este artículo se proporcionan detalles sobre cómo mejorar la calidad y la confiabilidad de la aplicación móvil antes de moverla a producción.

## <a name="handle-errors"></a>errores

Al preparar una aplicación móvil para producción, pueden surgir varias condiciones de error. Los principales casos que va a administrar serán errores silenciosos y retrocesos de interacción. Otras condiciones que debe tener en cuenta incluyen situaciones sin red, interrupciones de servicio, requisitos para el consentimiento del administrador y otros casos específicos del escenario.

Para cada tipo de Biblioteca de autenticación de Microsoft (MSAL), puede encontrar código de ejemplo y contenido de wiki que describe cómo administrar las condiciones de error:

- [MSAL Android wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)


[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

Para probar ejemplos adicionales, consulte [Aplicaciones de escritorio y móviles de cliente público](sample-v2-code.md#desktop-and-mobile-public-client-apps).