---
title: 'API web protegida: Información general'
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web protegida (información general).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 434c255f0b9634e0d17bad1627ddd92af9498bfe
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99582441"
---
# <a name="scenario-protected-web-api"></a>Escenario: API web protegida

En este escenario, aprenderá a exponer una API web. También aprenderá a proteger la API web para que solo los usuarios autenticados puedan tener acceso a ella.

Para usar la API web, habilite usuarios autenticados con cuentas profesionales y educativas, o bien habilite cuentas personales de Microsoft.

## <a name="specifics"></a>Características específicas

Esta es la información específica que necesita saber para proteger las API web:

- El registro de la aplicación debe exponer al menos un *ámbito* o un *rol de aplicación*.
  - Los ámbitos se exponen mediante API web a las que se llama en nombre de un usuario.
  - Los roles de aplicación se exponen mediante API web a las que llaman aplicaciones de demonio (que llaman a la API web en nombre propio).
- Si crea un registro de aplicación de API web, elija la [versión del token de acceso](reference-app-manifest.md#accesstokenacceptedversion-attribute) aceptada por la API web para `2`. En el caso de API web heredadas, la versión del token aceptada puede ser `null`, pero este valor restringe el público que inicia sesión a solo las organizaciones y no se admiten cuentas Microsoft (MSA) personales.
- La configuración de código de la API web debe validar el token usado cuando se llama a la API web.
- El código de las acciones de controlador debe validar los roles o ámbitos del token.

## <a name="recommended-reading"></a>Lecturas recomendadas

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Registro de la aplicación](scenario-protected-web-api-app-registration.md).
