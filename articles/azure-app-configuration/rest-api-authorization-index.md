---
title: 'API REST de Azure App Configuration: autorización'
description: Páginas de referencia para la autorización mediante la API REST de Azure App Configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 70f05799ce2856ad490937a17b456e78789088f1
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96932649"
---
# <a name="authorization"></a>Authorization

La autorización se refiere al procedimiento que se usa para determinar los permisos que tiene un autor de llamada cuando envía una solicitud. Hay varios modelos de autorización. El modelo de autorización que se usa para una solicitud depende del método de [autenticación](./rest-api-authentication-index.md) que se utilice. A continuación se indican los modelos de autorización.

## <a name="hmac"></a>HMAC

El [modelo de autorización](./rest-api-authorization-hmac.md) asociado a la autenticación HMAC divide los permisos en permisos de solo lectura o permisos de lectura y escritura. Consulte la página de [autorización HMAC](./rest-api-authorization-hmac.md) para obtener más información.

## <a name="azure-active-directory"></a>Azure Active Directory

El [modelo de autorización](./rest-api-authorization-azure-ad.md) asociado a la autenticación de Azure Active Directory (Azure AD) usa Azure RBAC para controlar los permisos. Consulte la página de [autorización de Azure AD](./rest-api-authorization-azure-ad.md) para obtener más información.
