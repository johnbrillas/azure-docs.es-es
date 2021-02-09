---
title: Cómo funciona el consentimiento de la aplicación
description: Más información sobre cómo funciona el marco de consentimiento de Azure AD para ver cómo se puede usar al desarrollar aplicaciones en Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: cd55375f2c5970ff0620e753923f369d40478cef
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99052032"
---
# <a name="how-application-consent-works"></a>Cómo funciona el consentimiento de la aplicación

Este artículo le ayudará a obtener más información acerca del funcionamiento del marco de consentimiento de Azure AD para desarrollar aplicaciones de forma más eficaz.

## <a name="recommended-documents"></a>Documentos recomendados

- Descripción general de [cómo el consentimiento permite al propietario de un recurso controlar el acceso de la aplicación a los recursos](./developer-glossary.md#consent).
- Introducción paso a paso de [cómo el marco de consentimiento de Azure AD implementa el consentimiento](./quickstart-register-app.md).
- Para más detalles, aprenda [cómo una aplicación multiinquilino puede utilizar el marco de consentimiento](./howto-convert-app-to-be-multi-tenant.md) para implementar el consentimiento de "usuario" y "administrador", compatible con patrones de aplicación de niveles múltiples más avanzados.
- Para más detalles, aprenda [cómo se admite el consentimiento en la capa del protocolo OAuth 2.0 durante el flujo de concesión de código de autorización](../azuread-dev/v1-protocols-oauth-code.md#request-an-authorization-code).

## <a name="next-steps"></a>Pasos siguientes
[Microsoft Q&A para Azure AD](https://docs.microsoft.com/answers/topics/azure-active-directory.html)