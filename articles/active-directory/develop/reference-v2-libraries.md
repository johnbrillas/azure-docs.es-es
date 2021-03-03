---
title: Bibliotecas de autenticación de la Plataforma de identidad de Microsoft | Azure
description: Lista de bibliotecas de cliente y middleware compatible con la Plataforma de identidad de Microsoft. Use estas bibliotecas para agregar compatibilidad con el inicio de sesión de usuario (autenticación) y acceso a API web protegida (autorización) a las aplicaciones.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 01/29/2021
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 228a15e9e9e27cbcfd71d4762db2f4ab9f6dfffe
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560160"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Bibliotecas de autenticación de la Plataforma de identidad de Microsoft

En las tablas siguientes se muestra la compatibilidad de la Biblioteca de autenticación de Microsoft con varios tipos de aplicaciones. Incluyen vínculos al código fuente de la biblioteca, dónde obtener el paquete para el proyecto de la aplicación y si la biblioteca admite el inicio de sesión de usuario (autenticación), el acceso a API web protegidas (autorización) o ambos.

OpenID Foundation ha certificado la Plataforma de identidad de Microsoft como un [proveedor certificado de OpenID](https://openid.net/certification/). Si prefiere usar una biblioteca que no sea la biblioteca de autenticación de Microsoft (MSAL) u otra biblioteca compatible con Microsoft, elija una con una [implementación certificada de OpenID Connect](https://openid.net/developers/certified/).

Si decide codificar manualmente su propia implementación de nivel de protocolo de [OAuth 2.0 u OpenID Connect 1.0](active-directory-v2-protocols.md), preste especial atención a las consideraciones de seguridad de cada especificación estándar y siga una metodología de ciclo de vida de desarrollo de software (SDL), como [Microsoft SDL][Microsoft-SDL].

## <a name="single-page-application-spa"></a>Aplicación de página única (SPA)

Una aplicación de una sola página se ejecuta completamente en la superficie del explorador y captura los datos de la página (HTML, CSS y JavaScript) dinámicamente o en el momento de la carga de la aplicación. Puede llamar a las API web para interactuar con los orígenes de datos de back-end.

Dado que el código de SPA se ejecuta completamente en el explorador, se considera un *cliente público* que no puede almacenar secretos de forma segura.

| Lenguaje/marco de trabajo | Proyecto en<br/>GitHub                                                                                                    | Paquete                                                                      | Introducción<br/>iniciado                             | Inicio de sesión de usuarios                                         | Acceso a API web                                                 | Disponible con carácter general (GA) *o*<br/>Versión preliminar pública<sup>1</sup> |
|----------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|:-----------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Angular              | [MSAL Angular 2.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)         | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | —                                               | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | Versión preliminar pública                                               |
| Angular              | [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/msal-angular-v1/lib/msal-angular) | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | [Tutorial](tutorial-v2-angular.md)              | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| AngularJS            | [MSAL AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs)         | [@azure/msal-angularjs](https://www.npmjs.com/package/@azure/msal-angularjs) | —                                               | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | Versión preliminar pública                                               |
| JavaScript           | [MSAL.js 2.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)              | [@azure/msal-browser](https://www.npmjs.com/package/@azure/msal-browser)     | [Tutorial](tutorial-v2-javascript-auth-code.md) | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| JavaScript           | [MSAL.js 1.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core)              | [@azure/msal-core](https://www.npmjs.com/package/@azure/msal-core)     | [Tutorial](tutorial-v2-javascript-spa.md) | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| React                | [MSAL React](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-react)                 | [@azure/msal-react](https://www.npmjs.com/package/@azure/msal-react)         | —                                               | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | Versión preliminar pública                                               |
<!--
| Vue | [Vue MSAL]( https://github.com/mvertopoulos/vue-msal) | [vue-msal]( https://www.npmjs.com/package/vue-msal) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Los términos de uso complementarios para las versiones preliminares de Microsoft Azure][preview-tos] se aplican a las bibliotecas en la *versión preliminar pública*.

## <a name="web-application"></a>Aplicación web

Una aplicación web ejecuta código en un servidor que genera y envía HTML, CSS y JavaScript al explorador web de un usuario que se va a representar. La identidad del usuario se mantiene como una sesión entre el explorador del usuario (el front-end) y el servidor web (el back-end).

Dado que el código de una aplicación web se ejecuta en el servidor web, se considera un *cliente confidencial* que puede almacenar secretos de forma segura.

| Lenguaje/marco de trabajo | Proyecto en<br/>GitHub                                                                                     | Paquete                                                                                                    | Introducción<br/>iniciado                               | Inicio de sesión de usuarios                                            | Acceso a API web                                                    | Disponible con carácter general (GA) *o*<br/>Versión preliminar pública<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|:-------------------------------------------------:|:--------------------------------------------------------:|:------------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)                      | —                                                 | ![La biblioteca no puede solicitar tokens de id. para el inicio de sesión de usuario.][n] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y]    | GA                                                           |
| ASP.NET Core         | [Seguridad de ASP.NET](/aspnet/core/security/)                                                                | [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) | —                                                 | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca no puede solicitar tokens de acceso para las API web protegidas.][n] | GA                                                           |
| ASP.NET Core         | [Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web)                               | [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web)                            | —                                                 | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y]    | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://search.maven.org/artifact/com.microsoft.azure/msal4j)                                     | [Guía de inicio rápido](quickstart-v2-java-webapp.md)        | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y]    | GA                                                           |
| Node.js              | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)                                                | [Guía de inicio rápido](quickstart-v2-nodejs-webapp-msal.md) | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y]    | GA                                               |
| Node.js              | [Azure AD Passport](https://github.com/AzureAD/passport-azure-ad)                                         | [passport-azure-ad](https://www.npmjs.com/package/passport-azure-ad)                                       | [Guía de inicio rápido](quickstart-v2-nodejs-webapp.md)      | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)                     | [msal](https://pypi.org/project/msal)                                                                      | [Guía de inicio rápido](quickstart-v2-python-webapp.md)      | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y]    | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y]    | GA                                                           |
<!--
| Java | [ScribeJava](https://github.com/scribejava/scribejava) | [ScribeJava 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Java | [Gluu oxAuth](https://github.com/GluuFederation/oxAuth) | [oxAuth 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| Node.js | [openid-client](https://github.com/panva/node-openid-client/) | [openid-client 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| PHP | [PHP League oauth2-client](https://github.com/thephpleague/oauth2-client) | [oauth2-client 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth) | [omniauth 1.3.1](https://github.com/omniauth/omniauth/releases/tag/v1.3.1)<br/>[omniauth-oauth2 1.4.0](https://github.com/intridea/omniauth-oauth2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Los términos de uso complementarios para las versiones preliminares de Microsoft Azure][preview-tos] se aplican a las bibliotecas en la *versión preliminar pública*.

## <a name="desktop-application"></a>Aplicación de escritorio

Una aplicación de escritorio suele ser código binario (compilado) que muestra una interfaz de usuario y que se ha diseñado para ejecutarse en el escritorio del usuario.

Dado que una aplicación de escritorio se ejecuta en el escritorio del usuario, se considera un *cliente público* que no puede almacenar secretos de forma segura.

| Lenguaje/marco de trabajo | Proyecto en<br/>GitHub                                                                                     | Paquete                                                                               | Introducción<br/>iniciado                        | Inicio de sesión de usuarios                                         | Acceso a API web                                                 | Disponible con carácter general (GA) *o*<br/>Versión preliminar pública<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Electron             | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [@azure/msal-node](https://www.npmjs.com/package/@azure/msal-node)                    | [Tutorial](tutorial-v2-nodejs-desktop.md)   | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                               |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j)               | —                                          | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| macOS (Swift/Obj-C)  | [MSAL para iOS y macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc)            | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [Tutorial](tutorial-v2-ios.md)             | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| UWP                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Tutorial](tutorial-v2-windows-uwp.md)     | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| WPF                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Tutorial](tutorial-v2-windows-desktop.md) | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
<!--
| Java | Scribe | [Scribe Java](https://mvnrepository.com/artifact/org.scribe/scribe) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| React Native | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Los términos de uso complementarios para las versiones preliminares de Microsoft Azure][preview-tos] se aplican a las bibliotecas en la *versión preliminar pública*.

## <a name="mobile-application"></a>Aplicación para dispositivos móviles

Una aplicación móvil suele ser código binario (compilado) que muestra una interfaz de usuario y que se ha diseñado para ejecutarse en un dispositivo móvil del usuario.

Dado que una aplicación móvil se ejecuta en el dispositivo móvil del usuario, se considera un *cliente público* que no puede almacenar secretos de forma segura.

| Plataforma          | Proyecto en<br/>GitHub                                                                          | Paquete                                                                               | Introducción<br/>iniciado                    | Inicio de sesión de usuarios                                         | Acceso a API web                                                 | Disponible con carácter general (GA) *o*<br/>Versión preliminar pública<sup>1</sup> |
|-------------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:--------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Android (Java)    | [MSAL de Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | [Guía de inicio rápido](quickstart-v2-android.md) | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| Android (Kotlin)  | [MSAL de Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | —                                      | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| iOS (Swift/Obj-C) | [MSAL para iOS y macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [Tutorial](tutorial-v2-ios.md)         | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| Xamarin (.NET)    | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)             | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | —                                      | ![La biblioteca puede solicitar tokens de id. para el inicio de sesión de usuario.][y] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
<!--
| React Native |[React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Los términos de uso complementarios para las versiones preliminares de Microsoft Azure][preview-tos] se aplican a las bibliotecas en la *versión preliminar pública*.

## <a name="service--daemon"></a>Servicio o demonio

Los servicios y los demonios suelen usarse para la comunicación de servidor a servidor y otras comunicaciones desatendidas (a veces denominados *equipos sin periféricos*). Dado que no hay ningún usuario en el teclado para escribir las credenciales o dar su consentimiento para el acceso a los recursos, estas aplicaciones se autentican a sí mismas, no a un usuario, al solicitar acceso autorizado a los recursos de una API web.

Un servicio o demonio que se ejecuta en un servidor se considera un *cliente confidencial* que puede almacenar sus secretos de forma segura.

| Lenguaje/marco de trabajo | Proyecto en<br/>GitHub                                                                 | Paquete                                                                                | Introducción<br/>iniciado                           | Inicio de sesión de usuarios                                            | Acceso a API web                                                 | Disponible con carácter general (GA) *o*<br/>Versión preliminar pública<sup>1</sup> |
|----------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|:---------------------------------------------:|:--------------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)    | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client/) | [Guía de inicio rápido](quickstart-v2-netcore-daemon.md) | ![La biblioteca no puede solicitar tokens de id. para el inicio de sesión de usuario.][n] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)        | [msal4j](https://javadoc.io/doc/com.microsoft.azure/msal4j/latest/index.html)          | —                                             | ![La biblioteca no puede solicitar tokens de id. para el inicio de sesión de usuario.][n] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA                                                           |
| Nodo               | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)  | [Guía de inicio rápido](quickstart-v2-nodejs-console.md)  | ![La biblioteca no puede solicitar tokens de id. para el inicio de sesión de usuario.][n] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA  |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [msal-python](https://github.com/AzureAD/microsoft-authentication-library-for-python)  | —  | ![La biblioteca no puede solicitar tokens de id. para el inicio de sesión de usuario.][n] | ![La biblioteca puede solicitar tokens de acceso para las API web protegidas.][y] | GA |
<!--
|PHP| [The PHP League oauth2-client](https://oauth2-client.thephpleague.com/usage/) | [League\OAuth2](https://oauth2-client.thephpleague.com/) | ![Green check mark.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Los términos de uso complementarios para las versiones preliminares de Microsoft Azure][preview-tos] se aplican a las bibliotecas en la *versión preliminar pública*.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la biblioteca de autenticación de Microsoft, consulte [Introducción a la Biblioteca de autenticación de Microsoft (MSAL)](msal-overview.md).

<!--Image references-->
[y]: ./media/common/yes.png
[n]: ./media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
