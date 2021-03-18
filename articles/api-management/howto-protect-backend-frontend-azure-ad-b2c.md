---
title: Protección del back-end de una aplicación de página única en Azure API Management con Active Directory B2C
description: Proteja una API con OAuth 2.0 mediante Azure Active Directory B2C, Azure API Management e Easy Auth para llamarla desde una aplicación de página única (SPA) de JavaScript mediante el flujo de autenticación de SPA habilitado para PKCE.
services: api-management, azure-ad-b2c, app-service
documentationcenter: ''
author: WillEastbury
manager: alberts
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2021
ms.author: wieastbu
ms.custom: fasttrack-new, fasttrack-update, devx-track-js
ms.openlocfilehash: 812b54d10ea3cc3c405f534e36ac66abf3466808
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449295"
---
# <a name="protect-spa-backend-with-oauth-20-azure-active-directory-b2c-and-azure-api-management"></a>Protección del back-end de SPA con OAuth 2.0, Azure Active Directory B2C y Azure API Management

En este escenario se muestra cómo configurar la instancia de Azure API Management para proteger una API.
Usaremos el flujo de SPA de Azure AD B2C (código de autenticación + PKCE) para adquirir un token, junto con API Management para proteger un back-end de Azure Functions con EasyAuth.

## <a name="aims"></a>Objetivos

Vamos a ver cómo se puede usar API Management en un escenario simplificado con Azure Functions y Azure AD B2C. Creará una aplicación JavaScript (JS) que llama a una API que inicia la sesión de los usuarios con Azure AD B2C. A continuación, usará las características de directiva de validación de JWT, CORS y limitar la tasa de llamadas por clave de API Management para proteger la API de back-end.

Para la defensa en profundidad, usamos EasyAuth para validar el token de nuevo dentro de la API de back-end y garantizar que API Management es el único servicio que puede llamar al back-end de Azure Functions.

## <a name="what-will-you-learn"></a>Qué aprenderá

> [!div class="checklist"]
> * Configuración de una aplicación de página única y una API de back-end en Azure Active Directory B2C
> * Creación de una API de back-end Azure Functions
> * Importación de una API de Azure Functions en Azure API Management
> * Protección de la API en Azure API Management
> * Llamada a los puntos de conexión de autorización de Azure Active Directory B2C a través de las bibliotecas de la plataforma de identidad de Microsoft (MSAL.js)
> * Almacenamiento de una aplicación de página única de HTML/Vanilla JS y su servicio desde un punto de conexión de Azure Blob Storage

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos de este artículo, debe tener:

* Una cuenta de almacenamiento de Azure (StorageV2) de uso general V2 para hospedar la aplicación de página única JS de front-end.
* Una instancia de Azure API Management (cualquier nivel funcionará, incluido "Consumo"; sin embargo, algunas características aplicables al escenario completo no están disponibles en este nivel [Limitar la tasa de llamadas por clave y dirección IP virtual dedicada]; estas restricciones se indican a continuación en el artículo en las secciones correspondientes).
* Una aplicación de Azure Functions vacía (que ejecute el entorno de ejecución de .NET Core V3.1 en un plan de consumo) para hospedar la API llamada.
* Un inquilino de Azure AD B2C vinculado a una suscripción.

Aunque en la práctica usaría recursos en la misma región en cargas de trabajo de producción, para este artículo de procedimientos, la región de implementación no importa.

## <a name="overview"></a>Información general

A continuación se muestra una ilustración de los componentes en uso y el flujo entre ellos una vez completado este proceso.
![Componentes en uso y flujo](../api-management/media/howto-protect-backend-frontend-azure-ad-b2c/image-arch.png "Componentes en uso y flujo")

Aquí se muestra una rápida visión general de los pasos:

