---
title: Configuración del registro y del inicio de sesión con una cuenta de Facebook
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión en sus aplicaciones a los clientes con cuentas de Facebook mediante Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/19/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 64a4404fa881181f92d442a73e5da4c16ae87ae3
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98598881"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-facebook-application"></a>Creación de una aplicación de Facebook

Para habilitar el inicio de sesión para los usuarios con una cuenta de Facebook en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en el [panel de aplicaciones de Facebook](https://developers.facebook.com/). Para obtener más información, consulte la página sobre [desarrollo de aplicaciones](https://developers.facebook.com/docs/development). Si aún no tiene una cuenta de Facebook, puede suscribirse en [https://www.facebook.com/](https://www.facebook.com/).

1. Inicie sesión en [Facebook for developers](https://developers.facebook.com/) con las credenciales de su cuenta de Facebook.
1. Si aún no lo ha hecho, debe registrarse como desarrollador de Facebook. Para ello, seleccione **Get Started** (Comenzar) en la esquina superior derecha de la página, acepte las directivas de Facebook y complete los pasos de registro.
1. Seleccione **Mis aplicaciones** y después **Crear una aplicación**.
1. Seleccione **Crear experiencias conectadas**.
1. Especifique el valor de **Display Name** (Nombre para mostrar) y un valor de **Contact Email** (Correo electrónico de contacto) válido.
1. Seleccione **Create App ID** (Crear identificador de aplicación). Es posible que deba aceptar las políticas de la plataforma Facebook y realizar una comprobación de seguridad en línea.
1. Seleccione **Settings** (Configuración)  > **Basic** (Básica).
    1. Elija una **Categoría**, por ejemplo, `Business and Pages`. Este valor es obligatorio para Facebook, pero no se usa para Azure AD B2C.
    1. Escriba una dirección URL para la **URL de Condiciones del servicio**, por ejemplo `http://www.contoso.com/tos`. La dirección URL de la directiva es una página que se mantiene para proporcionar los términos y condiciones de la aplicación.
    1. Escriba una dirección URL en **Privacy Policy URL** (URL de directiva de privacidad), por ejemplo `http://www.contoso.com/privacy`. La dirección URL de directiva es una página que sirve para proporcionar información de privacidad de la aplicación.
1. En la parte inferior de la página, seleccione **Add Platform** (Agregar plataforma) y, después, seleccione **Website** (Sitio web).
1. En **URL del sitio**, escriba la dirección del sitio web, por ejemplo `https://contoso.com`. 
1. Seleccione **Save changes** (Guardar los cambios).
1. En la parte superior de la página, copie el valor de **App ID** (Id. de la aplicación).
1. Seleccione **Show** (Mostrar) y copie el valor de **App Secret** (Secreto de la aplicación). Use ambos para configurar Facebook como proveedor de identidades de su inquilino. El **secreto de la aplicación** es una credencial de seguridad importante.
1. En el menú, seleccione el signo **más** junto a **PRODUCTOS**. En **Add Products to Your App** (Agregar productos a la aplicación), seleccione **Set up** (Configurar) en **Facebook Login** (Inicio de sesión de Facebook).
1. En el menú, seleccione **Facebook Login** (Inicio de sesión de Facebook) y después **Settings** (Configuración).
1. En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino. Seleccione **Save Changes** (Guardar cambios) en la parte inferior de la página.
1. Para que la aplicación de Facebook esté disponible para Azure AD B2C, seleccione el selector de Estado situado en la parte superior derecha de la página y **actívelo** para hacer que la aplicación sea pública y, después, seleccione **Switch Mode** (Modo de conmutador).  En este momento el estado debería cambiar de **Desarrollo** a **Activo**.

::: zone pivot="b2c-user-flow"

## <a name="configure-facebook-as-an-identity-provider"></a>Configuración de Facebook como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Facebook**.
1. Escriba un **nombre**. Por ejemplo, *Facebook*.
1. En **Id. de cliente**, escriba el identificador de aplicación de Facebook que ha creado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de aplicación que ha anotado.
1. Seleccione **Guardar**.

## <a name="add-facebook-identity-provider-to-a-user-flow"></a>Adición de un proveedor de identidades de Facebook a un flujo de usuario 

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario que quiera agregar al proveedor de identidades de Facebook.
1. En **Social identity providers** (Proveedores de identidades sociales), seleccione **Facebook**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configuración de una cuenta de Facebook como proveedor de identidades

1. En el archivo `SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`** , sustituya el valor de `client_id` por el identificador de aplicación de Facebook:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

## <a name="upload-and-test-the-policy"></a>Cargue y pruebe la directiva.

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Cargue el archivo *TrustFrameworkExtensions.xml* en el inquilino.
1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. En **Seleccionar aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y elija Facebook para iniciar sesión con Facebook y probar la directiva personalizada.

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [pasar el token de Facebook a la aplicación](idp-pass-through-user-flow.md).
