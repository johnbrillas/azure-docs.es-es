---
title: Creación de un punto de conexión de SCIM para el aprovisionamiento de usuarios en aplicaciones desde Azure Active Directory
description: System for Cross-domain Identity Management (SCIM) normaliza el aprovisionamiento automático de usuarios. Aprenderá a desarrollar un punto de conexión SCIM, integrar la API de SCIM con Azure Active Directory y comenzar a automatizar el aprovisionamiento de usuarios y grupos en las aplicaciones en la nube con Azure Active Directory.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 6b7451b0d664995a6b647f7926d856b0db6090d8
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99256109"
---
# <a name="tutorial-develop-a-sample-scim-endpoint"></a>Tutorial: Desarrollo un punto de conexión SCIM de ejemplo

Nadie desea crear un nuevo punto de conexión desde cero, por lo que hemos creado [código de referencia](https://aka.ms/scimreferencecode) para que pueda empezar a trabajar con [SCIM](https://aka.ms/scimoverview). En este tutorial se describe cómo implementar el código de referencia de SCIM en Azure y probarlo con Postman o mediante la integración en el cliente SCIM de Azure AD. Puede poner el punto de conexión de SCIM en funcionamiento sin código en tan solo 5 minutos. Este tutorial está destinado a desarrolladores que buscan empezar a trabajar con SCIM u a otros usuarios interesados en probar un punto de conexión de SICM. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Implementar el punto de conexión de SCIM en Azure
> * Probar el punto de conexión de SCIM

## <a name="deploy-your-scim-endpoint-in-azure"></a>Implementación de un punto de conexión de SCIM en Azure

Los pasos que se proporcionan aquí implementan el punto de conexión de SCIM en un servicio con [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) y [Azure App Service](https://docs.microsoft.com/azure/app-service/). El código de referencia de SCIM también se puede ejecutar localmente, hospedar en un servidor local o implementar en otro servicio externo. 

### <a name="open-solution-and-deploy-to-azure-app-service"></a>Apertura de la solución e implementación en Azure App Service

1. Vaya al [código de referencia](https://github.com/AzureAD/SCIMReferenceCode) de GitHub y seleccione **Clonar o descargar**.

1. Elija **Abrir en el escritorio**, o bien, copie el vínculo, abra **Visual Studio** y seleccione **Clonar o extraer código del repositorio** para escribir el vínculo copiado y realizar una copia local.

1. En **Visual Studio**, asegúrese de iniciar sesión en la cuenta que tiene acceso a los recursos de hospedaje.

1. En **Explorador de soluciones**, abra *Microsoft.SCIM.sln* y haga clic con el botón derecho en el archivo *Microsoft.SCIM.WebHostSample*. Seleccione **Publicar**.

    ![publicación en la nube](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish.png)

    > [!NOTE]
    > Para ejecutar esta solución de forma local, haga doble clic en el proyecto y seleccione **IIS Express** para iniciar el proyecto como página web con una dirección URL de host local.

1. Seleccione **Crear perfil** y asegúrese de que **App Service** y **Crear nuevo** estén seleccionados.

    ![publicación en la nube 2](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish-2.png)

1. Recorra las opciones del cuadro de diálogo y cambie el nombre de la aplicación por un nombre de su elección. Este nombre se usa tanto en la dirección URL de la aplicación como en el punto de conexión de SCIM.

    ![publicación en la nube 3](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish-3.png)

1. Seleccione el grupo de recursos que quiera usar y elija **Publicar**.

1. Vaya a la aplicación en **Azure App Service** > **Configuración** y seleccione **Nueva configuración de la aplicación** para agregar el valor *Token__TokenIssuer* con el valor `https://sts.windows.net/<tenant_id>/`. Reemplace `<tenant_id>` por su tenant_id de Azure AD y, si quiere probar el punto de conexión de SCIM con [Postman](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint), agregue también un valor *ASPNETCORE_ENVIRONMENT* con el valor `Development`. 

   ![configuración de appservice](media/use-scim-to-build-users-and-groups-endpoints/app-service-settings.png)

   Al probar el punto de conexión con una aplicación empresarial en Azure Portal, elija mantener el entorno como `Development`, proporcione el token generado a partir del punto de conexión de `/scim/token` para probar o cambiar el entorno a `Production` y deje el campo de token vacío en la aplicación empresarial en [Azure Portal](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client). 

Eso es todo. El punto de conexión de SCIM ahora está publicado y le permite usar la dirección URL del Azure App Service para probar el punto de conexión de SCIM.

## <a name="test-your-scim-endpoint"></a>Prueba de un punto de conexión de SCIM

Las solicitudes a un punto de conexión de SCIM requieren autorización, y el estándar SCIM deja varias opciones para la autenticación y autorización, como cookies, autenticación básica, autenticación de cliente TLS o cualquiera de los métodos enumerados en [RFC 7644](https://tools.ietf.org/html/rfc7644#section-2).

Asegúrese de evitar métodos inseguros, como nombre de usuario y contraseña, y favorezca un método más seguro, como OAuth. Azure AD admite tokens de portador de larga duración (para aplicaciones de la galería y ajenas a la galería) y la concesión de autorización de OAuth (para aplicaciones publicadas en la galería de aplicaciones).

> [!NOTE]
> Los métodos de autorización proporcionados en el repositorio son solo con fines de prueba. Durante la integración en Azure AD, puede revisar la guía de autorización, consulte [Planificación del aprovisionamiento de un punto de conexión de SCIM](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#authorization-for-provisioning-connectors-in-the-application-gallery). 

El entorno de desarrollo habilita características no seguras para producción, como el código de referencia para controlar el comportamiento de la validación de tokens de seguridad. El código de validación de tokens está configurado para usar un token de seguridad autofirmado, y la clave de firma se almacena en el archivo de configuración, consulte el parámetro **Token:IssuerSigningKey** en el archivo *appsettings.Development.json*.

```json
"Token": {
    "TokenAudience": "Microsoft.Security.Bearer",
    "TokenIssuer": "Microsoft.Security.Bearer",
    "IssuerSigningKey": "A1B2C3D4E5F6A1B2C3D4E5F6",
    "TokenLifetimeInMins": "120"
}
```

> [!NOTE]
> Al enviar una solicitud **GET** al punto de conexión de `/scim/token`, se emite un token con la clave configurada y se puede usar como token de portador para la posterior autorización.

El código de validación de tokens predeterminado está configurado para usar un token emitido por Azure Active Directory y requiere que el inquilino emisor se configure mediante el parámetro **Token:TokenIssuer** en el archivo *appsettings.json*.

``` json
"Token": {
    "TokenAudience": "8adf8e6e-67b2-4cf2-a259-e3dc5476c621",
    "TokenIssuer": "https://sts.windows.net/<tenant_id>/"
}
```

### <a name="use-postman-to-test-endpoints"></a>Uso de Postman para probar los puntos de conexión

Una vez implementado el punto de conexión de SCIM, puede probarlo para asegurarse de que cumpla con RFC SCIM. En este ejemplo se proporciona un conjunto de pruebas en **Postman** para validar operaciones CRUD en usuarios y grupos, filtrar, actualizar la pertenencia a grupos y deshabilitar usuarios.

Los puntos de conexión se encuentran en el directorio `{host}/scim/` y se puede interactuar con ellos mediante solicitudes HTTP estándar. Para modificar la ruta de `/scim/`, consulte *ControllerConstant.cs* en **AzureADProvisioningSCIMreference** > **ScimReferenceApi** > **Controllers**.

> [!NOTE]
> Solo puede usar puntos de conexión HTTP para las pruebas locales, ya que el servicio de aprovisionamiento de Azure AD requiere que el punto de conexión admita HTTPS.

#### <a name="open-postman-and-run-tests"></a>Apertura de Postman y ejecución de pruebas

1. Descargue [Postman](https://www.getpostman.com/downloads/) e inicie la aplicación.
1. Copie el vínculo [https://aka.ms/ProvisioningPostman](https://aka.ms/ProvisioningPostman) y péguelo en Postman para importar la colección de pruebas.

    ![colección de postman](media/use-scim-to-build-users-and-groups-endpoints/postman-collection.png)

1. Cree un entorno de prueba con las variables siguientes:

   |Entorno|Variable|Value|
   |-|-|-|
   |Ejecución del proyecto de manera local con IIS Express|||
   ||**Servidor**|`localhost`|
   ||**Puerto**|`:44359` *(no olvide los **:** )*|
   ||**Api**|`scim`|
   |Ejecución del proyecto de manera local con Kestrel|||
   ||**Servidor**|`localhost`|
   ||**Puerto**|`:5001` *(no olvide los **:** )*|
   ||**Api**|`scim`|
   |Hospedaje del punto de conexión en Azure|||
   ||**Servidor**|*(escriba la dirección URL de SCIM)*|
   ||**Puerto**|*(deje en blanco)*|
   ||**Api**|`scim`|

1. Use **Get Key** (Obtener clave) de la colección de Postman para enviar una solicitud **GET** al punto de conexión del token y recuperar un token de seguridad que se almacenará en la variable **token** para las solicitudes posteriores. 

   ![get key de postman](media/use-scim-to-build-users-and-groups-endpoints/postman-get-key.png)

   > [!NOTE]
   > Para que los puntos de conexión de SCIM sean seguros, necesita un token de seguridad antes de conectarse. En el tutorial se usa el punto de conexión `{host}/scim/token` para generar un token autofirmado.

Eso es todo. Ahora puede ejecutar la colección de **Postman** para probar la funcionalidad del punto de conexión de SCIM.

## <a name="next-steps"></a>Pasos siguientes

Para desarrollar un punto de conexión de usuario y grupo compatible con SCIM con la interoperabilidad para un cliente, consulte la [implementación de cliente de SCIM](http://www.simplecloud.info/#Implementations2).

> [!div class="nextstepaction"]
> [Tutorial: Desarrollo y planeación del aprovisionamiento de un punto de conexión de SCIM](use-scim-to-provision-users-and-groups.md)
> [Tutorial: Configuración del aprovisionamiento en una aplicación de la galería](configure-automatic-user-provisioning-portal.md)