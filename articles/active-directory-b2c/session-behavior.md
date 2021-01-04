---
title: 'Configuración del comportamiento de la sesión: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar el comportamiento de la sesión en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/14/2020
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: de0f4824cb23a37f37d3834dce67eb0b7edf0b15
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97503248"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Configuración del comportamiento de la sesión en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

El inicio de sesión único (SSO) agrega seguridad y comodidad cuando los usuarios inician sesión en las aplicaciones de Azure Active Directory B2C (Azure AD B2C). Este artículo describe los métodos de inicio de sesión único que se usan en Azure AD B2C y le ayuda a elegir el más apropiado al configurar la directiva.

Con el inicio de sesión único, los usuarios inician sesión una vez con una sola cuenta y obtienen acceso a varias aplicaciones. La aplicación puede ser una aplicación web, móvil o de una sola página, independientemente de la plataforma o del nombre de dominio.

Cuando el usuario inicia sesión por primera vez en una aplicación, Azure AD B2C conserva una sesión basada en cookies. En las solicitudes de autenticación posteriores, Azure AD B2C lee y valida la sesión basada en cookies y emite un token de acceso sin pedir al usuario que vuelva a iniciar sesión. Si la sesión basada en cookies expira o deja de ser válida, se solicita al usuario que vuelva a iniciar sesión.  

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="azure-ad-b2c-session-overview"></a>Información general de la sesión de Azure AD B2C

La integración con Azure AD B2C implica tres tipos de sesiones de SSO:

- **Azure AD B2C**: sesión que administra Azure AD B2C.
- **Proveedor de identidades federado**: sesión que administra el proveedor de identidades; por ejemplo, una cuenta de Microsoft, Facebook o Salesforce.
- **Aplicación**: sesión que administra la aplicación web, móvil o de una sola página.

![Sesión de SSO](media/session-behavior/sso-session-types.png)

### <a name="azure-ad-b2c-session"></a>Sesión de Azure AD B2C 

Cuando un usuario se autentica correctamente con una cuenta local o social, Azure AD B2C almacena una sesión basada en cookies en el explorador del usuario. La cookie se almacena en el nombre de dominio del inquilino de Azure AD B2C, como `https://contoso.b2clogin.com`.

Si un usuario inicia sesión por primera vez con una cuenta federada y, durante el período de la sesión (período de vida o TTL), inicia sesión en la misma aplicación o en otra, Azure AD B2C intenta adquirir un nuevo token de acceso del proveedor de identidades federado. Si la sesión del proveedor de identidades federado expiró o no es válida, el proveedor de identidades federado solicita al usuario sus credenciales. Si la sesión sigue activa (o el usuario ha iniciado sesión con una cuenta local en lugar de una cuenta federada), Azure AD B2C autoriza al usuario y elimina los mensajes posteriores.

Puede configurar el comportamiento de la sesión, incluido el TTL de la sesión y cómo Azure AD B2C comparte la sesión entre las directivas y las aplicaciones.

### <a name="federated-identity-provider-session"></a>Sesión del proveedor de identidades federadas

Un proveedor de identidades social o empresarial administra su propia sesión. La cookie se almacena en el nombre de dominio del inquilino del proveedor de identidades, como `https://login.salesforce.com`. Azure AD B2C no controla la sesión del proveedor de identidades federado. En su lugar, el comportamiento de la sesión viene determinado por el proveedor de identidades federado. 

Considere el caso siguiente:

1. Un usuario inicia sesión en Facebook para comprobar su sección de noticias.
2. Más adelante, abre su aplicación e inicia el proceso de inicio de sesión. La aplicación redirige al usuario a Azure AD B2C para completar el proceso de inicio de sesión.
3. En la página de registro o inicio de sesión de Azure AD B2C, el usuario elige iniciar sesión con su cuenta de Facebook. Se redirige al usuario a Facebook. Si hay una sesión activa en Facebook, no se le pedirá al usuario que proporcione sus credenciales y se redirigirá inmediatamente a Azure AD B2C con un token de Facebook.

### <a name="application-session"></a>Sesión de una aplicación

Una aplicación web, móvil o de una sola página se puede proteger mediante el acceso de OAuth, los tokens de identificador o los tokens de SAML. Cuando un usuario intenta acceder a un recurso protegido en la aplicación, esta comprueba si hay una sesión activa en el lado de la aplicación. Si no hay ninguna sesión de aplicación o la sesión expiró, la aplicación llevará al usuario a la página de inicio de sesión de Azure AD B2C.

