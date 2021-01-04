---
title: TechnicalProfiles
titleSuffix: Azure AD B2C
description: Especifique el elemento TechnicalProfiles de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 99ed7d3ad81202ab6fe67bf52888bbdbf0b28d2a
ms.sourcegitcommit: ea17e3a6219f0f01330cf7610e54f033a394b459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97387095"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Un perfil técnico proporciona un marco con un mecanismo integrado para comunicarse con distintos tipos de entidades utilizando una directiva personalizada en Azure Active Directory B2C (Azure AD B2C). Los perfiles técnicos se usan para comunicarse con su inquilino de Azure AD B2C, crear un usuario o leer un perfil de usuario. Un perfil técnico puede ser autoafirmado para habilitar la interacción con el usuario. Por ejemplo, recopilar las credenciales del usuario para iniciar sesión y, a continuación, representar la página de registro o la página de restablecimiento de contraseña.

## <a name="type-of-technical-profiles"></a>Tipo de perfiles técnicos

Un perfil técnico posibilita estos tipos de escenarios:

- [Application Insights](application-insights-technical-profile.md): envía datos de eventos a [Application Insights](../azure-monitor/app/app-insights-overview.md).
- [Azure Active Directory](active-directory-technical-profile.md): proporciona compatibilidad para la administración de usuarios de Azure Active Directory B2C.
- [Multi-Factor Authentication de Azure AD](multi-factor-auth-technical-profile.md): proporciona compatibilidad para verificar un número de teléfono mediante Multi-Factor Authentication (MFA) de Azure AD. 
- [Transformación de notificaciones](claims-transformation-technical-profile.md): llamada a transformaciones de notificaciones de salida para manipular los valores de notificaciones, validar las notificaciones o establecer valores predeterminados para un conjunto de notificaciones de salida.
- [Sugerencia de token de identificador](id-token-hint.md): valida la firma, el nombre del emisor y la audiencia del token JWT `id_token_hint` y extrae la notificación del token de entrada.
- [Emisor de tokens de JWT](jwt-issuer-technical-profile.md): emite un token de JWT que se devuelve a la aplicación de usuario de confianza.
- [OAuth1](oauth1-technical-profile.md) : federación con cualquier proveedor de identidades del protocolo OAuth 1.0.
- [OAuth2](oauth2-technical-profile.md) : federación con cualquier proveedor de identidades del protocolo OAuth 2.0.
- [Contraseña de un solo uso](one-time-password-technical-profile.md): proporciona compatibilidad para administrar la generación y verificación de una contraseña de un solo uso.
- [OpenID Connect](openid-connect-technical-profile.md): federación con cualquier proveedor de identidades del protocolo OpenID Connect.
- [Factor de teléfono](phone-factor-technical-profile.md): es compatible con la inscripción y comprobación de números de teléfono.
- [Proveedor de RESTful](restful-technical-profile.md): llamada a los servicios de la API de REST, como validar entrada de usuario, enriquecer los datos de usuario o integrar con aplicaciones de línea de negocio.
- [Proveedor de identidades SAML](saml-identity-provider-technical-profile.md) : federación con cualquier proveedor de identidades del protocolo SAML.
- [Emisor de tokens de SAML](saml-issuer-technical-profile.md): emite un token de SAML que se devuelve a la aplicación de usuario de confianza.
- [Autoaserción](self-asserted-technical-profile.md): interacción con el usuario. Por ejemplo, recopilar las credenciales del usuario para iniciar sesión, representar la página de registro o restablecer la contraseña.
- [Administración de sesiones](custom-policy-reference-sso.md): administración de distintos tipos de sesiones.

## <a name="technical-profile-flow"></a>Flujo del perfil técnico

Todos los tipos de perfiles técnicos comparten el mismo concepto. El usuario envía notificaciones de entrada, ejecuta la transformación de notificaciones y se comunica con la entidad configurada, como un proveedor de identidades, la API REST o los servicios de directorio de Azure AD. Una vez finalizado el proceso, el perfil técnico devuelve las notificaciones de salida y puede ejecutar la transformación de notificaciones de salida. El diagrama siguiente muestra cómo se procesan las transformaciones y las asignaciones a las que se hace referencia en el perfil técnico. Independientemente de la entidad con la que interactúe el perfil técnico, una vez que se ejecute la transformación de las notificaciones, las notificaciones de salida del perfil técnico se almacenan de inmediato en el contenedor de notificaciones.

