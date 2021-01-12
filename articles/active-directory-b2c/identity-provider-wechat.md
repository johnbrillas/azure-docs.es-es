---
title: Configuración del registro y del inicio de sesión con una cuenta de WeChat
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de WeChat en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: af840a7becb0fb2c23e01153828458ee1cfa51a1
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654241"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de WeChat mediante Azure Active Directory B2C


[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-wechat-application"></a>Creación de una aplicación de WeChat

Para usar una cuenta de WeChat como proveedor de identidades en Azure Active Directory B2C (Azure AD B2C), debe crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de WeChat, puede obtener información en [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registro de una aplicación de WeChat

1. Inicie sesión en [https://open.weixin.qq.com/](https://open.weixin.qq.com/) con sus credenciales de WeChat.
1. Seleccione **管理中心**(centro de administración).
1. Siga los pasos para registrar una nueva aplicación.
1. Escriba `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **授权回调域** (Dirección URL de devolución de llamada). Por ejemplo, si el nombre de inquilino es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Copie el valor de **APP ID** (Identificador de la aplicación) y de **APP KEY** (Clave de la aplicación). Los necesitará para agregar el proveedor de identidades a su inquilino.

::: zone pivot="b2c-user-flow"

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configuración de WeChat como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **WeChat (versión preliminar)** .
1. Escriba un **nombre**. Por ejemplo, *WeChat*.
1. En **Id. de cliente**, escriba el identificador de aplicación de WeChat que ha creado anteriormente.
1. En **Secreto de cliente**, escriba la clave de aplicación que ha anotado.
1. Seleccione **Guardar**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar el secreto de cliente que haya registrado previamente en el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripciones** del menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. En la página de introducción, seleccione **Identity Experience Framework**.
5. Seleccione **Claves de directiva** y luego **Agregar**.
6. En **Opciones**, elija `Manual`.
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `WeChatSecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
8. En **Secreto**, escriba el secreto de cliente que haya registrado previamente.
9. En **Uso de claves**, seleccione `Signature`.
10. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si quiere que los usuarios inicien sesión con una cuenta de WeChat, tendrá que definirla como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir una cuenta de WeChat como proveedor de notificaciones si la agrega al elemento **ClaimsProvider** del archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>wechat.com</Domain>
      <DisplayName>WeChat (Preview)</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="WeChat-OAUTH">
          <DisplayName>WeChat</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">wechat</Item>
            <Item Key="authorization_endpoint">https://open.weixin.qq.com/connect/qrconnect</Item>
            <Item Key="AccessTokenEndpoint">https://api.weixin.qq.com/sns/oauth2/access_token</Item>
            <Item Key="ClaimsEndpoint">https://api.weixin.qq.com/sns/userinfo</Item>
            <Item Key="scope">snsapi_login</Item>
            <Item Key="HttpBinding">GET</Item>
            <Item Key="AccessTokenResponseFormat">json</Item>
            <Item Key="ClientIdParamName">appid</Item>
            <Item Key="ClientSecretParamName">secret</Item>
            <Item Key="ExtraParamsInAccessTokenEndpointResponse">openid</Item>
            <Item Key="ExtraParamsInClaimsEndpointRequest">openid</Item>
            <Item Key="ResponseErrorCodeParamName">errcode</Item>
            <Item Key="external_user_identity_claim_id">unionid</Item>
          <Item Key="client_id">Your WeChat application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_WeChatSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="UserId" PartnerClaimType="unionid" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="wechat.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Establezca **client_id** en el identificador de la aplicación desde el registro de aplicación.
5. Guarde el archivo.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con la cuenta de WeChat. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

El proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro/inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de la plantilla existente y después modificarlo para que también tenga el proveedor de identidades de WeChat.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInWeChat`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón del proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de WeChat, se muestra un botón nuevo cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `WeChatExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="WeChatExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con una cuenta de WeChat para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó en **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="WeChatExchange" TechnicalProfileReferenceId="WeChat-OAuth" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `WeChat-OAuth`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-wechat-identity-provider-to-a-user-flow"></a>Adición del proveedor de identidades de WeChat a un flujo de usuario 

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario que quiera en el proveedor de identidades de WeChat.
1. En **Proveedores de identidades sociales**, seleccione **WeChat**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie el nombre a *SignUpSignInWeChat.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInWeChat`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_WeChat`
1. Actualice el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignWeChat).
1. Guarde los cambios y cargue el archivo.
1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. En **Seleccionar aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y elija WeChat para iniciar sesión con WeChat y probar la directiva personalizada.

::: zone-end