1. Cree las aplicaciones de llamada de Azure AD B2C (front-end, API Management) y de API con ámbitos, y concédales acceso de API
1. Cree las directivas de registro e inicio de sesión para permitir que los usuarios inicien sesión con Azure AD B2C. 
1. Configure API Management con los nuevos identificadores y claves de cliente de Azure AD B2C para habilitar la autorización de usuario de OAuth2 en la consola del desarrollador
1. Compile Function API
1. Configure Function API para habilitar Easy Auth con el nuevo identificador y las claves de cliente de Azure AD B2C y realice el bloqueo para la IP virtual de APIM
1. Compile la definición de API en API Management
1. Configure Oauth2 para la configuración de la API de API Management
1. Configure la directiva **CORS** y agregue la **validate-jwt** para validar el token de OAuth para todas las solicitudes entrantes
1. Compile la aplicación que realiza la llamada para que consuma la API
1. Cargue el ejemplo de aplicación JS de página única
1. Configure aplicación JS cliente de ejemplo con las nuevas claves y el nuevo identificador de cliente de Azure AD B2C 
1. Pruebe la aplicación cliente

   > [!TIP]
   > Vamos a capturar bastantes piezas de información y claves, etc. a medida que avanzamos en este documento; puede que le resulte útil tener un editor de texto abierto para almacenar los siguientes elementos de configuración temporalmente.  
   >
   > ID. DE CLIENTE DE BACK-END DE B2C:  
   > CLAVE DE SECRETO DE CLIENTE DE BACK-END DE B2C:  
   > URI DE ÁMBITO DE API DE BACK-END DE B2C:  
   > ID. DE CLIENTE DE FRONT-END DE B2C:  
   > URI DE PUNTO DE CONEXIÓN DE FLUJO DE USUARIO DE B2C:  
   > PUNTO DE CONEXIÓN DE OPENID CONOCIDO DE B2C:   
   > NOMBRE DE LA DIRECTIVA DE B2C: Frontendapp_signupandsignin DIRECCIÓN URL DE FUNCIÓN:  
   > DIRECCIÓN URL BASE DE APIM API: URL DEL PUNTO DE CONEXIÓN PRINCIPAL DE ALMACENAMIENTO:  

## <a name="configure-the-backend-application"></a>Configuración de la aplicación de back-end

Abra la hoja Azure AD B2C en el portal y realice los pasos siguientes.

1. Seleccione la pestaña **Registros de aplicaciones**.
1. Haga clic en el botón "Nuevo registro".
1. Elija "Web" en el cuadro de selección URI de redireccionamiento.
1. Ahora establezca el Nombre para mostrar, que sea único y pertinente para el servicio que se va a crear. En este ejemplo, usaremos el nombre "Aplicación de back-end".
1. Use marcadores de posición para las direcciones URL de respuesta, como "https://jwt.ms" (un sitio de descodificación de tokens propiedad de Microsoft), actualizaremos estas direcciones URL más adelante.
1. Asegúrese de que ha seleccionado la opción "Cuentas en cualquier proveedor de identidades o directorio de la organización (para autenticar usuarios con flujos de usuario)".
1. En este ejemplo, desactive la casilla "Concesión de consentimiento del administrador", ya que hoy no se requerirán permisos offline_access.
1. Haga clic en "Registrar".
1. Registre el identificador de cliente de la aplicación de back-end para su uso posterior, que aparece en "Id. de aplicación (cliente)".
1. Seleccione la pestaña *Certificados y secretos* (bajo Administrar) y haga clic en "Nuevo secreto de cliente" para generar una clave de autenticación (acepte la configuración predeterminada y haga clic en "Agregar").
1. Después de hacer clic en "Agregar", copie la clave (en "valor") en algún lugar seguro para su uso posterior como "secreto de cliente de back-end". Tenga en cuenta que este cuadro de diálogo es la ÚNICA posibilidad que tendrá para copiar esta clave.
1. Ahora, seleccione la pestaña *Exponer una API* (en Administrar).
1. Se le pedirá que establezca el URI de AppID, seleccione y registre el valor predeterminado.
1. Cree el ámbito de la API de función y póngale el nombre "Hello". Puede usar la frase "Hello" para todas las opciones en las que puede escribir un valor. Luego, registre el URI del valor de ámbito completo rellenado y haga clic en "Agregar ámbito".
1. Vuelva a la raíz de la hoja Azure AD B2C seleccionando la ruta de navegación "Azure AD B2C" en la parte superior izquierda del portal.

   > [!NOTE]
   > Los ámbitos de Azure AD B2C son permisos dentro de la API a los que otras aplicaciones pueden solicitar acceso mediante la hoja de acceso a API de sus aplicaciones.

## <a name="configure-the-frontend-application"></a>Configuración de la aplicación de front-end

