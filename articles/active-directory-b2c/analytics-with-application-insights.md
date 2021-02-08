---
title: Seguimiento del comportamiento del usuario con Application Insights
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo habilitar registros de eventos en Application Insights desde recorridos del usuario de Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 01/29/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ce80e3376482ef44b466757cf7e345c4bcf186ad
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99218560"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Seguimiento del comportamiento del usuario en Azure Active Directory B2C mediante Application Insights

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"

Azure Active Directory B2C (Azure AD B2C) envía los datos de eventos directamente a [Application Insights](../azure-monitor/app/app-insights-overview.md) mediante la clave de instrumentación proporcionada a Azure AD B2C. Con un perfil técnico de Application Insights, puede obtener registros de eventos detallados y personalizados para los recorridos del usuario para:

* Obtener información detallada sobre el comportamiento del usuario.
* Solucionar problemas con sus propias directivas durante la fase de desarrollo o producción.
* Medir el rendimiento.
* Crear notificaciones de Application Insights.

## <a name="overview"></a>Información general

Para habilitar los registros de eventos personalizados, agregue un perfil técnico de Application Insights. En el perfil técnico, se define la clave de instrumentación de Application Insights, el nombre del evento y las notificaciones que se van a registrar. Para publicar un evento, el perfil técnico se agrega entonces como un paso de orquestación en un [recorrido del usuario](userjourneys.md).

Al usar Application Insights, tenga en cuenta lo siguiente:

- Hay un breve retraso, normalmente inferior a cinco minutos, antes de que los nuevos registros estén disponibles en Application Insights.
- Azure AD B2C le permite elegir las notificaciones que se deben registrar. No incluya notificaciones con datos personales.
- Para registrar una sesión de usuario, los eventos se pueden unificar mediante un id. de correlación. 
- Llame al perfil técnico de Application Insights directamente desde un [recorrido](userjourneys.md) o un [subrecorrido del usuario](subjourneys.md). No use un perfil técnico de Application Insights como [perfil técnico de validación](validation-technical-profile.md).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites-custom-policy](../../includes/active-directory-b2c-customization-prerequisites-custom-policy.md)]

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights

Cuando usa Application Insights con Azure AD B2C, todo lo que necesita hacer es crear un recurso y obtener la clave de instrumentación. Para obtener más información, vea [Creación de recursos en Application Insights](../azure-monitor/app/create-new-resource.md)

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene la suscripción de Azure. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y elija el directorio que contiene la suscripción. Este inquilino no es el inquilino de Azure AD B2C.
3. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal y, a continuación, busque y seleccione **Application Insights**.
4. Haga clic en **Crear**.
5. En **Nombre**, escriba un nombre para el recurso.
6. En **Tipo de aplicación**, seleccione **Aplicación web ASP.NET**.
7. En **Grupo de recursos**, seleccione un grupo existente o escriba un nombre para un nuevo grupo.
8. Haga clic en **Crear**.
4. Después de crear el recurso de Application Insights, ábralo, expanda **Información esencial** y copie la clave de instrumentación.

![Introducción a Application Insights y la clave de instrumentación](./media/analytics-with-application-insights/app-insights.png)

## <a name="define-claims"></a>Definición de notificaciones

Una notificación proporciona un almacenamiento temporal de datos durante la ejecución de una directiva de Azure AD B2C. El [esquema de notificaciones](claimsschema.md) es el lugar en el que se declaran las notificaciones.

1. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue las notificaciones siguientes al elemento **ClaimsSchema**. 

```xml
<ClaimType Id="EventType">
  <DisplayName>Event type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="EventTimestamp">
  <DisplayName>Event timestamp</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="PolicyId">
  <DisplayName>Policy Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="Culture">
  <DisplayName>Culture ID</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="CorrelationId">
  <DisplayName>Correlation Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="federatedUser">
  <DisplayName>Federated user</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
<ClaimType Id="parsedDomain">
  <DisplayName>Domain name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>The domain portion of the email address.</UserHelpText>
</ClaimType>
<ClaimType Id="userInLocalDirectory">
  <DisplayName>userInLocalDirectory</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
```

## <a name="add-new-technical-profiles"></a>Incorporación de nuevos perfiles técnicos

