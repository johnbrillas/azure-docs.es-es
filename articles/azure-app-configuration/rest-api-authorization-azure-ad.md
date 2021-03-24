---
title: 'API REST de Azure App Configuration: autorización de Azure Active Directory'
description: Use Azure Active Directory para la autorización en Azure App Configuration mediante la API REST
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: c028dcf6649da9abd196997d077b58386a5e6dd9
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101092797"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Autorización de Azure Active Directory: referencia de API REST

Cuando se usa la autenticación de Azure Active Directory (Azure AD), la autorización se controla mediante control de acceso basado en roles (RBAC). RBAC exige que se asignen roles los usuarios a los efectos de conceder acceso a los recursos. Cada rol contiene un conjunto de acciones que los usuarios asignados al rol pueden realizar.

## <a name="roles"></a>Roles

Los roles siguientes están disponibles en las suscripciones de Azure de manera predeterminada:

- **Propietario de los datos de Azure App Configuration**: este rol proporciona acceso completo a todas las operaciones.
- **Lector de los datos de Azure App Configuration**: este rol permite operaciones de lectura.

## <a name="actions"></a>Acciones

Los roles contienen una lista de las acciones que pueden realizar los usuarios asignados a ese rol. Azure App Configuration admite las siguientes acciones:

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`: esta acción permite el acceso de lectura a recursos de clave y valor de App Configuration, como /kv y /labels.
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`: esta acción permite el acceso de escritura a recursos de clave y valor de App Configuration.
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`: esta acción permite que se eliminen recursos de clave y valor de App Configuration. Tenga en cuenta que, al eliminar un recurso, se devuelven el valor y la clave que se han eliminado.

## <a name="error"></a>Error

```http
HTTP/1.1 403 Forbidden
```

**Motivo:** La entidad de seguridad que realiza la solicitud no tiene los permisos necesarios para llevar a cabo la operación solicitada.
**Solución:** asigne el rol necesario para realizar la operación solicitada a la entidad de seguridad que lleva a cabo la solicitud.

## <a name="managing-role-assignments"></a>Administración de asignaciones de roles

Puede administrar las asignaciones de roles mediante [procedimientos de Azure RBAC](../role-based-access-control/overview.md) que son estándar en todos los servicios de Azure. Puede hacerlo a través de la CLI de Azure, PowerShell y Azure Portal. Para más información, consulte [Asignación de roles de Azure mediante Azure Portal](../role-based-access-control/role-assignments-portal.md).