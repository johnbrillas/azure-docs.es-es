---
title: Restricción de la aplicación de Azure AD a un conjunto de usuarios | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a restringir el acceso a las aplicaciones registradas en Azure AD a un conjunto de usuarios seleccionado.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f3eff6f5660089472ef431bad9a4f05be1fffe24
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100104152"
---
# <a name="how-to-restrict-your-azure-ad-app-to-a-set-of-users-in-an-azure-ad-tenant"></a>Procedimientos: Restricción de la aplicación de Azure AD a un conjunto de usuarios de un inquilino de Azure AD

Las aplicaciones registradas en un inquilino de Azure Active Directory (Azure AD) están disponibles de forma predeterminada para todos los usuarios del inquilino que se autentica correctamente.

Igualmente, en el caso de una aplicación [multiinquilino](howto-convert-app-to-be-multi-tenant.md), todos los usuarios del inquilino de Azure AD donde se aprovisiona esta aplicación podrán acceder a ella una vez que se autentiquen correctamente en sus respectivos inquilinos.

Los desarrolladores y administradores de inquilinos tienen con frecuencia el requisito de restringir una aplicación a un determinado conjunto de usuarios. Los desarrolladores pueden conseguir lo mismo mediante patrones de autorización conocidos, como el control de acceso basado en rol de Azure (Azure RBAC), pero este enfoque exige una importante cantidad de trabajo por parte del desarrollador.

Los administradores y desarrolladores de inquilinos pueden restringir una aplicación a un conjunto específico de usuarios o a grupos de seguridad del inquilino mediante esta característica integrada de Azure AD también.

## <a name="supported-app-configurations"></a>Configuraciones de aplicación admitidas

La opción para restringir una aplicación a un conjunto específico de usuarios o a los grupos de seguridad de un inquilino funciona con los siguientes tipos de aplicaciones:

- Aplicaciones configuradas para el inicio de sesión único federado con autenticación basada en SAML.
- Aplicaciones de proxy de aplicación que usan la autenticación previa de Azure AD.
- Aplicaciones integradas directamente en la plataforma de aplicaciones de Azure AD que usan la autenticación de OAuth 2.0 u OpenID Connect después de que un usuario o un administrador han dado su consentimiento a esa aplicación.

     > [!NOTE]
     > Esta característica está disponible únicamente para aplicaciones web o API web y aplicaciones empresariales. Las aplicaciones que se registran como [nativas](./quickstart-register-app.md) no se pueden restringir a un conjunto de usuarios o a los grupos de seguridad del inquilino.

## <a name="update-the-app-to-enable-user-assignment"></a>Actualización de la aplicación para permitir la asignación de usuarios

Hay dos maneras de crear una aplicación con la asignación de usuarios habilitada. Una necesita el rol **Administrador global**, la otra no.

### <a name="enterprise-applications-requires-the-global-administrator-role"></a>Aplicaciones empresariales (necesita el rol Administrador global)

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a> como **Administrador global**.
1. Si tiene acceso a varios inquilinos, use el filtro **Directorio + suscripción** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: del menú superior para seleccionar el inquilino en el que desea registrar una aplicación.
1. Busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista, seleccione la aplicación que quiere asignar a un usuario o a un grupo de seguridad. 
    Utilice los filtros de la parte superior de la ventana para buscar una aplicación específica.
1. En la página **Información general** de la aplicación, en **Administrar**, seleccione **Propiedades**.
1. Busque el valor **¿Asignación de usuarios?** y establézcalo en **Sí**. Cuando esta opción se establece en **Sí**, los usuarios del inquilino se deben asignar primero a esta aplicación o no podrán iniciar sesión en esta aplicación.
1. Seleccione **Guardar**.

### <a name="app-registration"></a>Registro de la aplicación

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>.
1. Si tiene acceso a varios inquilinos, use el filtro **Directorio + suscripción** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: del menú superior para seleccionar el inquilino en el que desea registrar una aplicación.
1. Busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones**.
1. Cree o seleccione la aplicación que quiere administrar. Debe ser el **propietario** de esta aplicación.
1. En la página **Información general** de la aplicación, seleccione **Aplicación administrada en directorio local** en la sección **Essentials**.
1. En **Administrar**, seleccione **Propiedades**.
1. Busque el valor **¿Asignación de usuarios?** y establézcalo en **Sí**. Cuando esta opción se establece en **Sí**, los usuarios del inquilino se deben asignar primero a esta aplicación o no podrán iniciar sesión en esta aplicación.
1. Seleccione **Guardar**.

## <a name="assign-users-and-groups-to-the-app"></a>Asignación de usuarios y grupos a la aplicación

Una vez que ha configurado la aplicación para permitir la asignación de usuarios, puede seguir adelante y asignar usuarios y grupos a la aplicación.

1. En **Administrar**, seleccione **Usuarios y grupos** > **Add user/group** (Agregar usuario o grupo).
1. Elija el selector **Usuarios**. 

     Se mostrará una lista de usuarios y grupos de seguridad junto con un cuadro de texto para buscar un usuario o grupo determinado. Esta pantalla permite seleccionar varios usuarios y grupos en una sola operación.

1. Cuando haya terminado de seleccionar los usuarios y grupos, elija **Seleccionar**.
1. (Opcional) Si ha definido roles de aplicación en su aplicación, puede usar la opción **Seleccionar rol** para asignar los usuarios y los grupos seleccionados a uno de los roles de la aplicación. 
1. Seleccione **Asignar** para completar las asignaciones de usuarios y grupos a la aplicación. 
1. Confirme que los usuarios y grupos que agregó se muestran en la lista **Usuarios y grupos** actualizada.

## <a name="more-information"></a>Más información

- [Cómo: Adición de roles de aplicación a la aplicación](./howto-add-app-roles-in-azure-ad-apps.md)
- [Incorporación de autorización mediante roles de aplicación y notificaciones de roles a una aplicación web de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Uso de grupos de seguridad y roles de aplicación en las aplicaciones (vídeo)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, ahora con notificaciones de grupo y roles de aplicación](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Manifiesto de la aplicación de Azure Active Directory](./reference-app-manifest.md)