Los perfiles técnicos pueden considerarse funciones en la directiva personalizada. En esta tabla se definen los perfiles técnicos que se usan para abrir una sesión y publicar eventos. La solución usa el enfoque de [inclusión de perfil técnico](technicalprofiles.md#include-technical-profile). Donde un perfil técnico incluya otro perfil técnico para cambiar la configuración o agregar funcionalidades.

| Perfil técnico | Tarea |
| ----------------- | -----|
| AppInsights-Common | El perfil técnico común con el conjunto de configuración común. Incluida la clave de instrumentación de Application Insights, la colección de notificaciones que se van a registrar y el modo de desarrollador. En los siguientes perfiles técnicos se incluye el perfil técnico común y se agregan más notificaciones, como el nombre del evento. |
| AppInsights-SignInRequest | Registra un `SignInRequest` evento con un conjunto de notificaciones cuando se ha recibido una solicitud de inicio de sesión. |
| AppInsights-UserSignUp | Registra un evento `UserSignUp` cuando el usuario activa la opción de inicio de sesión en un recorrido de inicio de sesión y registro. |
| AppInsights-SignInComplete | Registra un `SignInComplete` evento de finalización correcta de una autenticación cuando se ha enviado un token a la aplicación de usuario de confianza. |

Agregue los perfiles al archivo *TrustFrameworkExtensions.xml* desde el módulo de inicio. Agregue estos elementos al elemento **ClaimsProviders**:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AppInsights-Common">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <Item Key="DeveloperMode">false</Item>
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="EventTimestamp" PartnerClaimType="{property:EventTimestamp}" DefaultValue="{Context:DateTimeInUtc}" />
        <InputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="{property:TenantId}" DefaultValue="{Policy:TrustFrameworkTenantId}" />
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-UserSignUp">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignUp" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
    
    <TechnicalProfile Id="AppInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
        <InputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="{property:IDP}" DefaultValue="Local" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

> [!IMPORTANT]
> Cambie la clave de instrumentación del perfil técnico de `AppInsights-Common` por el GUID que su recurso de Application Insights proporciona.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Incorporación de los perfiles técnicos como pasos de orquestación

Llame a `AppInsights-SignInRequest` como paso de orquestación 2 para realizar el seguimiento de que se ha recibido una solicitud de inicio de sesión o suscripción:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AppInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Inmediatamente *antes* del paso de orquestación `SendClaims`, agregue un nuevo paso que llame a `AppInsights-UserSignup`. Se desencadena cuando el usuario selecciona el botón de suscripción en un recorrido de inicio de sesión o suscripción.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AppInsights-UserSignup" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Inmediatamente después del paso de orquestación `SendClaims`, llame a `AppInsights-SignInComplete`. Este paso muestra un recorrido completado correctamente.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AppInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Después de agregar los nuevos pasos de orquestación, vuelva a enumerar los pasos de forma secuencial sin omitir los enteros de 1 a N.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Carga del archivo, ejecución de la directiva y visualización de eventos

Cargue el archivo *TrustFrameworkExtensions.xml*. Luego, llame a la directiva de usuario de confianza desde su aplicación o use **Ejecutar ahora** en Azure Portal. Espere aproximadamente un minuto y los eventos estarán disponibles en Application Insights.

1. Abra el recurso de **Application Insights** en su inquilino de Azure Active Directory.
2. Seleccione **Uso** y, a continuación, **Eventos**.
3. Establezca **During** (Durante) en **Last hour** (Última hora) y **By** (por) en **3 minutes** (3 minutos).  Puede que tenga que seleccionar **Refresh** (Actualizar) para ver los resultados.

![Hoja de eventos de uso de Application Insights](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="collect-more-data"></a>Recopilar más datos

Para ajustarse a sus necesidades empresariales, puede que desee registrar más notificaciones. Para agregar una notificación, primero [defina una notificación](#define-claims) y, después, agregue la notificación a la colección de notificaciones de entrada. Las notificaciones que agregue al perfil técnico *AppInsights-Common* aparecerán en todos los eventos. Las notificaciones que agregue a un perfil técnico específico aparecerán solo en ese evento. El elemento de notificación de entrada contiene los atributos siguientes:

- **ClaimTypeReferenceId**: es la referencia a un tipo de notificación. 
- **PartnerClaimType**: es el nombre de la propiedad que aparece en Azure Insights. Use la sintaxis de `{property:NAME}`, donde `NAME` es la propiedad que se agrega al evento.
- **DefaultValue**: un valor predefinido que se debe registrar, como el nombre del evento. Una notificación que se usa en el recorrido del usuario, como el nombre del proveedor de identidades. Si la notificación está vacía, se usará el valor predeterminado. Por ejemplo, la notificación `identityProvider` se establece mediante los perfiles técnicos de la federación, como Facebook. Si la notificación está vacía, indica el inicio de sesión de usuario con una cuenta local. Por lo tanto, el valor predeterminado se establece en *Local*. También puede registrar [resoluciones de notificaciones](claim-resolver-overview.md) con un valor contextual, como el id. de aplicación o la dirección IP del usuario.

### <a name="manipulating-claims"></a>Manipulación de notificaciones

Puede usar [transformaciones de notificaciones de entrada](custom-policy-trust-frameworks.md#manipulating-your-claims) para modificar las notificaciones de entrada o generar otras nuevas antes de enviarlas a Application Insights. En el ejemplo siguiente, el perfil técnico incluye la *CheckIsAdmin* la transformación de notificaciones de entrada. 

```xml
<TechnicalProfile Id="AppInsights-SignInComplete">
  <InputClaimsTransformations>  
    <InputClaimsTransformation ReferenceId="CheckIsAdmin" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isAdmin" PartnerClaimType="{property:IsAdmin}"  />
    ...
  </InputClaims>
  <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
</TechnicalProfile>
```

### <a name="add-events"></a>Agregar eventos

Para agregar un evento, cree un nuevo perfil técnico que incluya el perfil técnico de *AppInsights-Common*. A continuación, agregue el perfil técnico como paso de orquestación al [recorrido de usuario](custom-policy-trust-frameworks.md#orchestration-steps). Utilice [precondition](userjourneys.md#preconditions) para desencadenar el evento cuando lo desee. Por ejemplo, informe del evento solo cuando los usuarios se ejecuten mediante MFA.

```xml
<TechnicalProfile Id="AppInsights-MFA-Completed">
  <InputClaims>
     <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="MFA-Completed" />
  </InputClaims>
  <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
</TechnicalProfile>
```

Ahora que tiene un perfil técnico, agregue el evento al recorrido del usuario. A continuación, vuelva a enumerar los pasos de forma secuencial sin omitir ningún entero de 1 a N.

```xml
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
    <Value>isActiveMFASession</Value>
    <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserMfaCompleted" TechnicalProfileReferenceId="AppInsights-MFA-Completed" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="enable-developer-mode"></a>Habilitar el modo de desarrollador

Al usar Application Insights para definir eventos, puede indicar si está habilitado el modo de desarrollador. El modo de desarrollador controla cómo se almacenan en búfer los eventos. En un entorno de desarrollo con un volumen de eventos mínimo, al habilitar el modo de desarrollador, los eventos se envían inmediatamente a Application Insights. El valor predeterminado es `false`. No habilite el modo de desarrollador en entornos de producción.

Para habilitar el modo de desarrollador, en el perfil técnico *AppInsights-Common*, cambie los metadatos de `DeveloperMode` a `true`: 

```xml
<TechnicalProfile Id="AppInsights-Common">
  <Metadata>
    ...
    <Item Key="DeveloperMode">true</Item>
  </Metadata>
</TechnicalProfile>
```

## <a name="disable-telemetry"></a>Deshabilitar la telemetría

Para deshabilitar los registros de Application Insights, en el perfil técnico *AppInsights-Common*, cambie los metadatos de `DisableTelemetry` a `true`: 

```xml
<TechnicalProfile Id="AppInsights-Common">
  <Metadata>
    ...
    <Item Key="DisableTelemetry">true</Item>
  </Metadata>
</TechnicalProfile>
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear paneles de indicadores clave de rendimiento (KPI) personalizados con Azure Application Insights](../azure-monitor/learn/tutorial-app-dashboards.md). 

::: zone-end