![Diagrama que ilustra el flujo de perfil técnico](./media/technical-profiles/technical-profile-flow.png)

1. **Administración de la sesión de inicio de sesión único (SSO)**: restaura el estado de la sesión del perfil técnico mediante la [administración de la sesión de inicio de sesión único](custom-policy-reference-sso.md).
1. **Transformación de notificaciones de entrada**: antes de iniciarse el perfil técnico, Azure AD B2C ejecuta la entrada [transformación de notificaciones].(claimstransformations.md).
1. **Notificaciones de entrada**: las notificaciones se toman del contenedor de notificaciones y se usan para el perfil técnico.
1. **Ejecución de perfil técnico**: el perfil técnico intercambia las notificaciones con la entidad configurada. Por ejemplo:
    - Redirija al usuario al proveedor de identidades para completar el inicio de sesión. Después de iniciar sesión correctamente, el usuario regresa y la ejecución del perfil técnico continúa.
    - Llame a una API REST mientras envía parámetros como InputClaims y recupera información como OutputClaims.
    - Cree o actualice la cuenta de usuario.
    - Envía y comprueba el mensaje de texto de MFA.
1. **Perfiles técnicos de validación**: un [perfil técnico autoafirmado](self-asserted-technical-profile.md) puede llamar a [perfiles técnicos de validación](validation-technical-profile.md) para validar los datos generados por el usuario.
1. **Notificaciones de salida**: las notificaciones se devuelven al contenedor de notificaciones. Puede usar dichas notificaciones en el siguiente paso de orquestación o en las transformaciones de notificaciones de salida.
1. **Transformaciones de notificaciones de salida**: una vez completado el perfil técnico, Azure AD B2C ejecuta la [transformación de notificaciones](claimstransformations.md) de salida. 
1. **Administración de la sesión de inicio de sesión único (SSO)**: conserva los datos del perfil técnico en la sesión mediante la [administración de la sesión de inicio de sesión único](custom-policy-reference-sso.md).

Un elemento **TechnicalProfiles** contiene un conjunto de perfiles técnicos admitidos por el proveedor de notificaciones. Cada proveedor de notificaciones necesita tener uno o más perfiles técnicos que determinen los puntos de conexión y protocolos necesarios para establecer comunicación con el proveedor de notificaciones. Un proveedor de notificaciones puede tener varios perfiles técnicos.

