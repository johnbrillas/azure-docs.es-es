---
title: Configuración de un flujo de restablecimiento de contraseña forzado en Azure AD B2C
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo configurar un flujo de restablecimiento de contraseña forzado en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/03/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 0477153b466702bec0fa2d5221fee1e054d06314
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102033781"
---
# <a name="set-up-a-force-password-reset-flow-in-azure-active-directory-b2c"></a>Configuración de un flujo de restablecimiento de contraseña forzado en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Como administrador, puede [restablecer la contraseña de un usuario](manage-users-portal.md#reset-a-users-password) si este no la recuerda. También puede forzar al usuario a que restablezca la contraseña. En este artículo, aprenderá a forzar un restablecimiento de contraseña en estos escenarios.

## <a name="overview"></a>Información general

Cuando un administrador restablece la contraseña de un usuario desde Azure Portal, el valor del atributo [forceChangePasswordNextSignIn](user-profile-attributes.md#password-profile-property) se establece en `true`.

El [proceso de inicio de sesión y registro](add-sign-up-and-sign-in-policy.md) comprueba el valor de este atributo. Una vez que el usuario completa el inicio de sesión, si el atributo está establecido en `true`, el usuario debe restablecer su contraseña. Después, el valor del atributo se vuelve a establecer en `false`.

![Flujo de restablecimiento de contraseña forzado](./media/force-password-reset/force-password-reset-flow.png)

El flujo de restablecimiento de contraseña es aplicable a las cuentas locales de Azure AD B2C que utilizan una [dirección de correo electrónico](identity-provider-local.md#email-sign-in) o un [nombre de usuario](identity-provider-local.md#username-sign-in) con una contraseña para el inicio de sesión.

### <a name="force-a-password-reset-after-90-days"></a>Restablecimiento de contraseña forzado después de 90 días

Como administrador, puede establecer la expiración de la contraseña de un usuario en 90 días, mediante [MS Graph](microsoft-graph-operations.md). Después de 90 días, el valor del atributo [forceChangePasswordNextSignIn](user-profile-attributes.md#password-profile-property) se establece automáticamente en `true`. Para obtener más información sobre cómo establecer la directiva de expiración de la contraseña de un usuario, vea [Atributo de directiva de contraseñas](user-profile-attributes.md#password-policy-attribute).

Una vez que se ha establecido una directiva de expiración de contraseña, también debe configurar el flujo de restablecimiento de contraseña, tal como se describe en este artículo.  

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="configure-your-policy"></a>Configuración de la directiva

::: zone pivot="b2c-user-flow"

Para habilitar la configuración de **Restablecimiento de la contraseña forzado** en un flujo de usuario de registro o de inicio de sesión:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Seleccione el flujo de usuario de registro e inicio de sesión o solo de inicio de sesión (de tipo **Recomendado**) que desea personalizar.
1. En el menú de la izquierda, en **Configuración**, seleccione **Propiedades**.
1. En **Complejidad de la contraseña**, seleccione **Restablecimiento de la contraseña forzado**.
1. Seleccione **Guardar**.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de usuarios o como administrador de contraseñas. Para más información acerca de los roles disponibles, consulte [Asignación de roles de administrador en Azure Active Directory](../active-directory/roles/permissions-reference.md#all-roles).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Usuarios**. Busque y seleccione el usuario que va a usar para probar el restablecimiento de la contraseña y, después, seleccione **Restablecer contraseña**.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Seleccione un flujo de usuario de registro o de inicio de sesión (de tipo **Recomendado**) que desee probar.
1. Seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar flujo de usuario**.
1. Inicie sesión con la cuenta de usuario para la que desea restablecer la contraseña.
1. Ahora debe cambiar la contraseña del usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

::: zone-end

::: zone pivot="b2c-custom-policy"

1. Obtenga el ejemplo de un restablecimiento de contraseña forzada en [GitHub](https://github.com/azure-ad-b2c/samples/tree/master/policies/force-password-reset).
1. En cada archivo, reemplace la cadena `yourtenant` por el nombre del inquilino de Azure AD B2C. Por ejemplo, si el nombre del inquilino de B2C es *contosob2c*, todas las instancias de `yourtenant.onmicrosoft.com` se convierten en `contosob2c.onmicrosoft.com`.
1. Cargue los archivos de directivas en el siguiente orden: la directiva de extensiones `TrustFrameworkExtensionsCustomForcePasswordReset.xml` y luego la directiva de usuarios de confianza `SignUpOrSigninCustomForcePasswordReset.xml`.

### <a name="test-the-policy"></a>Prueba de la directiva

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de usuarios o como administrador de contraseñas. Para más información acerca de los roles disponibles, consulte [Asignación de roles de administrador en Azure Active Directory](../active-directory/roles/permissions-reference.md#all-roles).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Usuarios**. Busque y seleccione el usuario que va a usar para probar el restablecimiento de la contraseña y, después, seleccione **Restablecer contraseña**.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione la directiva `B2C_1A_signup_signin_Custom_ForcePasswordReset` para abrirla. 
1. En **Aplicación**, seleccione la aplicación web que [registró anteriormente](troubleshoot-custom-policies.md#troubleshoot-the-runtime). La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione el botón **Ejecutar ahora**.
1. Inicie sesión con la cuenta de usuario para la que desea restablecer la contraseña.
1. Ahora debe cambiar la contraseña del usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Configuración de un [autoservicio de restablecimiento de contraseña](add-password-reset-policy.md)
