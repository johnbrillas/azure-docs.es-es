---
title: Control de errores y excepciones en MSAL para Python
titleSuffix: Microsoft identity platform
description: Aprenda a controlar errores y excepciones, desafíos de las notificaciones del acceso condicional y reintentos en las aplicaciones de MSAL para Python.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, rayluo
ms.custom: aaddev
ms.openlocfilehash: 3046787393d38ed60b54236f33acc065db90321d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98761094"
---
# <a name="handle-errors-and-exceptions-in-msal-for-python"></a>Control de errores y excepciones en MSAL para Python

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-python"></a>Control de errores en MSAL para Python

En MSAL para Python, la mayoría de los errores se transmiten como un valor devuelto de la llamada a la API. El error se representa como un diccionario que contiene la respuesta JSON de la Plataforma de identidad de Microsoft.

* Una respuesta correcta contiene la clave `"access_token"`. El formato de la respuesta se define mediante el protocolo OAuth2. Para más información, consulte [5.1 Respuesta correcta](https://tools.ietf.org/html/rfc6749#section-5.1)
* Una respuesta de error contiene `"error"` y, normalmente, `"error_description"`. El formato de la respuesta se define mediante el protocolo OAuth2. Para más información, consulte [5.2 Respuesta de error](https://tools.ietf.org/html/rfc6749#section-5.2)

Cuando se devuelve un error, la clave `"error_description"` suele contener un mensaje legible, que a su vez contiene un código de error de la Plataforma de identidad de Microsoft. Para ver los detalles de los diversos códigos de error, consulte [Códigos de error de autenticación y autorización](./reference-aadsts-error-codes.md).

En MSAL para Python, las excepciones son poco frecuentes, ya que, para controlar la mayoría de los errores, se devuelve un valor de error. La excepción `ValueError` solo se produce cuando hay un problema con la forma en que se intenta usar la biblioteca, como cuando los parámetros de la API no tienen el formato correcto.

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de habilitar el [registro en MSAL para Python](msal-logging-python.md) para ayudarle a diagnosticar y depurar problemas.