1. Seleccione la pestaña **Registros de aplicaciones**.
1. Haga clic en el botón "Nuevo registro".
1. Elija "Aplicación de página única (SPA)" en el cuadro de selección del URI de redireccionamiento.
1. Ahora establezca el nombre para mostrar y el URI de AppID, elija algo único y pertinente para la aplicación de front-end que usará este registro de la aplicación AAD B2C. En este ejemplo, puede usar "Aplicación de front-end".
1. Al igual que en el primer registro de la aplicación, deje la selección de los tipos de cuenta admitidos en su valor predeterminado (autenticación de usuarios con flujos de usuarios).
1. Use marcadores de posición para las direcciones URL de respuesta, como "https://jwt.ms" (un sitio de descodificación de tokens propiedad de Microsoft), actualizaremos estas direcciones URL más adelante.
1. Deje marcada la casilla Conceder consentimiento de administrador.
1. Haga clic en "Registrar".
1. Registre el identificador de cliente de la aplicación de front-end para su uso posterior, que aparece en "Id. de aplicación (cliente)".
1. Cambie a la pestaña *Permisos de API*.
1. Para conceder acceso a la aplicación de back-end, haga clic en "Agregar un permiso", luego en "Mis API", seleccione "Aplicación de back-end", seleccione "Permisos", seleccione el ámbito que creó en la sección anterior y haga clic en "Agregar permisos".
1. Haga clic en "Conceder consentimiento de administrador para {inquilino}" y haga clic en" Sí "en el cuadro de diálogo emergente. Este elemento emergente da su consentimiento a la "Aplicación de front-end" para usar el permiso "Hello" definido en la "Aplicación de back-end" que se creó anteriormente.
1. Todos los permisos deben mostrarse ahora para la aplicación como una marca de verificación verde en la columna de estado.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Creación de un flujo de usuario de registro e inicio de sesión

1. Vuelva a la raíz de la hoja de B2C seleccionando la ruta de navegación de Azure AD B2C.
1. Cambie a la pestaña "Flujos de usuario" (en Directivas).
1. Haga clic en "Nuevo flujo de usuario".
1. Elija el tipo de flujo de usuario "Registrarse e iniciar sesión" y seleccione "Recomendado". Luego, haga clic en "Crear".
1. Asígnele un nombre a la directiva y regístrelo para más adelante. En este ejemplo, puede usar "Frontendapp_signupandsignin", tenga en cuenta que se le antepondrá "B2C_1_" creando "B2C_1_Frontendapp_signupandsignin".
1. En "Proveedores de identidades" y "Cuentas locales", active "Registro con correo electrónico" (o "Registro con Id. de usuario" en función de la configuración del inquilino de B2C) y haga clic en Aceptar. Esta configuración se debe a que vamos a registrar cuentas locales B2C, y no remitirnos a otro proveedor de identidades (como un proveedor de identidades sociales) para utilizar la cuenta de medios sociales existente de un usuario.
1. Deje la configuración de MFA y de acceso condicional en sus valores predeterminados.
1. En "User Attributes and claims" (Notificaciones y atributos de usuario), haga clic en "Mostrar más...", elija las opciones de notificaciones que desee que los usuarios escriban y reciban de vuelta en el token. Active al menos "Nombre para mostrar" y "Dirección de correo electrónico" para recopilar, con "Nombre para mostrar" y "Direcciones de correo electrónico" para devolver (preste mucha atención al hecho de que está recopilando dirección de correo electrónico, en singular, y pidiendo que se devuelvan direcciones de correo electrónico, en plural), y haga clic en "Aceptar", y luego en "Crear".
1. Haga clic en el flujo de usuario que creó en la lista y luego en el botón "Ejecutar flujo de usuario".
1. Esta acción abrirá la hoja para ejecutar el flujo de usuario, seleccione la aplicación de front-end, copie el punto de conexión del flujo de usuario y guárdelo para más adelante.
1. Copie y guarde el vínculo de la parte superior, registrándolo como "punto de conexión de configuración de OpenID conocido" para su uso posterior.

   > [!NOTE]
   > Las directivas de B2C permiten exponer los puntos de conexión de inicio de sesión de Azure AD B2C para poder capturar diferentes componentes de datos e iniciar la sesión de los usuarios de diferentes maneras.
   > 
   > En este caso, se ha configurado un flujo de inicio de registro o de inicio de sesión (directiva). Esto también expuso un punto conexión de configuración conocido, en ambos casos nuestra directiva creada se identificó en la dirección URL mediante el parámetro de cadena de consulta "p=".  
   >
   > Una vez hecho esto tendrá una plataforma de identidad de negocio a consumidor (B2C) funcional para que los usuarios inicien sesión en varias aplicaciones.

## <a name="build-the-function-api"></a>Compilación de la API de función