```xml
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        ...
      </Metadata>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

El elemento **TechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
|---------|---------|---------|
| Identificador | Sí | Un identificador único del perfil técnico. Se puede hacer referencia al perfil técnico con este identificador desde otros elementos del archivo de directiva. Por ejemplo, **OrchestrationSteps** y **ValidationTechnicalProfile**. |

El elemento **TechnicalProfile** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| Domain | 0:1 | Nombre de dominio del perfil técnico. Por ejemplo, si el perfil técnico especifica el proveedor de identidades de Facebook, el nombre de dominio será Facebook.com. |
| DisplayName | 1:1 | Nombre para mostrar del perfil técnico. |
| Descripción | 0:1 | Descripción del perfil técnico. |
| Protocolo | 1:1 | Protocolo usado para la comunicación con la otra parte. |
| Metadatos | 0:1 | Colección de pares de clave y valor usada por el protocolo para comunicarse con el punto de conexión durante una transacción. |
| InputTokenFormat | 0:1 | Formato del token de entrada. Valores posibles: `JSON`, `JWT`, `SAML11` o `SAML2`. El valor `JWT` representa un token de JSON Web Token según la especificación IETF. El valor `SAML11` representa un token de seguridad SAML 1.1 según la especificación OASIS.  El valor `SAML2` representa un token de seguridad SAML 2.0 según la especificación OASIS. |
| OutputTokenFormat | 0:1 | Formato del token de salida. Valores posibles: `JSON`, `JWT`, `SAML11` o `SAML2`. |
| CryptographicKeys | 0:1 | Lista de claves criptográficas que se usan en el perfil técnico. |
| InputClaimsTransformations | 0:1 | Lista de referencias (definidas anteriormente) a transformaciones de notificaciones que tienen que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. |
| InputClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que se usan como entrada en el perfil técnico. |
| PersistedClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que el proveedor de notificaciones hace persistir y que están relacionadas con el perfil técnico. |
| DisplayClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que el proveedor de notificaciones presenta y que están relacionadas con el [perfil técnico autoafirmado](self-asserted-technical-profile.md). La característica DisplayClaims se encuentra actualmente en **versión preliminar**. |
| OutputClaims | 0:1 | Lista de referencias (definidas anteriormente) a tipos de notificación que se usan como salida en el perfil técnico. |
| OutputClaimsTransformations | 0:1 | Lista de referencias (definidas anteriormente) a transformaciones de notificaciones que tienen que ejecutarse cuando el proveedor de notificaciones reciba las notificaciones. |
| ValidationTechnicalProfiles | 0:n | Lista de referencias a otros perfiles técnicos que el perfil técnico usa con fines de validación. Para obtener más información, vea [Validación del perfil técnico](validation-technical-profile.md).|
| SubjectNamingInfo | 0:1 | Controla la producción del nombre de firmante en los tokens donde el nombre de firmante se especifica por separado de las notificaciones. Por ejemplo, OAuth o SAML.  |
| IncludeInSso | 0:1 |  Si el uso de este perfil técnico debe aplicar el comportamiento de inicio de sesión único (SSO) para la sesión o, en su lugar, requerir una interacción explícita. Este elemento solo es válido en los perfiles de SelfAsserted usados dentro de un perfil técnico de validación. Valores posibles: `true` (opción predeterminada) o `false`. |
| IncludeClaimsFromTechnicalProfile | 0:1 | Identificador de un perfil técnico cuyas notificaciones de entrada o salida quiere que se agreguen a este perfil técnico. El perfil técnico al que se haga referencia tiene que definirse en el mismo archivo de directiva. |
| IncludeTechnicalProfile |0:1 | Identificador de un perfil técnico cuyos datos quiere agregar a este perfil técnico. |
| UseTechnicalProfileForSessionManagement | 0:1 | Perfil técnico distinto que se usará para la administración de sesiones. |
|EnabledForUserJourneys| 0:1 |Controla si el perfil técnico se ejecuta en un recorrido del usuario.  |

## <a name="protocol"></a>Protocolo

**Protocol** especifica el protocolo que se va a usar para la comunicación con la otra parte. El elemento **Protocol** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Nombre | Sí | El nombre de un protocolo válido admitido por Azure AD B2C que se usará como parte del perfil técnico. Valores posibles: `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `Proprietary` o `None`. |
| Controlador | No | Cuando el nombre del protocolo se establece en `Proprietary`, especifique el nombre completo del ensamblado que usará Azure AD B2C para determinar el controlador de protocolo. |

## <a name="metadata"></a>Metadatos