La sesión de la aplicación puede ser una sesión basada en cookies almacenada en el nombre de dominio de la aplicación, como `https://contoso.com`. Las aplicaciones móviles pueden almacenar la sesión de forma distinta, aunque con una estrategia similar.

## <a name="configure-azure-ad-b2c-session-behavior"></a>Configuración del comportamiento de la sesión de Azure AD B2C

Puede configurar el comportamiento de la sesión de Azure AD B2C, entre otras cosas:

- **Duración de la sesión de la aplicación web (minutos)** : cantidad de tiempo durante la que la cookie de la sesión de Azure AD B2C está almacenada en el explorador del usuario después de una autenticación correcta. Puede establecer la duración de la sesión en un valor entre 15 y 720 minutos.

- **Tiempo de espera de la sesión de la aplicación web**: indica cómo se extiende una sesión, ya sea mediante el valor de duración de la sesión o mediante la configuración Mantener mi sesión iniciada.
  - **Rolling** (acumulado): indica que la sesión se extiende cada vez que el usuario realiza una autenticación basada en cookies (predeterminado).
  - **Absolute** (absoluto): indica que el usuario está obligado a autenticarse de nuevo tras el período especificado.

- **Configuración de inicio de sesión único**:la sesión de Azure AD B2C se puede configurar con los siguientes ámbitos:
  - **Inquilino**: esta es la configuración predeterminada. Esta configuración permite que varias aplicaciones y flujos de usuario del inquilino B2C compartan la misma sesión de usuario. Por ejemplo, una vez que un usuario inicia sesión en una aplicación, puede también iniciar sesión perfectamente en otra simplemente al acceder a ella.
  - **Aplicación**: este valor permite mantener una sesión de usuario exclusivamente para una aplicación, independientemente de otras aplicaciones. Por ejemplo, puede usar esta opción si quiere que el usuario inicie sesión en Contoso Pharmacy, independientemente de si ya inició sesión en Contoso Groceries.
  - **Directiva**: este valor permite mantener una sesión de usuario exclusivamente para un flujo de usuario, independientemente de las aplicaciones que lo usen. Por ejemplo, si el usuario ya ha iniciado sesión y ha realizado un paso de autenticación multifactor (MFA), se le puede dar acceso a zonas de mayor seguridad en varias aplicaciones mientras no expire la sesión asociada al flujo de usuario.
  - **Deshabilitado**: este valor obliga al usuario a ejecutar todo el flujo de usuario cada vez que se ejecuta la directiva.
::: zone pivot="b2c-custom-policy"
- **Mantener mi sesión iniciada**: amplía la duración de la sesión mediante el uso de una cookie persistente. La sesión permanece activa cuando el usuario cierra y vuelve a abrir el explorador. La sesión se revoca solo cuando un usuario cierra sesión. La característica Mantener mi sesión iniciada solo se aplica al inicio de sesión con cuentas locales. Esta característica tiene prioridad sobre la duración de la sesión. Si está habilitada y el usuario la selecciona, dicha característica indicará cuándo expira la sesión. 
::: zone-end

::: zone pivot="b2c-user-flow"

Para configurar el comportamiento de la sesión:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Abra el flujo de usuario que creó anteriormente.
1. Seleccione **Propiedades**.
1. Configure las opciones **Duración de la sesión de la aplicación web (minutos)** , **Tiempo de espera de la sesión de la aplicación web**, **Configuración de inicio de sesión único** y **Requerir token de identificador en solicitudes de cierre de sesión** según sea necesario.
1. Haga clic en **Save**(Guardar).

::: zone-end

::: zone pivot="b2c-custom-policy"

Para cambiar las configuraciones de comportamiento de sesión y SSO, debe agregar un elemento **UserJourneyBehaviors** dentro del elemento [RelyingParty](relyingparty.md).  El elemento **UserJourneyBehaviors** debe aparecer inmediatamente después de **DefaultUserJourney**. El elemento **UserJourneyBehavors** debería parecerse a este ejemplo:

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="enable-keep-me-signed-in-kmsi"></a>Habilitación de Mantener la sesión iniciada (KMSI)