1. Vuelva a cambiar al inquilino de Azure AD estándar en Azure Portal para poder configurar de nuevo los elementos de la suscripción.
1. Vaya a la hoja Aplicaciones de funciones de Azure Portal, abra la aplicación de función vacía y haga clic en "Functions". Luego, haga clic en "Agregar".
1. En el desplegable que aparece, elija "Desarrollar en el portal", en "Seleccionar una plantilla", elija "Desencadenador HTTP" y, en Detalles de la plantilla, póngale el nombre "hello" con el nivel de autorización "Función". Luego, seleccione Agregar.
1. Cambie a la hoja Código y prueba y copie y pegue el código de ejemplo siguiente *sobre el código existente* que aparece.
1. Seleccione Guardar.

   ```csharp
   
   using System.Net;
   using Microsoft.AspNetCore.Mvc;
   using Microsoft.Extensions.Primitives;
   
   public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
   {
      log.LogInformation("C# HTTP trigger function processed a request.");
      
      return (ActionResult)new OkObjectResult($"Hello World, time and date are {DateTime.Now.ToString()}");
   }
   
   ```

   > [!TIP]
   > El código de la función de script de C# que acaba de pegar simplemente registra una línea en los registros de función y devuelve el texto "Hola mundo" con algunos datos dinámicos (fecha y hora).

1. Seleccione "Integración" en la hoja de la izquierda y, a continuación, haga clic en el vínculo HTTP (solicitud) dentro del cuadro "Desencadenador".
1. En la lista desplegable "Métodos HTTP seleccionados", desactive el método HTTP POST, dejando solo seleccionado GET, y haga clic en Guardar.
1. Vuelva a la pestaña Código y prueba, haga clic en "Obtener la dirección URL de la función" y, a continuación, copie la dirección URL que aparece y guárdela para más adelante.

   > [!NOTE]
   > Los enlaces que acaba de crear simplemente indican a las funciones que respondan a solicitudes HTTP GET anónimas a la dirección URL que acaba de copiar (`https://yourfunctionappname.azurewebsites.net/api/hello?code=secretkey`). Ahora tenemos una API HTTPS sin servidor escalable capaz de devolver una carga muy sencilla.
   >
   > Ahora puede probar a llamar a esta API desde un explorador web utilizando su versión de la dirección URL anterior que acaba de copiar y guardar. También puede quitar la parte "?code=secretkey" de los parámetros de la cadena de consulta de la dirección URL y probar de nuevo para comprobar que Azure Functions devolverá un error 401.

## <a name="configure-and-secure-the-function-api"></a>Configuración y protección de la API de función

