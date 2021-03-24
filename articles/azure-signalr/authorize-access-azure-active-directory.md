---
title: Autorización del acceso con Azure Active Directory
description: En este artículo se proporciona información sobre cómo autorizar el acceso a recursos de Azure SignalR Service mediante Azure Active Directory.
author: terencefan
ms.author: tefa
ms.date: 08/03/2020
ms.service: signalr
ms.topic: conceptual
ms.openlocfilehash: 37c2e41b5c81f941245b895ecd144baee3b9db6d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97797415"
---
# <a name="authorize-access-to-azure-signalr-service-resources-using-azure-active-directory"></a>Autorice el acceso a los recursos de Azure SignalR Service mediante Azure Active Directory
Azure SignalR Service admite el uso de Azure Active Directory (Azure AD) para autorizar solicitudes a los recursos de Azure SignalR Service. Con Azure AD, puede usar el control de acceso basado en rol (RBAC) para conceder permisos a una entidad de seguridad, que puede ser un usuario o una entidad de servicio de aplicación. Para más información sobre los roles y las asignaciones de roles, consulte [Descripción de los distintos roles](../role-based-access-control/overview.md).

## <a name="overview"></a>Información general
Cuando una entidad de seguridad (una aplicación) intenta tener acceso a un recurso de Azure SignalR Service, la solicitud debe estar autorizada. Con Azure AD, el acceso a un recurso es un proceso de dos pasos. 

 1. En primer lugar, se autentica la identidad de la entidad de seguridad y se devuelve un token de OAuth 2.0. El nombre del recurso para solicitar un token es `https://signalr.azure.com/`.
 2. Luego, el token se pasa como parte de una solicitud al servicio Azure SignalR Service para autorizar el acceso al recurso especificado.

El paso de autenticación exige que una solicitud de aplicación contenga un token de acceso de OAuth 2.0 en tiempo de ejecución. Si el servidor concentrador se está ejecutando dentro de una entidad de Azure, como puede ser una máquina virtual de Azure, un conjunto de escalado de máquinas virtuales o una aplicación de Azure Functions, puede usar una identidad administrada para obtener acceso a los recursos. Para más información sobre cómo autenticar solicitudes realizadas por una identidad administrada al servicio Azure SignalR Service, consulte [Autenticación del acceso a recursos de Azure SignalR Service con Azure Active Directory e identidades administradas para los recursos de Azure](authenticate-managed-identity.md). 

El paso de autorización exige que se asignen uno o varios roles RBAC a la entidad de seguridad. Azure SignalR Service proporciona roles RBAC que abarcan conjuntos de permisos para recursos de Azure SignalR. Los roles que se asignan a una entidad de seguridad determinan los permisos que tiene esa entidad de seguridad. Para más información sobre los roles RBAC, consulte [Roles integrados de Azure para Azure SignalR Service](#azure-built-in-roles-for-azure-signalr-service). 

El servidor concentrador SignalR que no se ejecuta dentro de una entidad de Azure también puede autorizar con Azure AD. Para obtener información sobre cómo solicitar un token de acceso y usarlo para autorizar solicitudes para recursos de Azure SignalR Server, consulte [Autenticación del acceso a Azure SignalR Service con Azure AD desde una aplicación](authenticate-application.md). 

## <a name="azure-built-in-roles-for-azure-signalr-service"></a>Roles integrados de Azure para Azure SignalR Service

- [SignalR App Server]
- [SignalR Service Reader]
- [SignalR Service Owner]

## <a name="assign-rbac-roles-for-access-rights"></a>Asignación de roles RBAC para derechos de acceso
Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md). Azure SignalR Service define un conjunto de roles integrados de Azure que abarcan conjuntos de permisos comunes utilizados para obtener acceso a Azure SignalR Service y también puede definir roles personalizados para acceder al recurso.

Cuando un rol RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos a esa entidad de seguridad. El acceso puede tener como ámbito el nivel de suscripción, el grupo de recursos o cualquier recurso de Azure SignalR Service. Una entidad de seguridad de Azure AD puede ser un usuario, una aplicación o una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="built-in-roles-for-azure-signalr-service"></a>Roles integrados para Azure SignalR Service
Azure proporciona los siguientes roles integrados de Azure para autorizar el acceso al recurso de Azure SignalR Service mediante Azure AD y OAuth:

### <a name="signalr-app-server"></a>Servidor de aplicaciones de SignalR

Use este rol para conceder acceso a fin de obtener una clave de acceso temporal para firmar tokens cliente.

### <a name="signalr-serverless-contributor"></a>Colaborador sin servidor de SignalR

Use este rol para conceder acceso a fin de obtener un token cliente directamente de Azure SignalR Service.

## <a name="next-steps"></a>Pasos siguientes

Consulte los artículos relacionados siguientes:

- [Autenticación de una aplicación con Azure AD para acceder a Azure SignalR Service](authenticate-application.md)
- [Autenticación de una identidad administrada con Azure AD para acceder a Azure SignalR Service](authenticate-managed-identity.md)