Puede habilitar la funcionalidad Mantener la sesión iniciada para los usuarios y las aplicaciones nativas que tienen cuentas locales en el directorio de Azure Active Directory B2C (Azure AD B2C). Esta característica concede acceso a los usuarios que vuelven a la aplicación sin pedirles que escriban de nuevo el nombre de usuario y la contraseña. Este acceso se revocará cuando el usuario cierre la sesión.

![Ejemplo de página de inicio de sesión de registro que muestra la casilla Mantener la sesión iniciada](./media/session-behavior/keep-me-signed-in.png)

Los usuarios no deben habilitar esta opción en equipos públicos.

### <a name="configure-the-page-identifier"></a>Configuración del identificador de página

Para habilitar KMSI, establezca el elemento de `DataUri` de la definición de contenido en [identificador de página](contentdefinitions.md#datauri) `unifiedssp` y [versión de la página](page-layout.md) *1.1.0* o superior.

1. Abra el archivo de extensión de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>. Este archivo de extensión es uno de los archivos de directivas incluidos en el paquete de inicio de directivas personalizadas, que debería haber obtenido en el requisito previo, [Introducción a las directivas personalizadas](custom-policy-get-started.md).
1. Busque el elemento **BuildingBlocks**. Si el elemento no existe, agréguelo.
1. Agregue el elemento **ContentDefinitions** al elemento **BuildingBlocks** de la directiva.

    La directiva personalizada debería ser similar al fragmento de código siguiente:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

### <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Incorporación de los metadatos al perfil técnico autoafirmado

Para agregar la casilla KMSI a la página de registro e inicio de sesión, establezca los metadatos de `setting.enableRememberMe` en true. Invalide los perfiles técnicos de SelfAsserted-LocalAccountSignin-Email en el archivo de extensión.

1. Busque el elemento ClaimsProviders. Si el elemento no existe, agréguelo.
1. Agregue el siguiente proveedor de notificaciones al elemento ClaimsProviders:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="setting.enableRememberMe">True</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

1. Guarde el archivo de extensiones.

### <a name="configure-a-relying-party-file"></a>Configuración de un archivo de usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Abra el archivo de la directiva personalizada. Por ejemplo, *SignUpOrSignin.xml*.
1. Si aún no existe, agregue un nodo secundario `<UserJourneyBehaviors>` al nodo `<RelyingParty>`. Tiene que estar situado inmediatamente después de `<DefaultUserJourney ReferenceId="User journey Id" />`, por ejemplo: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Agregue el siguiente nodo como nodo secundario del elemento `<UserJourneyBehaviors>`.

    ```xml
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

Se recomienda que establezca el valor de SessionExpiryInSeconds para un período breve (1200 segundos), mientras que el valor de KeepAliveInDays se puede establecer en un período relativamente largo (30 días), como se muestra en el ejemplo siguiente:

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
    <SessionExpiryType>Absolute</SessionExpiryType>
    <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
  </UserJourneyBehaviors>
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

::: zone-end

## <a name="sign-out"></a>Cierre de sesión

Si desea cerrar la sesión del usuario de la aplicación, no basta con borrar las cookies de la aplicación o finalizar la sesión con el usuario. Debe redireccionar al usuario a Azure AD B2C para cerrar la sesión. De lo contrario, el usuario podría autenticarse de nuevo en la aplicación sin volver a escribir sus credenciales.

Después de una solicitud de cierre de sesión, Azure AD B2C hará lo siguiente:

1. Invalidar la sesión basada en cookies de Azure AD B2C.
::: zone pivot="b2c-user-flow"
2. Tratar de cerrar la sesión de los proveedores de identidades federados.
::: zone-end
::: zone pivot="b2c-custom-policy"
3. Tratar de cerrar la sesión de los proveedores de identidades federados:
   - OpenID Connect: si el punto de conexión de configuración conocido del proveedor de identidades especifica una ubicación `end_session_endpoint`.
   - OAuth2: si los [metadatos del proveedor de identidades](oauth2-technical-profile.md#metadata) contienen la ubicación `end_session_endpoint`.
   - SAML: si los [metadatos del proveedor de identidades](saml-identity-provider-technical-profile.md#metadata) contienen la ubicación `SingleLogoutService`.
4. También puede cerrar la sesión de otras aplicaciones. Para obtener más información, consulte la sección [Cierre de sesión único](#single-sign-out).

> [!NOTE]
> Puede deshabilitar el cierre de sesión de los proveedores de identidades federados. Para ello, debe establecer los metadatos de perfil técnico del proveedor de identidades `SingleLogoutEnabled` en `false`.
::: zone-end

El cierre de sesión borrará el estado de inicio de sesión único del usuario con Azure AD B2C, pero podría no cerrar la sesión del proveedor de identidades social del usuario. Si el usuario selecciona el mismo proveedor de identidades durante un inicio de sesión posterior, podría volver a autenticarse sin especificar sus credenciales. Si un usuario quiere cerrar sesión en su aplicación, eso no significa necesariamente que quiera cerrar sesión en su cuenta de Facebook. Sin embargo, si se usan cuentas locales, la sesión del usuario finalizará correctamente.

::: zone pivot="b2c-custom-policy"

### <a name="single-sign-out"></a>Cierre de sesión único 

Cuando redirige al usuario al punto de conexión de cierre de sesión de Azure AD B2C (para los protocolos OAuth2 y SAML), Azure AD B2C borra la sesión del usuario del explorador. Sin embargo, el usuario podría permanecer conectado a otras aplicaciones que usan Azure AD B2C para la autenticación. Para permitir que esas aplicaciones cierren la sesión del usuario de manera simultánea, Azure AD B2C envía una solicitud HTTP GET al elemento `LogoutUrl` registrado de todas las aplicaciones en las que tiene una sesión iniciada.

Las aplicaciones deben responder a esta solicitud mediante la eliminación de la sesión que identifica al usuario y la devolución de una respuesta `200`. Si quiere admitir el inicio de sesión único en la aplicación, debe implementar un elemento `LogoutUrl` en el código de la aplicación. 

Para admitir el cierre de sesión único, los perfiles técnicos del emisor de tokens para JWT y SAML deben especificar lo siguiente:

- El nombre del protocolo, como `<Protocol Name="OpenIdConnect" />`.
- La referencia al perfil técnico de la sesión, como `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />`.

En el ejemplo siguiente se muestran emisores de tokens de JWT y SAML con el cierre de sesión único:

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

::: zone-end

### <a name="secure-your-logout-redirect"></a>Protección de la redirección de cierre de sesión

Después del cierre de sesión, se redirige al usuario al URI especificado en el parámetro `post_logout_redirect_uri`, independientemente de las direcciones URL de respuesta que se hayan especificado para la aplicación. Sin embargo, si se pasa un valor de `id_token_hint` válido y la opción **Requerir token de identificador en solicitudes de cierre de sesión** está activada, Azure AD B2C comprueba que el valor de `post_logout_redirect_uri` coincida con uno de los URI de redirección configurados de la aplicación antes de realizar la redirección. Si no se configuró ninguna dirección URL de respuesta coincidente para la aplicación, se muestra un mensaje de error y no se redirige al usuario. 

::: zone pivot="b2c-user-flow"

Para requerir un token de identificador en las solicitudes de cierre de sesión:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Abra el flujo de usuario que creó anteriormente.
1. Seleccione **Propiedades**.
1. Habilite **Requerir token de identificador en solicitudes de cierre de sesión**.
1. Vuelva a **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y, luego, seleccione su aplicación.
1. Seleccione **Autenticación**.
1. En el cuadro de texto **URL de cierre de sesión**, escriba el URI de redirección posterior al cierre de sesión y, luego, seleccione **Guardar**.

::: zone-end

::: zone pivot="b2c-custom-policy"

Para requerir un token de identificador en las solicitudes de cierre de sesión, agregue un elemento **UserJourneyBehaviors** dentro del elemento [RelyingParty](relyingparty.md). Luego, establezca **EnforceIdTokenHintOnLogout** del elemento **SingleSignOn** en `true`. El elemento **UserJourneyBehavors** debe ser como este:

```xml
<UserJourneyBehaviors>
  <SingleSignOn Scope="Tenant" EnforceIdTokenHintOnLogout="true"/>
</UserJourneyBehaviors>
```

::: zone-end

Para configurar la dirección URL de cierre de sesión de la aplicación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y, luego, seleccione su aplicación.
1. Seleccione **Autenticación**.
1. En el cuadro de texto **URL de cierre de sesión**, escriba el URI de redirección posterior al cierre de sesión y, luego, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [configuración de tokens en Azure AD B2C](configure-tokens.md).
