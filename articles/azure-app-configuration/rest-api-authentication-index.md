---
title: 'API REST de Azure App Configuration: autenticación'
description: Páginas de referencia para la autenticación mediante la API REST de Azure App Configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 58fba309f4cf7e4fc4364d075500c02e0ed45259
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96932700"
---
# <a name="authentication"></a>Autenticación

Se deben autenticar todas las solicitudes HTTP. Se admiten los siguientes esquemas de autenticación.

## <a name="hmac"></a>HMAC

La [autenticación HMAC](./rest-api-authentication-hmac.md) usa un secreto generado aleatoriamente para firmar las cargas de las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de HMAC](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

La [autenticación de Azure Active Directory (Azure AD)](../active-directory/authentication/overview-authentication.md) emplea un token de portador que se obtiene de Azure Active Directory para autenticar las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de Azure AD](./rest-api-authorization-azure-ad.md).