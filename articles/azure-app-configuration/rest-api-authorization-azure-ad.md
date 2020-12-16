---
title: 'API REST de Azure App Configuration: autorización de Azure Active Directory'
description: Use Azure Active Directory para la autorización en Azure App Configuration mediante la API REST
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: f144c1f09f87e0b915daf86ba0391c2934e60095
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96932683"
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

Puede administrar las asignaciones de roles mediante [procedimientos de RBAC](../role-based-access-control/overview.md) que son estándar en todos los servicios de Azure. Puede hacerlo a través de la CLI de Azure, PowerShell y Azure Portal. Para obtener más información, consulte [Incorporación o eliminación de asignaciones de roles mediante Azure Portal](../role-based-access-control/role-assignments-portal.md).