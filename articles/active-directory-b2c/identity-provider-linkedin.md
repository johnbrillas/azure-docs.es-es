---
title: Configuración del registro y del inicio de sesión con una cuenta de LinkedIn
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de LinkedIn en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/17/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: bde7c1adefea88ed5b5d86e2c0e17f475be1bc71
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654377"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de LinkedIn mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-linkedin-application"></a>Creación de una aplicación de LinkedIn

Para usar una cuenta de LinkedIn como [proveedor de identidades](authorization-code-flow.md) en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de LinkedIn, puede obtenerla en [https://www.linkedin.com/](https://www.linkedin.com/).

1. Inicie sesión en el [sitio web para desarrolladores de LinkedIn](https://www.developer.linkedin.com/) con las credenciales de su cuenta de LinkedIn.
1. Seleccione **My Apps** (Mis aplicaciones) y, a continuación, haga clic en **Create Application** (Crear aplicación).
1. Escriba lo que corresponda en **App name** (Nombre de la aplicación), **LinkedIn Page** (Página de LinkedIn), **Privacy policy URL** (Dirección URL de la directiva de privacidad) y **App logo** (Logotipo de la aplicación).
1. Acepte las **condiciones de uso de API** de LinkedIn y haga clic en **Create app** (Crear aplicación).
1. Seleccione la pestaña **Autenticación**. En **Authentication Keys** (Claves de autenticación), copie los valores de **Client ID** (Id. de cliente) y **Client Secret** (Secreto de cliente). Necesitará ambos para configurar LinkedIn como proveedor de identidades de su inquilino. **secreto de cliente** es una credencial de seguridad importante.
1. Seleccione el lápiz de edición situado junta a **Authorized redirect URLs for your app** (Direcciones URL de redireccionamiento autorizadas para la aplicación) y, después, seleccione **Add redirect URL** (Agregar dirección URL de redireccionamiento). Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` y reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C. Seleccione **Actualizar**.
2. De forma predeterminada, la aplicación de LinkedIn no está aprobada para los ámbitos relacionados con el inicio de sesión. Para solicitar una revisión, seleccione la pestaña **Products** (Productos) y, a continuación, seleccione **Sign In with LinkedIn** (Iniciar sesión con LinkedIn). Una vez completada la revisión, los ámbitos requeridos se agregarán a la aplicación.
   > [!NOTE]
   > Puede ver los ámbitos permitidos actualmente para su aplicación en la pestaña **Auth** de la sección **OAuth 2.0 scopes** (Ámbitos de OAuth 2.0).

::: zone pivot="b2c-user-flow"

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Configuración de una cuenta de LinkedIn como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **LinkedIn**.
1. Escriba un **nombre**. Por ejemplo, *LinkedIn*.
1. En **Id. de cliente**, escriba el identificador de cliente de LinkedIn que creó anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado.
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
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `LinkedInSecret`. Se agregará el prefijo *B2C_1A_* automáticamente al nombre de la clave.
8. En **Secreto**, escriba el secreto de cliente que haya registrado previamente.
9. En **Uso de claves**, seleccione `Signature`.
10. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si desea que los usuarios inicien sesión con una cuenta de LinkedIn, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Defina una cuenta de LinkedIn como proveedor de notificaciones; para ello, agréguela al elemento **ClaimsProvider** en el archivo de extensión de la directiva.

1. Abra el archivo *SocialAndLocalAccounts/**TrustFrameworkExtensions.xml** _ en el editor. Este archivo se encuentra en el [paquete de inicio de directivas personalizadas][starter-pack] que descargó como parte de uno de los requisitos previos.
1. Busque el elemento _ *ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
1. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v2/me</Item>
            <Item Key="scope">r_emailaddress r_liteprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="external_user_identity_claim_id">id</Item>
            <Item Key="BearerTokenTransmissionMethod">AuthorizationHeader</Item>
            <Item Key="ResolveJsonPathsInJsonTokens">true</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <InputClaims />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName.localized" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName.localized" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="ExtractGivenNameFromLinkedInResponse" />
            <OutputClaimsTransformation ReferenceId="ExtractSurNameFromLinkedInResponse" />
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

1. Reemplace el valor de **client_id** por el id. de cliente de la aplicación de LinkedIn que haya registrado previamente.
1. Guarde el archivo.

### <a name="add-the-claims-transformations"></a>Adición de transformaciones de notificaciones

El perfil técnico de LinkedIn requiere agregar las transformaciones de notificaciones **ExtractGivenNameFromLinkedInResponse** y **ExtractSurNameFromLinkedInResponse** a la lista de ClaimsTransformations. Si no tiene definido el elemento **ClaimsTransformations** en el archivo, agregue los elementos XML primarios tal como se muestra a continuación. Las transformaciones de notificaciones también necesitan un nuevo tipo de notificación denominado **nullStringClaim**.

Agregue el elemento **BuildingBlocks** cerca de la parte superior del archivo *TrustFrameworkExtensions.xml*. Consulte *TrustFrameworkBase.xml* para ver un ejemplo.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <!-- Claim type needed for LinkedIn claims transformations -->
    <ClaimType Id="nullStringClaim">
      <DisplayName>nullClaim</DisplayName>
      <DataType>string</DataType>
      <AdminHelpText>A policy claim to store output values from ClaimsTransformations that aren't useful. This claim should not be used in TechnicalProfiles.</AdminHelpText>
      <UserHelpText>A policy claim to store output values from ClaimsTransformations that aren't useful. This claim should not be used in TechnicalProfiles.</UserHelpText>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <!-- Claim transformations needed for LinkedIn technical profile -->
    <ClaimsTransformation Id="ExtractGivenNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
    <ClaimsTransformation Id="ExtractSurNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="surname" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Ahora tiene configurado una directiva para que Azure AD B2C sepa cómo comunicarse con su cuenta de LinkedIn. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
1. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
1. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este punto, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro o de inicio de sesión. Para que esté disponible, creará un duplicado de un recorrido del usuario de plantilla existente y, después, lo modificará para que también tenga el proveedor de identidades de LinkedIn.

1. Abra el archivo *TrustFrameworkBase.xml* en el paquete de inicio.
1. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
1. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
1. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInLinkedIn`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de LinkedIn, se muestra un nuevo botón cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelections**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `LinkedInExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de LinkedIn para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
1. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó para **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `LinkedIn-OAUTH`.

1. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

::: zone-end

:: zone pivot="b2c-user-flow"

## <a name="add-linkedin-identity-provider-to-a-user-flow"></a>Agregación del proveedor de identidades de LinkedIn a un flujo de usuario 

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario que quiera en el proveedor de identidades de LinkedIn.
1. En **Social identity providers** (Proveedores de identidades sociales), seleccione **LinkedIn**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *SignUpSignInLinkedIn.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInLinkedIn`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_linkedin`
1. Cambie el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignLinkedIn).
1. Guarde los cambios, cargue el archivo y seleccione la nueva directiva en la lista.
1. Asegúrese de que la aplicación de Azure AD B2C que creó está seleccionada en el campo **Seleccionar aplicación** y pruébela; para ello, haga clic en **Ejecutar ahora**.


## <a name="migration-from-v10-to-v20"></a>Migración desde la versión 1.0 a la 2.0

Recientemente, LinkedIn [actualizó sus API de la versión 1.0 a 2.0](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Para migrar la configuración existente a la nueva configuración, use la información de las secciones siguientes para actualizar los elementos del perfil técnico.

### <a name="replace-items-in-the-metadata"></a>Sustitución de los elementos en los metadatos

En el elemento **Metadata** existente de **TechnicalProfile**, actualice los siguientes elementos **Item** de:

```xml
<Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
<Item Key="scope">r_emailaddress r_basicprofile</Item>
```

Por:

```xml
<Item Key="ClaimsEndpoint">https://api.linkedin.com/v2/me</Item>
<Item Key="scope">r_emailaddress r_liteprofile</Item>
```

### <a name="add-items-to-the-metadata"></a>Adición de elementos a los metadatos

En el elemento **Metadata** de **TechnicalProfile**, agregue los siguientes elementos **Item**:

```xml
<Item Key="external_user_identity_claim_id">id</Item>
<Item Key="BearerTokenTransmissionMethod">AuthorizationHeader</Item>
<Item Key="ResolveJsonPathsInJsonTokens">true</Item>
```

### <a name="update-the-outputclaims"></a>Actualización de OutputClaims

En el elemento **Metadata** existente de **TechnicalProfile**, actualice los siguientes elementos **OutputClaim** de:

```xml
<OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
<OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
```

Por:

```xml
<OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName.localized" />
<OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName.localized" />
```

### <a name="add-new-outputclaimstransformation-elements"></a>Adición de nuevos elementos OutputClaimsTransformation

En el elemento **OutputClaimsTransformations** de **TechnicalProfile**, agregue los siguientes elementos **OutputClaimsTransformation**:

```xml
<OutputClaimsTransformation ReferenceId="ExtractGivenNameFromLinkedInResponse" />
<OutputClaimsTransformation ReferenceId="ExtractSurNameFromLinkedInResponse" />
```

### <a name="define-the-new-claims-transformations-and-claim-type"></a>Definición de las nuevas transformaciones de notificaciones y del tipo de notificación

En el último paso, ha agregado nuevas transformaciones de notificaciones que se deben definir. Para definir las transformaciones de notificaciones, debe agregarlas a la lista de **ClaimsTransformations**. Si no tiene definido el elemento **ClaimsTransformations** en el archivo, agregue los elementos XML primarios tal como se muestra a continuación. Las transformaciones de notificaciones también necesitan un nuevo tipo de notificación denominado **nullStringClaim**.

El elemento **BuildingBlocks** se debe agregar cerca de la parte superior del archivo. Consulte *TrustframeworkBase.xml* como ejemplo.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <!-- Claim type needed for LinkedIn claims transformations -->
    <ClaimType Id="nullStringClaim">
      <DisplayName>nullClaim</DisplayName>
      <DataType>string</DataType>
      <AdminHelpText>A policy claim to store unuseful output values from ClaimsTransformations. This claim should not be used in a TechnicalProfiles.</AdminHelpText>
      <UserHelpText>A policy claim to store unuseful output values from ClaimsTransformations. This claim should not be used in a TechnicalProfiles.</UserHelpText>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <!-- Claim transformations needed for LinkedIn technical profile -->
    <ClaimsTransformation Id="ExtractGivenNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
    <ClaimsTransformation Id="ExtractSurNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="surname" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

### <a name="obtain-an-email-address"></a>Obtención de una dirección de correo electrónico

Como parte de la migración de LinkedIn desde la versión 1.0 a la 2.0, se requiere una llamada adicional a otra API para obtener la dirección de correo electrónico. Si necesita obtener la dirección de correo electrónico durante el registro, realice lo siguiente:

1. Complete los pasos anteriores para permitir que Azure AD B2C se federe con LinkedIn para permitir al usuario iniciar sesión. Como parte de la federación, Azure AD B2C recibe el token de acceso de LinkedIn.
1. Guarde el token de acceso de LinkedIn en una notificación. [Vea las instrucciones aquí](idp-pass-through-user-flow.md).
1. Agregue el siguiente proveedor de notificaciones que realiza la solicitud a la API `/emailAddress` de LinkedIn. Para autorizar esta solicitud, necesita el token de acceso de LinkedIn.

    ```xml
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="API-LinkedInEmail">
          <DisplayName>Get LinkedIn email</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
              <Item Key="ServiceUrl">https://api.linkedin.com/v2/emailAddress?q=members&amp;projection=(elements*(handle~))</Item>
              <Item Key="AuthenticationType">Bearer</Item>
              <Item Key="UseClaimAsBearerToken">identityProviderAccessToken</Item>
              <Item Key="SendClaimsIn">Url</Item>
              <Item Key="ResolveJsonPathsInJsonTokens">true</Item>
          </Metadata>
          <InputClaims>
              <InputClaim ClaimTypeReferenceId="identityProviderAccessToken" />
          </InputClaims>
          <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="elements[0].handle~.emailAddress" />
          </OutputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Agregue el siguiente paso de orquestación en el recorrido del usuario, para que el proveedor de notificaciones de API se desencadene cuando un usuario inicie sesión con LinkedIn. Recuerde actualizar el número `Order` según corresponda. Agregue este paso inmediatamente después del paso de orquestación que desencadena el perfil técnico de LinkedIn.

    ```xml
    <!-- Extra step for LinkedIn to get the email -->
    <OrchestrationStep Order="3" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
          <Value>identityProvider</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="false">
          <Value>identityProvider</Value>
          <Value>linkedin.com</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="GetEmail" TechnicalProfileReferenceId="API-LinkedInEmail" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ```

La obtención de la dirección de correo electrónico de LinkedIn durante el registro es opcional. Si decide no obtener la dirección de correo electrónico de LinkedIn, pero si requiere una durante el registro, el usuario deberá escribir manualmente la dirección de correo electrónico y validarla.

Para ver un ejemplo completo de una directiva que usa el proveedor de identidades de LinkedIn, consulte el [paquete de inicio de directivas personalizadas](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/linkedin-identity-provider).

<!-- Links - EXTERNAL -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="migration-from-v10-to-v20"></a>Migración desde la versión 1.0 a la 2.0

Recientemente, LinkedIn [actualizó sus API de la versión 1.0 a 2.0](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Como parte de la migración, Azure AD B2C solo es capaz de obtener el nombre completo del usuario de LinkedIn durante la suscripción. Si una dirección de correo electrónico es uno de los atributos que se recopilan durante el proceso de suscripción, el usuario debe escribir manualmente la dirección de correo electrónico y validarla.

::: zone-end