1. Es necesario configurar dos áreas adicionales en la aplicación de funciones (restricciones de autenticación y de red).
1. En primer lugar, vamos a configurar la autenticación y la autorización, así que vuelva a la hoja raíz de la aplicación de funciones a través de la ruta de navegación.
1. A continuación, seleccione "Autenticación/autorización" (en "Configuración").
1. Active la característica de autenticación de App Service.
1. Establezca la acción que desea realizar cuando la solicitud no se autentique en el cuadro desplegable "Iniciar sesión con Azure Active Directory".
1. En "Proveedores de autenticación", elija "Azure Active Directory".
1. Elija "Opciones avanzadas" en el conmutador Modo de administración.
1. Pegue el identificador de cliente de la aplicación de back-end [Aplicación] (de Azure AD B2C) en el cuadro "Id. de cliente".
1. Pegue el punto de conexión de configuración OpenID conocido de la directiva de registro y inicio de sesión en el cuadro URL del emisor (hemos registrado esta configuración anteriormente).
1. Haga clic en "Mostrar secreto" y pegue el secreto de cliente de la aplicación de back-end en el cuadro correspondiente.
1. Seleccione Aceptar para volver a la pantalla o la hoja de selección del proveedor de identidades.
1. Deje el [Almacén de tokens](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization#token-store) habilitado en la configuración avanzada (valor predeterminado).
1. Haga clic en "Guardar" (en la parte superior de la hoja).

   > [!IMPORTANT]
   > Ahora, la API de Function está implementada, por lo que debe generar respuestas 401 si no se proporciona el JWT correcto como encabezado Autorización: Portador y devolver datos cuando se presente una solicitud correcta.  
   > Ha agregado una medida de seguridad de defensa en profundidad adicional en EasyAuth configurando la opción "Login With Azure AD" (Inicio de sesión con Azure AD) para administrar las solicitudes no autenticadas. Tenga en cuenta que esto cambiará el comportamiento de las solicitudes no autorizadas entre la aplicación BackEnd Function y la SPA Frontend, ya que EasyAuth emitirá un redireccionamiento 302 a AAD en lugar de una respuesta 401 no autorizada. Corregiremos este comportamiento más adelante con API Management.  
   >
   > Sin embargo, aún no hemos aplicado la seguridad IP. Si tiene una clave válida y un token de OAuth2, cualquiera puede realizar la llamada desde cualquier lugar (lo ideal sería exigir que todas las solicitudes lleguen a través de API Management).  
   > 
   > Si usa el nivel de consumo de APIM, [no hay una dirección IP virtual de Azure API Management dedicada](./api-management-howto-ip-addresses.md#ip-addresses-of-consumption-tier-api-management-service) para la lista de permitidos con las funciones de restricción de acceso. En la SKU Estándar de Azure API Management Standard y superiores, [la IP virtual es de un solo inquilino y para toda la vigencia del recurso](./api-management-howto-ip-addresses.md#changes-to-the-ip-addresses). En el nivel Consumo de Azure API Management, puede bloquear las llamadas API a través de la clave de función secreta compartida en la parte del URI que copió anteriormente. Además, para el nivel Consumo, los pasos 12-17 siguientes no se aplican.

1. Cierre la hoja "Autenticación/Autorización". 
1. Abra la *hoja API Management del portal* y, luego, abra *su instancia*.
1. Registre la IP virtual privada que aparece en la pestaña Información general.
1. Vuelva a la *hoja Azure Functions del portal* y, a continuación, vuelva a abrir *su instancia*.
1. Seleccione "Redes" y luego "Configuración de las restricciones de acceso".
1. Haga clic en "Agregar regla" y escriba la IP virtual copiada en el paso 3 anterior con el formato xx.xx.xx.xx/32.
1. Si desea seguir interactuando con el portal de funciones y llevar a cabo los pasos opcionales que se indican a continuación, primero debe agregar su propia dirección IP pública o intervalo CIDR.
1. Cuando haya una entrada de permiso en la lista, Azure agrega una regla de denegación implícita para bloquear todas las demás direcciones.

Deberá agregar bloques de direcciones con formato CIDR al panel restricciones de IP. Cuando necesite agregar una sola dirección, como la IP virtual de API Management, debe hacerlo en formato xx.xx.xx.xx/32.

   > [!NOTE]
   > Ahora la API de función no debería estar accesible por llamada desde otro origen distinto de API Management o su dirección.

1. Abra la hoja *API Management* y después *la instancia*.
1. Seleccione la hoja API (en API).
1. En el panel "Add a New API" (Agregar una nueva API), elija "Function App" y seleccione "Completo" en la parte superior de la ventana emergente.
1. Haga clic en Examinar, elija la aplicación de funciones que hospede la API y haga clic en Seleccionar. A continuación, haga clic en Seleccionar de nuevo.
1. Proporciónele un nombre a la API y una descripción para uso interno de API Management y agréguelos al producto "Unlimited" (Ilimitado).
1. Copie y registre la "URL base" de la API y haga clic en "Crear".
1. Haga clic en la pestaña "Configuración" y, a continuación, en Suscripción. Desactive la casilla "Suscripción necesaria", ya que usaremos el token de JWT de OAuth en este caso para limitar la frecuencia. Tenga en cuenta que, si usa el nivel Consumo, esto seguirá siendo necesario en un entorno de producción. 

   > [!TIP]
   > Si usa el nivel Consumo de APIM, el producto ilimitado no estará disponible de forma inmediata. Tendrá que ir a "Productos" en "API" y presionar "Agregar".  
   > Escriba "Ilimitado" como nombre y descripción del producto y seleccione la API que acaba de agregar en la llamada "+" de API en la parte inferior izquierda de la pantalla. Seleccione la pestaña "Publicadas". Deje el resto tal como está. Por último, pulse el botón "Crear". Esto creó el producto "ilimitado" y lo asignó a su API. Puede personalizar el nuevo producto más adelante.

## <a name="configure-and-capture-the-correct-storage-endpoint-settings"></a>Configuración y captura de la configuración correcta del punto de conexión de almacenamiento

1. Abra la hoja Cuentas de almacenamiento en Azure Portal. 
1. Seleccione la cuenta que ha creado y la hoja "Sitio web estático" en la sección Configuración (si no ve la opción "Sitio web estático", compruebe que creó una cuenta V2).
1. Establezca la característica de hospedaje web estático en "Habilitado" y dele a documento de índice el nombre "index.html". Después, haga clic en "Guardar".
1. Anote el contenido del punto de conexión principal para más adelante, ya que esta ubicación es donde se hospedará el sitio de front-end.

   > [!TIP]
   > Puede usar Azure Blob Storage + reescritura de la red CDN, o Azure App Service para hospedar la aplicación de página única, pero la característica de hospedaje de sitios web estáticos de Blob Storage proporciona un contenedor predeterminado que sirve contenido/html/JS/css web estático de Azure Storage e infiere una página predeterminada para que no tengamos que hacer nada.

## <a name="set-up-the-cors-and-validate-jwt-policies"></a>Configuración de las directivas **CORS** y **validate-jwt**

> Se deben seguir las siguientes secciones independientemente del nivel de APIM que se use. La dirección URL de la cuenta de almacenamiento es de la cuenta de almacenamiento que estará disponible a raíz de los requisitos previos de la parte superior de este artículo.
1. Cambie a la hoja API Management del portal y abra su instancia.
1. Seleccione API y, a continuación, "Todas las API".
1. En "Procesamiento de entrada", haga clic en el botón de vista Código "</>" para mostrar el editor de directivas.
1. Edite la sección de entrada y pegue el código XML siguiente para que sea similar al siguiente.
1. Sustituya los siguientes parámetros en la directiva.
1. {PrimaryStorageEndpoint} (El punto de conexión de almacenamiento principal que copió en la sección anterior), {b2cpolicy-Well-Known-OpenID} (el "punto de conexión de configuración de OpenID conocido" que copió anteriormente) y {backend-API-Application-Client-ID} (el identificador de cliente/aplicación B2C para la **API de back-end**) con los valores correctos guardados anteriormente.
1. Si usa el nivel Consumo de API Management, debe quitar ambas directivas de limitar la tasa de llamadas por clave, ya que esta directiva no está disponible cuando se usa el nivel Consumo de Azure API Management.

   ```xml
   <inbound>
      <cors allow-credentials="true">
            <allowed-origins>
                <origin>{PrimaryStorageEndpoint}</origin>
            </allowed-origins>
            <allowed-methods preflight-result-max-age="120">
                <method>GET</method>
            </allowed-methods>
            <allowed-headers>
                <header>*</header>
            </allowed-headers>
            <expose-headers>
                <header>*</header>
            </expose-headers>
        </cors>
      <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid." require-expiration-time="true" require-signed-tokens="true" clock-skew="300">
         <openid-config url="{b2cpolicy-well-known-openid}" />
         <required-claims>
            <claim name="aud">
               <value>{backend-api-application-client-id}</value>
            </claim>
         </required-claims>
      </validate-jwt>
      <rate-limit-by-key calls="300" renewal-period="120" counter-key="@(context.Request.IpAddress)" />
      <rate-limit-by-key calls="15" renewal-period="60" counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
   </inbound>
   ```

   > [!NOTE]
   > Ahora, Azure API Management podrá responder a las solicitudes de origen cruzado desde las aplicaciones de página única de JavaScript y realizará la limitación, la limitación de velocidad y la validación previa del token de autenticación JWT que se pasa ANTES de reenviar la solicitud a la API de función.
   > 
   > Enhorabuena, ahora tiene Azure AD B2C, API Management y Azure Functions trabajando juntos para publicar, proteger y consumir una API.

   > [!TIP]
   > Si usa el nivel Consumo de API Management, en lugar de la limitación de velocidad por el sujeto de JWT o la dirección IP entrante (actualmente no se admite la directiva de limitar la tasa de llamadas por clave para el nivel "Consumo"), puede limitar por la cuota de tasa de llamadas, (consulte [aquí](./api-management-access-restriction-policies.md#LimitCallRate)).  
   > Como este ejemplo es una aplicación JavaScript de página única, la clave de API Management solo se usa para las llamadas de facturación y de limitación de velocidad. La autenticación y la autorización reales se administran mediante Azure AD B2C y se encapsulan en JWT, que se valida dos veces, una desde API Management y, a continuación, desde la función de Azure de backend.

## <a name="upload-the-javascript-spa-sample-to-static-storage"></a>Carga del ejemplo de aplicación de página única de JavaScript en el almacenamiento estático

1. Todavía en la hoja de la cuenta de almacenamiento, seleccione la hoja "Contenedores" en la sección Blob service y haga clic en el contenedor $web que aparece en el panel derecho.
1. Guarde el código siguiente en un archivo de la máquina como index.html y cargue este archivo en el contenedor $web.

   ```html
    <!doctype html>
    <html lang="en">
    <head>
         <meta charset="utf-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1">
         <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
         <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.11.1/js/msal-browser.min.js"></script>
    </head>
    <body>
         <div class="container-fluid">
             <div class="row">
                 <div class="col-md-12">
                    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
                        <div class="container-fluid">
                            <a class="navbar-brand" href="#">Azure Active Directory B2C with Azure API Management</a>
                            <div class="navbar-nav">
                                <button class="btn btn-success" id="signinbtn"  onClick="login()">Sign In</a>
                            </div>
                        </div>
                    </nav>
                 </div>
             </div>
             <div class="row">
                 <div class="col-md-12">
                     <div class="card" >
                        <div id="cardheader" class="card-header">
                            <div class="card-text"id="message">Please sign in to continue</div>
                        </div>
                        <div class="card-body">
                            <button class="btn btn-warning" id="callapibtn" onClick="getAPIData()">Call API</a>
                            <div id="progress" class="spinner-border" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                     </div>
                 </div>
             </div>
         </div>
         <script lang="javascript">
                // Just change the values in this config object ONLY.
                var config = {
                    msal: {
                        auth: {
                            clientId: "{CLIENTID}", // This is the client ID of your FRONTEND application that you registered with the SPA type in AAD B2C
                            authority:  "{YOURAUTHORITYB2C}", // Formatted as https://{b2ctenantname}.b2clogin.com/tfp/{b2ctenantguid or full tenant name including onmicrosoft.com}/{signuporinpolicyname}
                            redirectUri: "{StoragePrimaryEndpoint}", // The storage hosting address of the SPA, a web-enabled v2 storage account - recorded earlier as the Primary Endpoint.
                            knownAuthorities: ["{B2CTENANTDOMAIN}"] // {b2ctenantname}.b2clogin.com
                        },
                        cache: {
                            cacheLocation: "sessionStorage",
                            storeAuthStateInCookie: false 
                        }
                    },
                    api: {
                        scopes: ["{BACKENDAPISCOPE}"], // The scope that we request for the API from B2C, this should be the backend API scope, with the full URI.
                        backend: "{APIBASEURL}/hello" // The location that we will call for the backend api, this should be hosted in API Management, suffixed with the name of the API operation (in the sample this is '/hello').
                    }
                }
                document.getElementById("callapibtn").hidden = true;
                document.getElementById("progress").hidden = true;
                const myMSALObj = new msal.PublicClientApplication(config.msal);
                myMSALObj.handleRedirectPromise().then((tokenResponse) => {
                    if(tokenResponse !== null){
                        console.log(tokenResponse.account);
                        document.getElementById("message").innerHTML = "Welcome, " + tokenResponse.account.name;
                        document.getElementById("signinbtn").hidden = true;
                        document.getElementById("callapibtn").hidden = false;
                    }}).catch((error) => {console.log("Error Signing in:" + error);
                });
                function login() {
                    try {
                        myMSALObj.loginRedirect({scopes: config.api.scopes});
                    } catch (err) {console.log(err);}
                }
                function getAPIData() {
                    document.getElementById("progress").hidden = false; 
                    document.getElementById("message").innerHTML = "Calling backend ... "
                    document.getElementById("cardheader").classList.remove('bg-success','bg-warning','bg-danger');
                    myMSALObj.acquireTokenSilent({scopes: config.api.scopes, account: getAccount()}).then(tokenResponse => {
                        const headers = new Headers();
                        headers.append("Authorization", `Bearer ${tokenResponse.accessToken}`);
                        fetch(config.api.backend, {method: "GET", headers: headers})
                            .then(async (response)  => {
                                if (!response.ok)
                                {
                                    document.getElementById("message").innerHTML = "Error: " + response.status + " " + JSON.parse(await response.text()).message;
                                    document.getElementById("cardheader").classList.add('bg-warning');
                                }
                                else
                                {
                                    document.getElementById("cardheader").classList.add('bg-success');
                                    document.getElementById("message").innerHTML = await response.text();
                                }
                                }).catch(async (error) => {
                                    document.getElementById("cardheader").classList.add('bg-danger');
                                    document.getElementById("message").innerHTML = "Error: " + error;
                                });
                    }).catch(error => {console.log("Error Acquiring Token Silently: " + error);
                        return myMSALObj.acquireTokenRedirect({scopes: config.api.scopes, forceRefresh: false})
                    });
                    document.getElementById("progress").hidden = true;
             }
            function getAccount() {
                var accounts = myMSALObj.getAllAccounts();
                if (!accounts || accounts.length === 0) {
                    return null;
                } else {
                    return accounts[0];
                }
            }
        </script>
     </body>
    </html>
   ```

1. Vaya al punto de conexión principal del sitio web estático que almacenó en la última sección.

   > [!NOTE]
   > Enhorabuena, acaba de implementar una aplicación de página única de JavaScript para el hospedaje de contenido estático de Azure Storage.  
   > Dado que no hemos configurado la aplicación JS con los detalles de Azure AD B2C aún, la página no funcionará todavía si la abre.

## <a name="configure-the-javascript-spa-for-azure-ad-b2c"></a>Configuración de la aplicación de página única de JavaScript SPA para Azure AD B2C

1. Ahora sabemos dónde está todo: podemos configurar la aplicación de página única con la dirección de API de API Management adecuada y los identificadores de aplicación y de cliente de Azure AD B2C correctos.
1. Vuelva a la hoja de almacenamiento de Azure Portal. 
1. Seleccione "Contenedores" (en "Configuración"). 
1. Seleccione el contenedor "$web" en la lista.
1. Seleccione el blob index.html en la lista. 
1. Haga clic en "Editar". 
1. Actualice los valores de autenticación en la sección de configuración de msal para que coincida con la aplicación de *front-end* que registró anteriormente en B2C. Use los comentarios de código para ver sugerencias sobre el aspecto que deberían tener los valores de configuración.
El valor de *autoridad* debe tener el formato https://{b2ctenantname}.b2clogin.com/tfp/{b2ctenantname}.onmicrosoft.com}/{signupandsigninpolicyname}.Si ha usado los nombres de ejemplo y el inquilino de B2C se llama "contoso", se espera que la autoridad sea "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com}/Frontendapp_signupandsignin".
1. Establezca los valores de la API para que coincidan con la dirección del back-end (la URL base de API que registró anteriormente, y los valores de "b2cScopes" se registraron anteriormente para la *aplicación de back-end*).
1. Haga clic en Guardar

## <a name="set-the-redirect-uris-for-the-azure-ad-b2c-frontend-app"></a>Establecimiento de los URI de redireccionamiento para la aplicación de front-end de Azure AD B2C

1. Abra la hoja Azure AD B2C y vaya al registro de la aplicación JavaScript de front-end.
1. Haga clic en "URI de redireccionamiento" y elimine el marcador de posición "https://jwt.ms" que se especificó anteriormente.
1. Agregue un nuevo URI para el punto de conexión principal (almacenamiento) (sin la barra diagonal final).

   > [!NOTE]
   > Esta configuración hará que un cliente de la aplicación de front-end reciba un token de acceso con las notificaciones adecuadas de Azure AD B2C.  
   > La aplicación de página única podrá agregarlo como token de portador al encabezado HTTPS en la llamada a la API de back-end.  
   > 
   > API Management validará previamente el token, limitará la frecuencia de las llamadas al punto de conexión por el sujeto del JWT emitido por el identificador de Azure (el usuario) y por la dirección IP del autor de la llamada (en función del nivel de servicio de API Management, consulte la nota anterior) antes de pasar la solicitud a la API de Azure Functions de recepción, agregando la clave de seguridad.  
   > La aplicación de página única representará la respuesta en el explorador.
   >
   > *Enhorabuena, ha configurado Azure AD B2C, Azure API Management, Azure Functions y la autorización de Azure App Service para que trabajen en perfecta armonía.*

Ahora tenemos una aplicación sencilla con una API segura sencilla, vamos a probarla.

## <a name="test-the-client-application"></a>Prueba de la aplicación cliente

1. Abra la dirección URL de la aplicación de ejemplo que anotó de la cuenta de almacenamiento que creó anteriormente.
1. Haga clic en "Iniciar sesión" en la esquina superior derecha, lo que hará que aparezca su perfil de registro o de inicio de sesión de Azure AD B2C emergente.
1. La aplicación debería darte la bienvenida por su nombre de perfil B2C.
1. Ahora haga clic en "Llamar a API" y la página debería actualizarse con los valores devueltos desde la API protegida.
1. Si hace clic *repetidamente* en el botón Llamar a API y ejecuta en el nivel de desarrollador o superior de API Management, debe tener en cuenta que su solución comenzará a limitar la frecuencia de la API y esta característica debe notificarse en la aplicación con un mensaje adecuado.

## <a name="and-were-done"></a>Y hemos terminado

Los pasos anteriores se pueden adaptar y editar para permitir gran cantidad de usos diferentes de Azure AD B2C con API Management.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [Escenarios de autenticación para Azure AD](../active-directory/develop/authentication-vs-authorization.md).
* Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
* Para conocer otras formas de proteger el servicio back-end, consulte [Autenticación de certificado mutua](api-management-howto-mutual-certificates.md).
* [Creación de una instancia del servicio de API Management](get-started-create-service-instance.md).
* [Administración de su primera API en Administración de API de Azure](import-and-publish.md).
