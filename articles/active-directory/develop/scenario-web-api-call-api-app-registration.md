---
title: Registro de una API web que llama a las API web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web que llame a API web de bajada (registro de la aplicación).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: af1047c5f890b1b88ae6d043a30704e84b8dc079
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99584321"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>API web que llama a API web: Registro de aplicación

Una API web que llama a API web de bajada tiene el mismo registro que una API web protegida. Siga las instrucciones de [API web protegida: registro de aplicación](scenario-protected-web-api-app-registration.md).

Dado que la aplicación web ahora llama a las API web, se convierte en una aplicación cliente confidencial. Por eso se exige información de registro adicional: la aplicación tiene que compartir secretos (credenciales de cliente) con la Plataforma de identidad de Microsoft.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones web llaman a las API en nombre de los usuarios para los que se ha recibido el token de portador. Tienen que solicitar permisos delegados. Para más información, consulte [Adición de permisos para acceder a la API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Configuración del código de la aplicación](scenario-web-api-call-api-app-configuration.md).
