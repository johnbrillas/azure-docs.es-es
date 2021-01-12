---
title: Configuración del registro y del inicio de sesión con una cuenta de Weibo
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Weibo en las aplicaciones con Azure Active Directory B2C.
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
ms.openlocfilehash: 24e267b66d11cb3c5ca2b70ed09b7acb3653da99
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97653612"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-weibo-application"></a>Creación de una aplicación de Weibo

Para usar una cuenta de Weibo como proveedor de identidades en Azure Active Directory B2C (Azure AD B2C), debe crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de Weibo, puede suscribirse en [https://weibo.com/signup/signup.php?lang=en-us](https://weibo.com/signup/signup.php?lang=en-us).

1. Inicie sesión en el [portal para desarrolladores de Weibo](https://open.weibo.com/) con sus credenciales de cuenta de Weibo.
1. Después de iniciar sesión, seleccione el nombre para mostrar en la esquina superior derecha.
1. En el menú desplegable, seleccione **编辑开发者信息** (Editar información de desarrollador).
1. Escriba la información necesaria y luego seleccione **提交** (Enviar).
1. Complete el proceso de comprobación de correo electrónico.
1. Vaya a la [página de comprobación de identidad](https://open.weibo.com/developers/identity/edit).
1. Escriba la información necesaria y luego seleccione **提交** (Enviar).

### <a name="register-a-weibo-application"></a>Registro de una aplicación de Weibo

1. Vaya a la [página de registro de la aplicación de Weibo](https://open.weibo.com/apps/new).
1. Escriba la información de aplicación necesaria.
1. Seleccione **创建** (Crear).
1. Copie los valores de **Clave de la aplicación** y **Secreto de la aplicación**. Necesitará ambos para agregar el proveedor de identidades a su inquilino.
1. Cargue las fotos y escriba la información necesarias.
1. Seleccione **保存以上信息** (Guardar).
1. Seleccione **高级信息** (Información avanzada).
1. Seleccione **编辑** (Editar) junto al campo de OAuth2.0 **授权设置** (Dirección URL de redireccionamiento).
1. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en OAuth2.0 **授权设置** (Dirección URL de redireccionamiento). Por ejemplo, si el nombre de inquilino es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Seleccione **提交** (Enviar).

::: zone pivot="b2c-user-flow"

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Configuración de una cuenta de Weibo como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Weibo (versión preliminar)** .
1. Escriba un **nombre**. Por ejemplo, *Weibo*.
1. En **Id. de cliente**, escriba la clave de aplicación de Weibo que ha creado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de aplicación que ha anotado.
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
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `WeiboSecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
8. En **Secreto**, escriba el secreto de cliente que haya registrado previamente.
9. En **Uso de claves**, seleccione `Signature`.
10. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si quiere que los usuarios inicien sesión con una cuenta de Weibo, tendrá que definirla como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir una cuenta de Weibo como proveedor de notificaciones si la agrega al elemento **ClaimsProvider** del archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>Weibo.com</Domain>
      <DisplayName>Weibo</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Weibo-OAUTH">
          <DisplayName>Weibo</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">Weibo</Item>
            <Item Key="authorization_endpoint">https://accounts.Weibo.com/o/oauth2/auth</Item>
            <Item Key="AccessTokenEndpoint">https://accounts.Weibo.com/o/oauth2/token</Item>
            <Item Key="ClaimsEndpoint">https://www.Weiboapis.com/oauth2/v1/userinfo</Item>
            <Item Key="scope">email profile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="client_id">Your Weibo application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_WeiboSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="Weibo.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
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
    <ClaimsProvider>
      <Domain>weibo.com</Domain>
      <DisplayName>Weibo (Preview)</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Weibo-OAUTH">
          <DisplayName>Weibo</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">weibo</Item>
            <Item Key="authorization_endpoint">https://api.weibo.com/oauth2/authorize</Item>
            <Item Key="AccessTokenEndpoint">https://api.weibo.com/oauth2/access_token</Item>
            <Item Key="ClaimsEndpoint">https://api.weibo.com/2/account/get_uid.json</Item>
            <Item Key="scope">email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="external_user_identity_claim_id">uid</Item>
            <Item Key="client_id">Your Weibo application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_WeiboSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="numericUserId" PartnerClaimType="uid" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="weibo.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="displayName" DefaultValue="Weibo User" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="UserId" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateIssuerUserId" />
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
    ```

4. Establezca **client_id** en el identificador de la aplicación desde el registro de aplicación.
5. Guarde el archivo.

### <a name="add-the-claims-transformations"></a>Adición de transformaciones de notificaciones

Para el perfil técnico de GitHub es necesario agregar las transformaciones de notificaciones **CreateIssuerUserId** a la lista de ClaimsTransformations. Si no tiene definido el elemento **ClaimsTransformations** en el archivo, agregue los elementos XML primarios tal como se muestra a continuación. Las transformaciones de notificaciones también necesitan un nuevo tipo de notificación denominado **numericUserId**.

1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue la notificación numericUserId al elemento **ClaimsSchema**.
1. Busque el elemento [ClaimsTransformations](claimstransformations.md). Si el elemento no existe, agréguelo.
1. Agregue las transformaciones de notificaciones de CreateIssuerUserId al elemento **ClaimsTransformations**.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="numericUserId">
      <DisplayName>Numeric user Identifier</DisplayName>
      <DataType>long</DataType>
    </ClaimType>
  </ClaimsSchema>
  <ClaimsTransformations>
    <ClaimsTransformation Id="CreateIssuerUserId" TransformationMethod="ConvertNumberToStringClaim">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="issuerUserId" TransformationClaimType="outputClaim" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con la cuenta de Weibo. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

El proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro/inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de la plantilla existente y después modificarlo para que también tenga el proveedor de identidades de Weibo.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInWeibo`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón del proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de Weibo, se muestra un botón nuevo cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `WeiboExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="WeiboExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con una cuenta de Weibo para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó en **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="WeiboExchange" TechnicalProfileReferenceId="Weibo-OAuth" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `Weibo-OAuth`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-weibo-identity-provider-to-a-user-flow"></a>Adición del proveedor de identidades de Weibo a un flujo de usuario 

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario que quiera en el proveedor de identidades de Weibo.
1. En **Proveedores de identidades sociales**, seleccione **Weibo**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie el nombre a *SignUpSignInWeibo.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInWeibo`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_Weibo`
1. Actualice el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignWeibo).
1. Guarde los cambios y cargue el archivo.
1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. En **Seleccionar aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y elija Weibo para iniciar sesión con Weibo y probar la directiva personalizada.

::: zone-end