El elemento **Metadata** contiene las opciones de configuración pertinentes de un protocolo específico. La lista de metadatos admitidos se documenta en la especificación del [perfil técnico](#type-of-technical-profiles) correspondiente. Un elemento **Metadata** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| Elemento | 0:n | Los metadatos relacionados con el perfil técnico. Cada tipo de perfil técnico tiene un conjunto distinto de elementos de metadatos. Para obtener más información, vea la sección sobre los tipos de perfil técnico. |

### <a name="item"></a>Elemento

El elemento **Item** del elemento **Metadata** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Clave | Sí | La clave de metadatos. Puede ver la lista de elementos de metadatos en cada [tipo de perfil técnico](#type-of-technical-profiles). |

En el ejemplo siguiente se muestra el uso de metadatos pertinentes para el [perfil técnico de OAuth2](oauth2-technical-profile.md#metadata).

```xml
<TechnicalProfile Id="Facebook-OAUTH">
  ...
  <Metadata>
    <Item Key="ProviderName">facebook</Item>
    <Item Key="authorization_endpoint">https://www.facebook.com/dialog/oauth</Item>
    <Item Key="AccessTokenEndpoint">https://graph.facebook.com/oauth/access_token</Item>
    <Item Key="HttpBinding">GET</Item>
    <Item Key="UsePolicyInRedirectUri">0</Item>
    ...
  </Metadata>
  ...
</TechnicalProfile>
```

En el ejemplo siguiente se muestra el uso de metadatos pertinentes para el [perfil técnico de la API de REST](restful-technical-profile.md#metadata).

```xml
<TechnicalProfile Id="REST-Validate-Email">
  ...
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    ...
  </Metadata>
  ...
</TechnicalProfile>
```

## <a name="cryptographic-keys"></a>Claves de cifrado

Azure AD B2C almacena secretos y certificados en forma de [claves de directiva](policy-keys-overview.md) para establecer la confianza con los servicios con los que se integra. Durante la ejecución del perfil técnico, Azure AD B2C recupera las claves criptográficas de las claves de directiva de Azure AD B2C las claves de directiva y, a continuación, usa las claves para establecer la confianza, cifrar o firmar un token. Estas confianzas constan de los siguientes componentes:

- Federación con los proveedores de identidades [OAuth1](oauth1-technical-profile.md#cryptographic-keys), [OAuth2](oauth2-technical-profile.md#cryptographic-keys) y [SAML](saml-identity-provider-technical-profile.md#cryptographic-keys)
- Protección de la conexión con los [servicios de la API de REST](secure-rest-api.md)
- Firma y cifrado de los tokens [JWT](jwt-issuer-technical-profile.md#cryptographic-keys) y [SAML](saml-issuer-technical-profile.md#cryptographic-keys)

El elemento **CryptographicKeys** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| Clave | 1:n | Una clave criptográfica usada en este perfil técnico. |

### <a name="key"></a>Clave

El elemento **Key** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Identificador | No | Un identificador único de un par de claves específico al que se hace referencia desde otros elementos en el archivo de directiva. |
| StorageReferenceId | Sí | Un identificador del contenedor de claves de almacenamiento al que se hace referencia desde otros elementos en el archivo de directiva. |

## <a name="input-claims-transformations"></a>Transformaciones de notificaciones de entrada

El elemento **InputClaimsTransformations** puede contener una colección de elementos de transformación de notificaciones de entrada que se usan para modificar las notificaciones de entrada o generar otras nuevas. 

Las notificaciones de salida de una transformación de notificaciones anterior en la colección de transformaciones de notificaciones pueden ser notificaciones de entrada de una transformación de notificaciones de entrada subsiguiente, lo que le permite tener una secuencia de transformación de notificaciones dependiendo unas de otras.

El elemento **InputClaimsTransformations** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| InputClaimsTransformation | 1:n | El identificador de una transformación de notificaciones que tiene que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. Una transformación de notificaciones puede usarse para modificar notificaciones ClaimsSchema existentes o para generar nuevas. |

### <a name="inputclaimstransformation"></a>InputClaimsTransformation

El elemento **InputClaimsTransformation** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ReferenceId | Sí | Un identificador de una transformación de notificaciones que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

Los siguientes perfiles técnicos hacen referencia a la transformación de notificaciones **CreateOtherMailsFromEmail**. La transformación de notificaciones agrega el valor de la notificación `email` a la colección `otherMails`, antes de conservar los datos en el directorio.

```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  ...
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  ...
</TechnicalProfile>
```

## <a name="input-claims"></a>Notificaciones de entrada

**InputClaims** toma notificaciones del contenedor de notificaciones y se usan para el perfil técnico. Por ejemplo, un [perfil técnico autoafirmado](self-asserted-technical-profile.md) usa las notificaciones de entrada para rellenar previamente las notificaciones de salida que proporciona el usuario. Un perfil técnico de la API REST usa las notificaciones de entrada para enviar los parámetros de entrada al punto de conexión de la API REST. Azure Active Directory usa la notificación de entrada como un identificador único para leer, actualizar o eliminar una cuenta.

El elemento **InputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| InputClaim | 1:n | Un tipo de notificación de entrada esperado. |

### <a name="inputclaim"></a>InputClaim

El elemento **InputClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Sí | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | No | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
| PartnerClaimType | No | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

## <a name="display-claims"></a>Notificaciones de visualización

El elemento **DisplayClaims** contiene una lista de notificaciones definidas por el [perfil técnico autoafirmado](self-asserted-technical-profile.md) que se va a presentar en la pantalla para recopilar datos del usuario. En la colección de notificaciones de visualización, puede incluir una referencia a un [tipo de notificación](claimsschema.md) o a un elemento [DisplayControl](display-controls.md) que haya creado. 

- Un tipo de notificación es una referencia a una notificación que se va a mostrar en la pantalla. 
  - Para forzar que el usuario proporcione un valor para una notificación específica, establezca el atributo **Required** del elemento **DisplayClaim** en `true`.
  - Para rellenar previamente los valores de las notificaciones de visualización, use las notificaciones de entrada descritas anteriormente. El elemento también puede incluir un valor predeterminado.
  - El elemento **ClaimType** de la colección **DisplayClaims** necesita establecer el elemento **UserInputType** en cualquier tipo de entrada de usuario admitido por Azure AD B2C. Por ejemplo, `TextBox` o `DropdownSingleSelect`.
- Un control de visualización es un elemento de la interfaz de usuario que tiene una funcionalidad especial e interactúa con el servicio de back-end de Azure AD B2C. Permite al usuario realizar acciones en la página que invocan un perfil técnico de validación en el back-end. Por ejemplo, la comprobación de una dirección de correo electrónico, un número de teléfono o un número de fidelidad del cliente.

El orden de los elementos en **DisplayClaims** especifica el orden en que Azure AD B2C presenta las notificaciones en la pantalla. 

El elemento **DisplayClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| DisplayClaim | 1:n | Un tipo de notificación de entrada esperado. |

### <a name="displayclaim"></a>DisplayClaim

El elemento **DisplayClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | No | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DisplayControlReferenceId | No | Identificador de un [control de visualización](display-controls.md) ya definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| Obligatorio | No | Indica si la notificación de visualización es obligatoria. |

En el ejemplo siguiente se muestra el uso de notificaciones de visualización y controles de visualización con un perfil técnico autoafirmado.

![Un perfil técnico autoafirmado con notificaciones de visualización](./media/technical-profiles/display-claims.png)

En el siguiente perfil técnico:

- La primera notificación de visualización hace referencia al control de visualización `emailVerificationControl`, que recopila y comprueba la dirección de correo.
- La quinta notificación de visualización hace referencia al control de visualización `phoneVerificationControl`, que recopila y comprueba un número de teléfono.
- Las demás notificaciones de visualización son elementos ClaimTypes que se van a recopilar del usuario.

```xml
<TechnicalProfile Id="Id">
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
    <DisplayClaim DisplayControlReferenceId="phoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
  </DisplayClaims>
</TechnicalProfile>
```

### <a name="persisted-claims"></a>Notificaciones persistentes

El elemento **PersistedClaims** contiene todos los valores que el [perfil técnico de Azure AD](active-directory-technical-profile.md) debe conservar con la información de asignaciones posibles entre un tipo de notificación ya definido en la sección [ClaimsSchema](claimsschema.md) de la directiva y el nombre de atributo de Azure AD.

El nombre de la notificación es el del [atributo de Azure AD](user-profile-attributes.md), a menos que se especifique el atributo **PartnerClaimType**, que contiene el nombre de atributo de Azure AD.

El elemento **PersistedClaims** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| PersistedClaim | 1:n | El tipo de notificación que persistirá. |

### <a name="persistedclaim"></a>PersistedClaim

El elemento **PersistedClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Sí | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | No | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
| PartnerClaimType | No | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

En el siguiente ejemplo, el perfil técnico **AAD-UserWriteUsingLogonEmail** o el [paquete de inicio](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccounts) crea una cuenta local y conserva las notificaciones siguientes:

```xml
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
  <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
  <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
  <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
  <PersistedClaim ClaimTypeReferenceId="givenName" />
  <PersistedClaim ClaimTypeReferenceId="surname" />
</PersistedClaims>
```

## <a name="output-claims"></a>Notificaciones de salida

**OutputClaims** son la colección de notificaciones que se devuelven al contenedor de notificaciones una vez completado el perfil técnico. Puede usar dichas notificaciones en el siguiente paso de orquestación o en las transformaciones de notificaciones de salida. El elemento **OutputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| OutputClaim | 1:n | Un tipo de notificación de salida esperado. |

### <a name="outputclaim"></a>OutputClaim

El elemento **OutputClaim** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Sí | El identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva o del archivo de directiva principal. |
| DefaultValue | No | Un valor predeterminado que se usará para crear una notificación si la notificación indicada por ClaimTypeReferenceId no existe, por lo que la notificación resultante puede usarse como un elemento InputClaim por el perfil técnico. |
|AlwaysUseDefaultValue |No |Obliga a usar el valor predeterminado.  |
| PartnerClaimType | No | El identificador del tipo de notificación del socio externo al que se asigna el tipo de notificación de directiva especificado. Si no se especifica el atributo PartnerClaimType, el tipo de notificación de directiva especificado se asignará al tipo de notificación del socio que tenga el mismo nombre. Use esta propiedad cuando el nombre del tipo de notificación sea distinto de la otra entidad. Por ejemplo, el nombre de la primera notificación es “givenName”, mientras que el socio usa una notificación denominada “first_name”. |

## <a name="output-claims-transformations"></a>Transformaciones de notificaciones de salida

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas. Después de la ejecución, las notificaciones de salida se vuelven a poner en el contenedor de notificaciones. Puede usar esas notificaciones en el paso de orquestaciones siguiente.

Las notificaciones de salida de una transformación de notificaciones anterior en la colección de transformaciones de notificaciones pueden ser notificaciones de entrada de una transformación de notificaciones de entrada subsiguiente, lo que le permite tener una secuencia de transformación de notificaciones dependiendo unas de otras.

El elemento **OutputClaimsTransformations** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| OutputClaimsTransformation | 1:n | Los identificadores de transformaciones de notificaciones que tienen que ejecutarse antes de enviar notificaciones al proveedor de notificaciones o al usuario de confianza. Una transformación de notificaciones puede usarse para modificar notificaciones ClaimsSchema existentes o para generar nuevas. |

### <a name="outputclaimstransformation"></a>OutputClaimsTransformation

El elemento **OutputClaimsTransformation** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ReferenceId | Sí | Un identificador de una transformación de notificaciones que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

El siguiente perfil técnico hace referencia a la transformación de notificaciones AssertAccountEnabledIsTrue para evaluar si la cuenta se habilita o no después de leer la notificación `accountEnabled` del directorio.    

```xml
<TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
  ...
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="accountEnabled" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
  </OutputClaims>
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
  ...
</TechnicalProfile>
```

## <a name="validation-technical-profiles"></a>Perfiles técnicos de validación

Los perfiles técnicos de validación se usan para validar algunas o todas las notificaciones de salida de las referencias en un [perfil técnico autoafirmado](self-asserted-technical-profile.md#validation-technical-profiles). Un perfil técnico de validación es un perfil técnico normal de cualquier protocolo, como [Azure Active Directory](active-directory-technical-profile.md) o una [API de REST](restful-technical-profile.md). El perfil técnico de validación devuelve notificaciones de salida o devuelve el código de error. El mensaje de error se representa para el usuario en la pantalla, lo que le permite volver a intentarlo.

En el siguiente diagrama se muestra cómo Azure AD B2C usa un perfil técnico de validación para validar las credenciales de usuario

![Flujo del perfil técnico de validación de diagramas](./media/technical-profiles/validation-technical-profile.png) 

El elemento **ValidationTechnicalProfiles** contiene el elemento siguiente:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1:n | Los identificadores de perfiles técnicos que se usan para validar algunas o todas las notificaciones de salida del perfil técnico al que se hace referencia. Todas las notificaciones de entrada del perfil técnico al que se hace referencia tienen que aparecer en las notificaciones de salida del perfil técnico al que se hace referencia. |

### <a name="validationtechnicalprofile"></a>ValidationTechnicalProfile

El elemento **ValidationTechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ReferenceId | Sí | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

## <a name="subjectnaminginfo"></a>SubjectNamingInfo

**SubjectNamingInfo** define el nombre del firmante que se usa en los tokens de una [directiva de usuario de confianza](relyingparty.md#subjectnaminginfo). El elemento **SubjectNamingInfo** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClaimType | Sí | Un identificador de un tipo de notificación que ya se ha definido en la sección ClaimsSchema del archivo de directiva. |

## <a name="include-technical-profile"></a>Inclusión de un perfil técnico

Un perfil técnico puede incluir otro perfil técnico para cambiar la configuración o agregar funcionalidades. El elemento **IncludeTechnicalProfile** es una referencia al perfil técnico común del que se deriva un perfil técnico. Para reducir la redundancia y complejidad de los elementos de la directiva, use la inclusión cuando tenga varios perfiles técnicos que compartan los elementos básicos. Use un perfil técnico común con el conjunto común de configuración, junto con perfiles técnicos de tareas específicos que incluyen el perfil técnico común. Por ejemplo, supongamos que tiene un [perfil técnico de la API de REST](restful-technical-profile.md) con un solo punto de conexión en el que debe enviar diferentes conjuntos de notificaciones para diversos escenarios. Cree un perfil técnico común con la funcionalidad compartida, como el URI de punto de conexión de la API de REST, los metadatos, el tipo de autenticación y las claves criptográficas. A continuación, cree perfiles técnicos de tareas específicos que incluyan el perfil técnico común, agregue las notificaciones de entrada, las notificaciones de salida o sobrescriba el URI de punto de conexión de la API de REST pertinente para ese perfil técnico.

El elemento **IncludeTechnicalProfile** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ReferenceId | Sí | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |


En el siguiente ejemplo se muestra el uso de la instrucción:

- *REST-API-Common*: un perfil técnico común con la configuración básica.
- *REST-ValidateProfile*: incluye el perfil técnico *REST-API-Common* y especifica las notificaciones de entrada y salida.
- *REST-UpdateProfile*: incluye el perfil técnico *REST-API-Common*, especifica las notificaciones de entrada y sobrescribe los metadatos `ServiceUrl`.

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-API-Commom">
      <DisplayName>Base REST API configuration</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity</Item>
        <Item Key="AuthenticationType">Basic</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
      </CryptographicKeys>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

    <TechnicalProfile Id="REST-ValidateProfile">
      <DisplayName>Validate the account and return promo code</DisplayName>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" />
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="userLanguage" PartnerClaimType="lang" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="promoCode" />
      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="REST-API-Commom" />
    </TechnicalProfile>

    <TechnicalProfile Id="REST-UpdateProfile">
       <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/update</Item>
      </Metadata>
      <DisplayName>Update the user profile</DisplayName>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" />
        <InputClaim ClaimTypeReferenceId="email" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="REST-API-Commom" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

### <a name="multi-level-inclusion"></a>Inclusión de varios niveles 

Un perfil técnico puede incluir un solo perfil técnico. No existe un límite en el número de niveles de inclusión. Por ejemplo, el perfil técnico **AAD-UserReadUsingAlternativeSecurityId-NoError** incluye **AAD-UserReadUsingAlternativeSecurityId**. Este perfil técnico establece el elemento de metadatos `RaiseErrorIfClaimsPrincipalDoesNotExist` en `true` y genera un error si no existe una cuenta de redes sociales en el directorio. **AAD-UserReadUsingAlternativeSecurityId-NoError** invalida este comportamiento y deshabilita ese mensaje de error.

```xml
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
```

**AAD UserReadUsingAlternativeSecurityId** incluye el perfil técnico de `AAD-Common`.

```xml
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

Ni **AAD-UserReadUsingAlternativeSecurityId-NoError** ni **AAD-UserReadUsingAlternativeSecurityId** especifican el elemento **Protocol**, porque se especifica en el perfil técnico **AAD-Common**.

```xml
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```

## <a name="use-technical-profile-for-session-management"></a>Uso de un perfil técnico para la administración de sesiones

La referencia de elementos **UseTechnicalProfileForSessionManagement** a un [perfil técnico de sesión de inicio de sesión único](custom-policy-reference-sso.md). El elemento **UseTechnicalProfileForSessionManagement** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ReferenceId | Sí | Un identificador de un perfil técnico que ya se ha definido en el archivo de directiva o en el archivo de directiva principal. |

## <a name="enabled-for-user-journeys"></a>Habilitado para recorridos de usuario

El elemento [ClaimsProviderSelections](userjourneys.md#claimsproviderselection) en un recorrido del usuario define la lista de opciones de selección de proveedor de notificaciones y el orden en que se mostrarán. Con el elemento **EnabledForUserJourneys**, puede filtrar los proveedores de notificaciones que estarán disponibles para el usuario. El elemento **EnabledForUserJourneys** contiene uno de los valores siguientes:

- **Always**: ejecuta el perfil técnico.
- **Never**: omite el perfil técnico.
- **OnClaimsExistence**: solo se ejecuta cuando existe una notificación específica (indicada en el perfil técnico).
- **OnItemExistenceInStringCollectionClaim**: solo se ejecuta cuando existe un elemento en una notificación de colección de cadenas.
- **OnItemAbsenceInStringCollectionClaim**: solo se ejecuta cuando no existe un elemento en una notificación de colección de cadenas.

Para usar **OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim** o **OnItemAbsenceInStringCollectionClaim** es necesario que proporcione los siguientes metadatos: **ClaimTypeOnWhichToEnable** especifica el tipo de notificación que se va a evaluar, mientras **ClaimValueOnWhichToEnable** especifica el valor que se va a comparar.

El siguiente perfil técnico solo se ejecuta si la colección de cadenas **identityProviders** contiene el valor de `facebook.com`:

```xml
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
      <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
      <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>
```
