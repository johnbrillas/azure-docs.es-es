---
title: Búsqueda de una API para una aplicación de desarrollo personalizado | Azure
description: Configuración de los permisos necesarios para acceder a una API determinada en una aplicación desarrollada internamente con Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 4a108f8f5c5b333a9fc7e6eabbfa516b6578ca76
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063831"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Búsqueda de una API específica necesaria para una aplicación desarrollada internamente

El acceso a API requiere configurar los ámbitos y roles de acceso. Si desea exponer su API web de aplicaciones de recursos a aplicaciones cliente, debe configurar los roles y ámbitos de acceso de la API. Si desea que una aplicación cliente pueda acceder a una API web, debe configurar los permisos de acceso a la API en el registro de la aplicación.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configuración de una aplicación de recursos para exponer las API web

Al exponer la API web, la API se muestra en la lista **Seleccionar una API** al agregar permisos al registro de una aplicación. Para agregar ámbitos de acceso, siga los pasos descritos en [Configuración de una aplicación para exponer las API web](quickstart-configure-app-expose-web-apis.md).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configuración de una aplicación cliente para acceder a las API web

Al agregar permisos al registro de la aplicación, también puede **agregar acceso a API** a las API web expuestas. Para obtener acceso a las API web, siga los pasos descritos en [Configuración de una aplicación cliente para tener acceso a las API web](quickstart-configure-app-access-web-apis.md).

## <a name="next-steps"></a>Pasos siguientes

- [Descripción del manifiesto de aplicación de Azure Active Directory](./reference-app-manifest.md)