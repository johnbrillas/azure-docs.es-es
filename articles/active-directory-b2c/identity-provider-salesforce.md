---
title: Configuración del registro y el inicio de sesión con una cuenta de Salesforce
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Salesforce en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/05/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 129809a83bcebdcf80b05a7300dd9acf862e5886
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97900406"
---
# <a name="set-up-sign-up-and-sign-in-with-a-salesforce-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Salesforce mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


## <a name="create-a-salesforce-application"></a>Creación de una aplicación de Salesforce

Para usar una cuenta de Salesforce en Azure Active Directory B2C (Azure  AD B2C), debe crear una aplicación en el **administrador de aplicaciones** de Salesforce App Manager. Para obtener más información, consulte [Configurar ajustes de aplicaciones conectadas básicos](https://help.salesforce.com/articleView?id=connected_app_create_basics.htm) y [Activar configuración de OAuth para integración de API](https://help.salesforce.com/articleView?id=connected_app_create_api_integration.htm).

1. [Inicie sesión en Salesforce](https://login.salesforce.com/).
1. En el menú, seleccione **Setup** (Configuración).
1.  Expanda **Apps** (Aplicaciones) y, a continuación, seleccione **App Manager** (Administrador de aplicaciones).
1. Seleccione **New Connected App** (Nueva aplicación conectada).
1. En **Basic Information** (Información básica), escriba:
    1. **Connected App Name** (Nombre de la aplicación conectada): el nombre de la aplicación conectada se muestra en el administrador de aplicaciones y en su icono de App Launcher (Iniciador de aplicaciones). El nombre debe ser único dentro de una organización. 
    1. **API Name** (Nombre de la API) 
    1. **Contact Email** (Correo electrónico de contacto): el correo electrónico de contacto para Salesforce.
1. En **API (Enable OAuth Settings)** [API (Habilitar configuración de OAuth)], seleccione **Enable OAuth Settings** (Habilitar configuración de OAuth).
    1. En **Callback URL** (Dirección URL de devolución de llamada), escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
    1. En **Selected OAuth Scopes** (Ámbitos de OAuth seleccionados), seleccione **Access your basic information (id, profile, email, address, phone)** [Acceder a la información básica (identificador, perfil, correo electrónico, dirección y teléfono)] y **Allow access to your unique identifier (openid)** [Permitir el acceso al identificador único (OpenID)].
    1. Seleccione **Require Secret for Web Server Flow** (Requerir secreto para flujo de servidor web).
1. Seleccione **Configure ID Token** (Configurar token de identificador). 
    1. Establezca el valor **Token Valid for** (Token válido durante) en 5 minutos.
    1. Seleccione **Include Standard Claims** (Incluir notificaciones estándar).
1. Haga clic en **Guardar**.
1. Copie los valores de **Consumer Key** (Clave de consumidor) y **Consumer Secret** (Secreto de consumidor). Necesitará ambos para configurar Salesforce como proveedor de identidades de su inquilino. **Secreto del cliente** es una credencial de seguridad importante.

::: zone pivot="b2c-user-flow"

## <a name="configure-a-salesforce-account-as-an-identity-provider"></a>Configuración de una cuenta de Salesforce como proveedor de identidades

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Nuevo proveedor de OpenID Connect**.
1. Escriba un **nombre**. Por ejemplo, escriba *Salesforce*.
1. En **URL de metadatos**, escriba la dirección URL del [documento de configuración de Salesforce OpenID Connect](https://help.salesforce.com/articleView?id=remoteaccess_using_openid_discovery_endpoint.htm). En el caso de un espacio aislado, login.salesforce.com se reemplaza por test.salesforce.com. En el caso de una comunidad, login.salesforce.com se reemplaza por la dirección URL de la comunidad, como username.force.com/.well-known/openid-configuration. La dirección URL debe ser HTTPS.

    ```
    https://login.salesforce.com/.well-known/openid-configuration
    ```

1. En **Id. de cliente**, escriba el identificador de aplicación que ha anotado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado anteriormente.
1. En **Ámbito**, escriba el valor de `openid id profile email`.
1. Deje los valores predeterminados para **Tipo de respuesta** y **Modo de respuesta**.
1. (Opcional) En **Sugerencia de dominio**, escriba `contoso.com`. Para más información, consulte [Configuración de inicio de sesión directo con Azure Active Directory B2C](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. En **Asignación de notificaciones del proveedor de identidades**, seleccione las siguientes notificaciones:

    - **Id. de usuario**: *sub*
    - **Nombre para mostrar**: *name*
    - **Nombre propio**: *given_name*
    - **Apellido**: *family_name*
    - **Correo electrónico**: *email*

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
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `SalesforceSecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
8. En **Secreto**, escriba el secreto de cliente que haya registrado previamente.
9. En **Uso de claves**, seleccione `Signature`.
10. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si desea que los usuarios inicien sesión con una cuenta de Salesforce, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir una cuenta de Salesforce como un proveedor de notificaciones; para ello, agréguela al elemento **ClaimsProvider** en el archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>salesforce.com</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Salesforce-OIDC">
          <DisplayName>Salesforce</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="METADATA">https://login.salesforce.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid id profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">Your Salesforce application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_SalesforceSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="salesforce.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. **METADATA** se establece en la dirección URL del [documento de configuración de Salesforce OpenID Connect](https://help.salesforce.com/articleView?id=remoteaccess_using_openid_discovery_endpoint.htm). En el caso de un espacio aislado, login.salesforce.com se reemplaza por test.salesforce.com. En el caso de una comunidad, login.salesforce.com se reemplaza por la dirección URL de la comunidad, como username.force.com/.well-known/openid-configuration. La dirección URL debe ser HTTPS.
5. Establezca **client_id** en el identificador de la aplicación desde el registro de aplicación.
6. Guarde el archivo.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con su cuenta de Salesforce. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

El proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro/inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente y luego modificarlo para que también tenga el proveedor de identidades de Salesforce.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón del proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de Salesforce, se muestra un nuevo botón cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `SalesforceExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de Salesforce para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó en **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="Salesforce-OIDC" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `Salesforce-OIDC`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-salesforce-identity-provider-to-a-user-flow"></a>Adición de un proveedor de identidades de Salesforce a un flujo de usuario 

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario que quiera en el proveedor de identidades de Salesforce.
1. En **Proveedores de identidades sociales**, seleccione **Salesforce**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInSalesforce`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_Salesforce`
1. Actualice el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignSalesforce).
1. Guarde los cambios y cargue el archivo.
1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. En **Seleccionar aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y seleccione Salesforce para iniciar sesión con Salesforce y probar la directiva personalizada.

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [pasar el token de Salesforce a la aplicación](idp-pass-through-user-flow.md).
