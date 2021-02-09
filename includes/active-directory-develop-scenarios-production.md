---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 75650d7ff0ac647aeb6dace76c270680b1b89347
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98954973"
---
## <a name="enable-logging"></a>Habilitar registro

Para ayudar en los escenarios de solución de problemas de depuración y de errores de autenticación, la Biblioteca de autenticación de Microsoft proporciona compatibilidad con el registro integrado. El registro en cada biblioteca se trata en los siguientes artículos:

:::row:::
    :::column:::
        - [Registro en MSAL.NET](../articles/active-directory/develop/msal-logging-dotnet.md)
        - [Inicio de sesión en MSAL para Android](../articles/active-directory/develop/msal-logging-android.md)
        - [Registro en MSAL.js](../articles/active-directory/develop/msal-logging-js.md)
    :::column-end:::
    :::column:::
        - [Inicio de sesión en MSAL para iOS/macOS](../articles/active-directory/develop/msal-logging-ios.md)
        - [Registro en MSAL para Java](../articles/active-directory/develop/msal-logging-java.md)
        - [Inicio de sesión en MSAL para Python](../articles/active-directory/develop/msal-logging-python.md)
    :::column-end:::
:::row-end:::

Estas son algunas sugerencias para la recopilación de datos:

- Los usuarios pueden pedir ayuda cuando tengan problemas. Un procedimiento recomendado es capturar y almacenar temporalmente los registros. Proporcione una ubicación donde los usuarios puedan cargar los registros. MSAL proporciona extensiones de registro para capturar información detallada acerca de la autenticación.

- Si hay telemetría disponible, habilítela mediante MSAL para recopilar datos acerca de cómo inician sesión en la aplicación los usuarios.


## <a name="validate-your-integration"></a>Validación de la integración

Pruebe la integración siguiendo la [lista de comprobación de integración de la Plataforma de identidad de Microsoft](../articles/active-directory/develop/identity-platform-integration-checklist.md).
