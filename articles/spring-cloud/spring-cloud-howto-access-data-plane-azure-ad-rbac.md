---
title: Acceso al plano de datos con RBAC de Azure AD
description: Cómo obtener acceso al plano de datos con el control de acceso basado en roles de Azure Active Directory.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 02/04/2021
ms.custom: devx-track-java
ms.openlocfilehash: 2909d40fbc7cb5b310ea17f17a6e683ce47638ce
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102220099"
---
# <a name="access-the-data-plane-with-azure-active-directory-and-role-based-access-control"></a>Acceso al plano de datos con Azure Active Directory y el control de acceso basado en roles

En este artículo se explica cómo acceder a los puntos de conexión del registro del servicio y al servidor de configuración de Azure Spring Cloud mediante el control de acceso basado en roles (RBAC) de Azure Active Directory (AAD).

## <a name="assign-role-to-aad-usergroup-msi-or-service-principal"></a>Asignación de un rol a un usuario o un grupo de AAD, una MSI o una entidad de servicio

Para usar AAD y RBAC, debe asignar el rol *Lector de datos de Azure Spring Cloud* a un usuario, un grupo o una entidad de servicio mediante el siguiente procedimiento:

1. Vaya a la página de información general del servicio de la instancia de servicio.

2. Haga clic en **Control de acceso (IAM)** para abrir la hoja de control de acceso.

3. Haga clic en el botón **Agregar** y en **Agregar asignación de roles** (es posible que se necesite autorización para poder agregar).

4. Busque y seleccione *Lector de datos de Azure Spring Cloud* en **Rol**.
5. Asigne el acceso a `User, group, or service principal` o `User assigned managed identity` según el tipo de usuario. Busque y seleccione el usuario.  
6. Haga clic en `Save`

   ![Asignación de roles](media/access-data-plane-aad-rbac/assign-data-reader-role.png)

## <a name="access-data-plane"></a>Acceso al plano de datos

Una vez que un usuario de AAD tiene asignado el rol *Lector de datos de Azure Spring Cloud*, los clientes pueden iniciar sesión en la CLI de Azure con el usuario, la entidad de servicio o la identidad administrada.  Consulte [Inicio de sesión con la CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) para obtener un token de acceso.  A continuación, use los comandos siguientes.

```azurecli
az login
az account get-access-token
```

Actualmente, la CLI admite los puntos de conexión predeterminados del servidor de configuración y del registro del servicio. Para más información, consulte [Puntos de conexión listos para producción](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints). 

Los clientes también pueden obtener una lista completa de los puntos de conexión admitidos del servidor de configuración y del registro del servicio mediante el acceso a los puntos de conexión:
* *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/*
* *https://SERVICE_NAME.Root_Endpoint/config/actuator/* 

El token de acceso del encabezado proporciona la autorización. Solo se admite el método "GET".

Por ejemplo, acceda a un punto de conexión como *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/health* para ver el estado de mantenimiento de Eureka.

A continuación se muestran varios puntos de conexión raíz según los distintos tipos de nube.

| Nube          | Punto de conexión raíz              |
| -------------- | -------------------------- |
| Público         | svc.azuremicroservices.io  |
| Mooncake/China | svc.microservices.azure.cn |

Si la respuesta es *401 no autorizado*, compruebe si el rol se ha asignado correctamente.  El rol tardará varios minutos en surtir efecto o bien, compruebe que el token de acceso no ha expirado.

## <a name="next-steps"></a>Pasos siguientes
* [Inicio de sesión con la CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)
* [Puntos de conexión listos para producción](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)

## <a name="see-also"></a>Consulte también
* [Creación de roles y permisos](spring-cloud-howto-permissions.md)