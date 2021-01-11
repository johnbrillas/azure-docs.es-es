---
title: Configuración de un flujo de restablecimiento de contraseña
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo configurar un flujo de restablecimiento de contraseña en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/16/2020
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 6e6e4aa4ece781f415308b638323f8d4d4b34038
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618660"
---
# <a name="set-up-a-password-reset-flow-in-azure-active-directory-b2c"></a>Configuración de un flujo de restablecimiento de contraseña en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="password-rest-flow"></a>Flujo de restablecimiento de contraseña

La directiva de restablecimiento de contraseña permite a los usuarios restablecer su propia contraseña olvidada. El flujo de restablecimiento de contraseña consta de los siguientes pasos: 
1. En la página de registro e inicio de sesión, el usuario hace clic en la "¿Olvidó la contraseña?" que se mostrará. Azure AD B2C devuelve el código de error AADB2C90118 a la aplicación. La aplicación controla este código de error invocando la directiva de restablecimiento de contraseña. 
1. Los usuarios proporcionan y comprueban su correo electrónico con un código de acceso de tiempo de una hora.
1. Escriba una nueva contraseña.

![Flujo de restablecimiento de contraseña](./media/add-password-reset-policy/password-reset-flow.png)

## <a name="prerequisites"></a>Prerrequisitos

Si todavía no lo ha hecho, [registre una aplicación web en Azure Active Directory B2C](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-password-reset-user-flow"></a>Creación de un flujo de usuario de restablecimiento de contraseña

Para habilitar que los usuarios de la aplicación restablezcan la contraseña, se usa un flujo de usuario de restablecimiento de contraseña.

1. En el menú de información general del inquilino de Azure AD B2C, seleccione **Flujos de usuario** y **Nuevo flujo de usuario**.
1. En la página **Crear un flujo de usuario**, seleccione el flujo de usuario **Restablecer contraseña**. 
1. En **Seleccione una versión**, elija **Recomendada** y, luego, seleccione **Crear**.
1. Escriba un **nombre** para el flujo de usuario. Por ejemplo, *passwordreset1*.
1. En **Proveedores de identidades**, habilite **Reset password using email address** (Restablecer contraseña mediante la dirección de correo electrónico).
2. En Notificaciones de la aplicación, haga clic en **Mostrar más** y elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación. Por ejemplo, seleccione **Id. de objeto del usuario**.
3. Haga clic en **OK**.
4. Haga clic en **Crear** para agregar el flujo de usuario. El prefijo *B2C_1* se anexa automáticamente al nombre.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione el flujo de usuario que ha creado para abrir su página de información general y, luego, **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**, compruebe la dirección de correo electrónico de la cuenta que creó previamente y seleccione **Continuar**.
1. Ahora tiene la oportunidad de cambiar la contraseña para el usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-password-reset-policy"></a>Crear una directiva de restablecimiento de contraseña

Las directivas personalizadas son un conjunto de archivos XML que se cargan en el inquilino de Azure AD B2C para definir recorridos de usuario. Proporcionamos paquetes de inicio con varias directivas predefinidas, entre las que se incluyen: registro e inicio de sesión, restablecimiento de contraseña y directiva de edición de perfiles. Para obtener información, consulte [Introducción a las directivas personalizadas en Azure AD B2C](custom-policy-get-started.md).

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Configure un [flujo de edición de perfiles](add-profile-editing-policy.md).