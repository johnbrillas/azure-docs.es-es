---
title: Complemento de Microsoft Enterprise SSO para dispositivos Apple
titleSuffix: Microsoft identity platform | Azure
description: Obtenga información sobre el complemento SSO de Microsoft Azure Active Directory para dispositivos iOS, iPadOS y macOS.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 96fbf23128896f23beee70a6b3d32b4b87a454ea
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102427103"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Complemento de Microsoft Enterprise SSO para dispositivos Apple (versión preliminar)

>[!IMPORTANT]
> Esta característica [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

El *complemento Microsoft Enterprise SSO para dispositivos de Apple* proporciona cuentas de inicio de sesión único (SSO) para Azure Active Directory (Azure AD) en macOS, iOS y iPadOS en todas las aplicaciones que admiten la característica [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices) de Apple. Esto incluye las aplicaciones antiguas de las que puede depender su empresa, pero que aún no admiten los protocolos o las bibliotecas de identidades más recientes. Microsoft ha trabajado estrechamente con Apple para desarrollar este complemento con el fin de aumentar la facilidad de uso de su aplicación y ofrecer la máxima protección de la que disponen Apple y Microsoft.

El complemento Enterprise SSO está disponible actualmente como una característica integrada de las siguientes aplicaciones:

* [Microsoft Authenticator](../user-help/user-help-auth-app-overview.md): iOS, iPadOS
* [Portal de empresa](/mem/intune/apps/apps-company-portal-macos) de Microsoft Intune: macOS

## <a name="features"></a>Características

El complemento Microsoft Enterprise SSO para dispositivos de Apple ofrece las siguientes ventajas:

- Proporciona SSO para las cuentas de Azure AD en todas las aplicaciones que admiten la característica Enterprise Single Sign-On de Apple.
- Se puede habilitar con cualquier solución de administración de dispositivos móviles (MDM).
- Extiende el inicio de sesión único a aplicaciones que aún no usan las bibliotecas de la plataforma de identidad de Microsoft.
- Extiende el inicio de sesión único a las aplicaciones que usan OAuth2, OpenID Connect y SAML.

## <a name="requirements"></a>Requisitos

Para usar el complemento de Microsoft Enterprise SSO para dispositivos de Apple:

- El dispositivo debe **admitir** y tener una aplicación que incluya el complemento Microsoft Enterprise SSO para dispositivos de Apple **instalado**:
  - iOS 13.0 y versiones posteriores: [aplicación Microsoft Authenticator](../user-help/user-help-auth-app-overview.md)
  - iPadOS 13.0 y versiones posteriores: [aplicación Microsoft Authenticator](../user-help/user-help-auth-app-overview.md)
  - macOS 10.15 y versiones posteriores: [aplicación Portal de empresa de Intune](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp)
- El dispositivo debe estar **inscrito en MDM** (por ejemplo, con Microsoft Intune).
- La configuración se debe **insertar en el dispositivo** para habilitar el complemento Enterprise SSO en el dispositivo. Apple requiere esta restricción de seguridad.

### <a name="ios-requirements"></a>Requisitos de iOS:
- iOS 13.0 o posterior debe estar instalado en el dispositivo.
- Debe instalarse una aplicación de Microsoft que proporcione el complemento Microsoft Enterprise SSO para dispositivos de Apple en el dispositivo. En el caso de la versión preliminar pública, estas aplicaciones se refieren a la [aplicación Microsoft Authenticator](/intune/user-help/user-help-auth-app-overview.md).


### <a name="macos-requirements"></a>Requisitos de macOS:
- macOS 10.15 o una versión posterior debe estar instalado en el dispositivo. 
- Debe instalarse una aplicación de Microsoft que proporcione el complemento Microsoft Enterprise SSO para dispositivos de Apple en el dispositivo. En el caso de la versión preliminar pública, estas aplicaciones incluyen la [aplicación Portal de empresa de Intune](/intune/user-help/enroll-your-device-in-intune-macos-cp.md).

## <a name="enable-the-sso-plug-in-with-mobile-device-management-mdm"></a>Habilitación del complemento de inicio de sesión único con administración de dispositivos móviles (MDM)

### <a name="microsoft-intune-configuration"></a>Configuración de Microsoft Intune

Si usa Microsoft Intune como su servicio MDM, puede usar la configuración del perfil de configuración integrado para habilitar el complemento Microsoft Enterprise SSO.

En primer lugar, configure la [extensión de la aplicación de inicio de sesión único](/mem/intune/configuration/device-features-configure#single-sign-on-app-extension) de un perfil de configuración y [asigne el perfil a un usuario o un grupo de dispositivos](/mem/intune/configuration/device-profile-assign) (si aún no se ha asignado).

La configuración del perfil que habilita el complemento SSO se aplica automáticamente a los dispositivos del grupo la siguiente vez que cada dispositivo se registra con Intune.

### <a name="manual-configuration-for-other-mdm-services"></a>Configuración manual para otros servicios de MDM

Si no usa Microsoft Intune para la administración de dispositivos móviles, use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO para dispositivos Apple.

#### <a name="ios-settings"></a>Configuración de iOS:

- **Identificador de extensión**: `com.microsoft.azureauthenticator.ssoextension`
- **Id. de equipo**: (este campo no es necesario en iOS).

#### <a name="macos-settings"></a>Configuración de macOS:

- **Identificador de extensión**: `com.microsoft.CompanyPortalMac.ssoextension`
- **Identificador del equipo**: `UBF8T346G9`

#### <a name="common-settings"></a>Configuración común:

- **Tipo**: Redirigir
  - `https://login.microsoftonline.com`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`
  
### <a name="additional-configuration-options"></a>Opciones de configuración adicionales
Se pueden agregar opciones de configuración adicionales para ampliar la funcionalidad de inicio de sesión único a otras aplicaciones.

#### <a name="enable-sso-for-apps-that-dont-use-a-microsoft-identity-platform-library"></a>Habilitación del inicio de sesión único para aplicaciones que no usan una biblioteca de plataforma de identidad de Microsoft

El complemento de inicio de sesión único permite que cualquier aplicación participe en el inicio de sesión único, aunque no se haya desarrollado con un SDK de Microsoft, como la Biblioteca de autenticación de Microsoft (MSAL).

El complemento SSO se instala automáticamente en los dispositivos que han descargado la aplicación Microsoft Authenticator en iOS y iPadOS o la aplicación Portal de empresa de Intune en macOS y han registrado el dispositivo en la organización. Es probable que la organización ya use la aplicación Authenticator en escenarios como la autenticación multifactor, la autenticación sin contraseña y el acceso condicional. Aunque se puede activar en las aplicaciones con cualquier proveedor de MDM, Microsoft ha facilitado su configuración en Microsoft Endpoint Manager de Intune. Para configurar estas aplicaciones de forma que usen el complemento de inicio de sesión único, se usa una lista de permitidos.

>[!IMPORTANT]
> Solo se admiten las aplicaciones que usan vistas web o tecnologías de red de Apple nativas. Si una aplicación incluye su propia implementación de capa de red, no se admite el complemento Microsoft Enterprise SSO.  

Use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO en dispositivos que no usan MSAL:

Si desea proporcionar una lista de aplicaciones específicas:

- **Clave**: `AppAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de identificadores de lote de aplicaciones que pueden participar en el inicio de sesión único.
- **Ejemplo**: `com.contoso.workapp, com.contoso.travelapp`

O bien, si desea proporcionar una lista de prefijos:
- **Clave**: `AppPrefixAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de los prefijos de identificadores de lote de las aplicaciones que pueden participar en el inicio de sesión único. Tenga en cuenta que esto permitirá que todas las aplicaciones que comienzan con un prefijo determinado participen en el SSO.
- **Ejemplo**: `com.contoso., com.fabrikam.`

Las [aplicaciones consentidas](./application-consent-experience.md) que el administrador de MDM permita participar en el inicio de sesión único pueden obtener un token para el usuario final de forma silenciosa. Por lo tanto, es importante agregar únicamente aplicaciones de confianza a la lista de permitidos. 

>[!NOTE]
> No es necesario agregar aplicaciones que usen MSAL o ASWebAuthenticationSession a esta lista. Esas aplicaciones están habilitadas de forma predeterminada. 

##### <a name="how-to-discover-app-bundle-identifiers-on-ios-devices"></a>Detección de identificadores de lote de aplicaciones en dispositivos iOS

Apple no proporciona una manera fácil de detectar los id. de agrupación desde App Store. La manera más sencilla de detectar los id. de agrupación de las aplicaciones que desean usar SSO es preguntar al proveedor o al desarrollador de la aplicación. Si esa opción no está disponible, puede usar la configuración de MDM para detectar los id. de agrupación. 

Habilite temporalmente la siguiente marca en la configuración de MDM:

- **Clave**: `admin_debug_mode_enabled`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

Si esta marca está activada, inicie sesión en las aplicaciones iOS en el dispositivo del que desea conocer el id. de paquete. Aplicación Microsoft Authenticator abierta -> Ayuda -> Enviar registros -> Ver registros. 

En el archivo de registro, busque la siguiente línea:

`[ADMIN MODE] SSO extension has captured following app bundle identifiers:`

Esto debería capturar todos los identificadores de lote de aplicaciones visibles para la extensión de SSO. Después, puede usar esos identificadores para configurar el inicio de sesión único (SSO) para esas aplicaciones. 

#### <a name="allow-user-to-sign-in-from-unknown-applications-and-the-safari-browser"></a>Permita que el usuario inicie sesión desde aplicaciones desconocidas y el explorador Safari.

De forma predeterminada, el complemento Microsoft Enterprise SSO proporciona SSO para aplicaciones autorizadas solo cuando un usuario ha iniciado sesión desde una aplicación que usa una biblioteca de la plataforma de identidad de Microsoft, como ADAL o MSAL. El complemento Microsoft Enterprise SSO también puede adquirir una credencial compartida cuando la llama otra aplicación que usa una biblioteca de plataforma de identidad de Microsoft durante una nueva adquisición de token.

La habilitación de la marca `browser_sso_interaction_enabled` permite que la aplicación que no usa una biblioteca de la plataforma de identidad de Microsoft realice el arranque inicial y obtenga una credencial compartida. También permite que el explorador Safari realice el arranque inicial y obtenga una credencial compartida. Si el complemento Microsoft Enterprise SSO aún no tiene una credencial compartida, intentará obtener una cada vez que se solicite un inicio de sesión desde una dirección URL de Azure AD en el explorador Safari, ASWebAuthenticationSession, SafariViewController u otra aplicación nativa incluida permitida.  

- **Clave**: `browser_sso_interaction_enabled`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

Para macOS, este valor es necesario para obtener una experiencia más coherente en todas las aplicaciones. Para iOS y iPadOS, esta configuración no es necesaria, ya que la mayoría de las aplicaciones usan la aplicación Microsoft Authenticator para el inicio de sesión. Sin embargo, si tiene algunas aplicaciones que no usan Microsoft Authenticator en iOS o iPadOS, esta marca mejorará la experiencia, por lo que le recomendamos que habilite la configuración. Esta opción está deshabilitada de manera predeterminada.

#### <a name="disable-asking-for-mfa-on-initial-bootstrapping"></a>Deshabilitación de la solicitud de MFA en el arranque inicial

De forma predeterminada, el complemento Microsoft Enterprise SSO siempre solicita al usuario la autenticación multifactor (MFA) al realizar el arranque inicial y obtener una credencial compartida, incluso si no es necesario para la aplicación actual que ha iniciado el usuario. De este modo, la credencial compartida se puede usar fácilmente en todas las aplicaciones adicionales sin preguntar al usuario si se requiere MFA más adelante. Esto reduce las veces que se debe preguntar al usuario en el dispositivo y, por lo general, es una buena decisión.

Si habilita `browser_sso_disable_mfa`, esta opción se desactivará y solo se preguntará al usuario cuando una aplicación o un recurso necesite MFA. 

- **Clave**: `browser_sso_disable_mfa`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

Se recomienda mantener esta marca deshabilitada, ya que reduce el número de veces que se debe preguntar al usuario en el dispositivo. Si su organización rara vez utiliza MFA, es posible que quiera habilitar la marca, pero se recomienda usar MFA con mayor frecuencia en su lugar. Por esta razón, está deshabilitada de forma predeterminada.

#### <a name="disable-oauth2-application-prompts"></a>Deshabilitación de los avisos de aplicaciones OAuth2

El complemento Microsoft Enterprise SSO proporciona inicio de sesión único mediante la anexión de credenciales compartidas a las solicitudes de red procedentes de aplicaciones permitidas. Sin embargo, algunas aplicaciones OAuth2 pueden exigir incorrectamente los mensajes del usuario final en la capa del protocolo. Si esto sucede, verá que las credenciales compartidas se omiten para esas aplicaciones y se solicita al usuario que inicie sesión aunque el complemento Microsoft Enterprise SSO esté funcionando para otras aplicaciones.  

Al habilitar la marca `disable_explicit_app_prompt`, se restringe la posibilidad de que las aplicaciones web y nativas fuercen un aviso al usuario final en la capa del protocolo y omitan el inicio de sesión único.

- **Clave**: `disable_explicit_app_prompt`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

Se recomienda habilitar esta marca para obtener una experiencia más uniforme entre todas las aplicaciones. Esta opción está deshabilitada de manera predeterminada. 

#### <a name="enable-sso-through-cookies-for-specific-application"></a>Habilitación de SSO a través de cookies para una aplicación específica

Un pequeño número de aplicaciones podría ser incompatible con la extensión de SSO. En concreto, las aplicaciones que tienen una configuración de red avanzada pueden experimentar problemas inesperados cuando se habilitan para el inicio de sesión único (por ejemplo, es posible que vea un error que indique que la solicitud de red se canceló o se interrumpió). 

Si tiene problemas para iniciar sesión con el método descrito en la sección `Enable SSO for apps that don't use MSAL`, puede probar la configuración alternativa para esas aplicaciones. 

Use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO para esas aplicaciones específicas:

- **Clave**: `AppCookieSSOAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de los prefijos de identificadores de lote de las aplicaciones que pueden participar en el inicio de sesión único. Tenga en cuenta que esto permitirá que todas las aplicaciones que comienzan con un prefijo determinado participen en el SSO.
- **Ejemplo**: `com.contoso.myapp1, com.fabrikam.myapp2`

Tenga en cuenta que las aplicaciones habilitadas para el SSO mediante este mecanismo deben agregarse a `AppCookieSSOAllowList` y `AppPrefixAllowList`.

Se recomienda probar esta opción solo para aplicaciones que experimentan errores de inicio de sesión inesperados. 

#### <a name="use-intune-for-simplified-configuration"></a>Uso de Intune para la configuración simplificada

Como se indicó anteriormente, puede usar Microsoft Intune como su servicio MDM para facilitar la configuración del complemento Microsoft Enterprise SSO, como habilitar el complemento y agregar las aplicaciones anteriores a una lista de permitidos para que obtengan SSO. Para obtener más información, consulte la [documentación de configuración de Intune](/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-plug-in-in-your-application"></a>Uso del complemento de inicio de sesión único en la aplicación

La [Biblioteca de autenticación de Microsoft (MSAL) para dispositivos Apple](https://github.com/AzureAD/microsoft-authentication-library-for-objc) de la versión 1.1.0 y versiones posteriores admite el complemento Microsoft Enterprise SSO para dispositivos Apple. Es el método recomendado para agregar compatibilidad con el complemento Microsoft Enterprise SSO y garantiza que obtendrá todas las funcionalidades de la plataforma de identidad de Microsoft.

Si va a compilar una aplicación para escenarios de Frontline Worker, consulte [Modo de dispositivo compartido para dispositivos iOS](msal-ios-shared-devices.md) para conocer la configuración adicional de la característica.

## <a name="how-the-sso-plug-in-works"></a>Funcionamiento del complemento de inicio de sesión único

El complemento Microsoft Enterprise SSO se basa en el [marco Enterprise Single Sign-On de Apple](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc). Los proveedores de identidades que se incorporan al marco pueden interceptar el tráfico de red en sus dominios y mejorar o cambiar el modo en que se administran esas solicitudes. Por ejemplo, el complemento de inicio de sesión único puede mostrar una interfaz de usuario adicional para recopilar credenciales de usuario final de forma segura, exigir MFA o proporcionar tokens a la aplicación de forma silenciosa.

Las aplicaciones nativas también pueden implementar operaciones personalizadas y comunicarse directamente con el complemento de inicio de sesión único.
Puede aprender más sobre el marco de inicio de sesión único en este [vídeo de WWDC de 2019 de Apple](https://developer.apple.com/videos/play/tech-talks/301/).

### <a name="applications-that-use-a-microsoft-identity-platform-library"></a>Aplicaciones que usan una biblioteca de plataforma de identidad de Microsoft

La [Biblioteca de autenticación de Microsoft (MSAL) para dispositivos Apple](https://github.com/AzureAD/microsoft-authentication-library-for-objc) de la versión 1.1.0 y posteriores admite de forma nativa el complemento Microsoft Enterprise SSO para dispositivos Apple con cuentas profesionales y educativas. 

Si ha seguido [todos los pasos recomendados](./quickstart-v2-ios.md) y ha usado el [formato de URI de redirección](./redirect-uris-ios.md) predeterminado, no es necesaria ninguna configuración especial. Cuando se ejecuta en un dispositivo en el que existe el complemento de inicio de sesión único, MSAL lo invoca automáticamente en todas las solicitudes de token interactivas y silenciosas, así como en las operaciones de enumeración y eliminación de cuentas. Dado que MSAL implementa el protocolo del complemento de inicio de sesión único nativo que depende de operaciones personalizadas, esta instalación proporciona la mejor experiencia nativa para el usuario final. 

Si MDM no habilita el complemento de inicio de sesión único, pero la aplicación Microsoft Authenticator existe en el dispositivo, MSAL la usará en su lugar con todas las solicitudes de token interactivas. El complemento de inicio de sesión único comparte el inicio de sesión único con la aplicación Microsoft Authenticator.

### <a name="applications-that-dont-use-a-microsoft-identity-platform-library"></a>Aplicaciones que no usan una biblioteca de plataforma de identidad de Microsoft

Las aplicaciones que no usan una biblioteca de plataforma de identidad de Microsoft, como MSAL, también pueden obtener el inicio de sesión único si un administrador las agrega explícitamente a la lista de permitidos. 

No es necesario cambiar el código de esas aplicaciones siempre y cuando se cumplan las siguientes condiciones:

- La aplicación usa marcos de Apple para ejecutar las solicitudes de red (por ejemplo, [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession)) 
- La aplicación usa protocolos estándar para comunicarse con Azure AD (por ejemplo, OAuth2, SAML, WS-Federation)
- La aplicación no recopila el nombre de usuario y la contraseña de texto no cifrado en la interfaz de usuario nativa

En este caso, el inicio de sesión único se proporciona cuando la aplicación crea una solicitud de red y abre un explorador web para iniciar la sesión del usuario. Cuando a un usuario se le redirige a una dirección URL de inicio de sesión de Azure AD, el complemento de inicio de sesión único la valida y comprueba si hay una credencial de inicio de sesión único disponible para ella. Si hay una, el complemento de inicio de sesión único pasa la credencial de inicio de sesión único a Azure AD, que autoriza a la aplicación que lleve a cabo la solicitud de red sin pedir al usuario que escriba sus credenciales. Además, si Azure AD conoce el dispositivo, el complemento de inicio de sesión único también aprueba el certificado de este para satisfacer la comprobación de acceso condicional basada en dispositivo. 

Para admitir el inicio de sesión único en aplicaciones que no son de MSAL, el complemento de inicio de sesión único implementa un protocolo similar al del complemento de explorador de Windows descrito en [¿Qué es un token de actualización principal?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt). 

En comparación con las aplicaciones basadas en MSAL, el complemento de inicio de sesión único funciona de forma más transparente con las aplicaciones que no son de MSAL gracias a la integración con la experiencia de inicio de sesión del explorador existente que proporcionan las aplicaciones. El usuario final verá la experiencia con la que ya está familiarizado, con la ventaja de no tener que realizar inicios de sesión adicionales en cada una de las aplicaciones. Por ejemplo, en lugar de mostrar el selector de cuentas nativo, el complemento de inicio de sesión único agrega sesiones de inicio de sesión único a la experiencia del selector de cuentas basado en web. 

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el modo de dispositivo compartido en iOS, consulte [Modo de dispositivo compartido para dispositivos iOS](msal-ios-shared-devices.md).
