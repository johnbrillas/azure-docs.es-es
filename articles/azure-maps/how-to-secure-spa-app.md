---
title: Protección de una aplicación de página única con inicio de sesión no interactivo
titleSuffix: Azure Maps
description: Configuración de una aplicación de página única con el control de acceso basado en roles de Azure no interactivo (Azure RBAC) y el SDK web de Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/12/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: devx-track-js
ms.openlocfilehash: 9d2af0bf731ab069a8512cb10feccf5ba18d3fa0
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101092727"
---
# <a name="how-to-secure-a-single-page-application-with-non-interactive-sign-in"></a>Protección de una aplicación de página única con inicio de sesión no interactivo

La guía siguiente pertenece a una aplicación que usa Azure Active Directory (Azure AD) para proporcionar un token de acceso a aplicaciones de Azure Maps cuando el usuario no puede iniciar sesión en Azure AD. Este flujo requiere el hospedaje de un servicio web que debe protegerse para que solo la aplicación web de página única pueda acceder a él. Hay varias implementaciones que pueden realizar la autenticación en Azure AD. En esta guía se utiliza Azure Functions para adquirir tokens de acceso.

[!INCLUDE [authentication details](./includes/view-authentication-details.md)]

> [!Tip]
> Azure Maps puede admitir tokens de acceso de flujos interactivos o de inicio de sesión de usuario. Los flujos interactivos permiten un ámbito más restringido de administración de secretos y revocación de acceso.

## <a name="create-azure-function"></a>Creación de una función de Azure

Cree una aplicación de servicio web segura que sea responsable de la autenticación en Azure AD. 

1. Cree una función en Azure Portal. Para más información, consulte [Creación de una función de Azure](../azure-functions/functions-get-started.md).

2. Configure la directiva CORS en la función de Azure para que la aplicación web de página única pueda acceder a ella. Esto protegerá a los clientes de explorador en los orígenes permitidos de la aplicación web. Consulte [Adición de funcionalidad CORS](../app-service/app-service-web-tutorial-rest-api.md#add-cors-functionality).

3. [Agregue una identidad asignada por el sistema](../app-service/overview-managed-identity.md?tabs=dotnet#add-a-system-assigned-identity) en la función de Azure para habilitar la creación de una entidad de servicio para autenticarse en Azure AD.  

4. Conceda a la cuenta de Azure Maps el acceso basado en roles para la identidad asignada por el sistema. Consulte [Concesión de acceso basado en roles](#grant-role-based-access) para más información.

5. Escriba código para que la función de Azure obtenga tokens de acceso de Azure Maps mediante la identidad asignada por el sistema con uno de los mecanismos admitidos o con el protocolo de REST. Consulte [Obtención de tokens para recursos de Azure](../app-service/overview-managed-identity.md?tabs=dotnet#add-a-system-assigned-identity).

    Ejemplo de protocolo de REST:

    ```http
    GET /MSI/token?resource=https://atlas.microsoft.com/&api-version=2019-08-01 HTTP/1.1
    Host: localhost:4141
    ```

    Respuesta de ejemplo:

    ```http
    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "access_token": "eyJ0eXAi…",
        "expires_on": "1586984735",
        "resource": "https://atlas.microsoft.com/",
        "token_type": "Bearer",
        "client_id": "..."
    }
    ```

6. Configuración de la seguridad de la función de Azure HttpTrigger

   * [Creación de una clave de acceso de la función](../azure-functions/functions-bindings-http-webhook-trigger.md?tabs=csharp#authorization-keys).
   * [Punto de conexión HTTP seguro](../azure-functions/functions-bindings-http-webhook-trigger.md?tabs=csharp#secure-an-http-endpoint-in-production) para la función de Azure en producción.
   
7. Configure el SDK web de Azure Maps de la aplicación web. 

    ```javascript
    //URL to custom endpoint to fetch Access token
    var url = 'https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>';

    var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                view: "Auto",
            authOptions: {
                authType: "anonymous",
                clientId: "<insert>", // azure map account client id
                getToken: function(resolve, reject, map) {
                    fetch(url).then(function(response) {
                        return response.text();
                    }).then(function(token) {
                        resolve(token);
                    });
                }
            }
        });

        // use the following events to debug, you can remove them at any time.
        map.events.add("tokenacquired", function () {
            console.log("token acquired");
        });
        map.events.add("error", function (err) {
            console.log(JSON.stringify(err.error));
        });
    ```

## <a name="grant-role-based-access"></a>Concesión del acceso basado en roles

Puede conceder el *control de acceso basado en roles de Azure (RBAC de Azure)* mediante la asignación de la identidad asignada por el sistema a una o más definiciones de roles de Azure. Para ver las definiciones de roles de Azure disponibles para Azure Maps, vaya a **Control de acceso (IAM)** . Seleccione **Roles** y busque los roles que comienzan con *Azure Maps*.

1. Vaya a su **cuenta de Azure Maps**. Seleccione **Control de acceso (IAM)**  > **Asignación de roles**.

    > [!div class="mx-imgBorder"]
    > ![Concesión del acceso mediante RBAC de Azure](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. En la pestaña **Asignaciones de roles**, en **Rol**, seleccione una definición de roles de Azure Maps integrada, como **Lector de datos de Azure Maps** o **Colaborador de datos de Azure Maps**. En **Asignar acceso a**, seleccione **Function App**. Seleccione la entidad de seguridad por nombre. Después, seleccione **Guardar**.

   * Consulte los detalles sobre la [asignación de roles de Azure](../role-based-access-control/role-assignments-portal.md).

> [!WARNING]
> Las definiciones de roles de Azure Maps integradas proporcionan un acceso de autorización muy amplio a muchas API REST de Azure Maps. Para restringir al mínimo el acceso a las API, consulte cómo [crear una definición de roles personalizada y asignar la identidad asignada por el sistema](../role-based-access-control/custom-roles.md) a la definición de roles personalizada. Esto proporcionará el privilegio mínimo necesario para que la aplicación acceda a Azure Maps.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el escenario de aplicación de página única:
> [!div class="nextstepaction"]
> [Aplicación de página única](../active-directory/develop/scenario-spa-overview.md)

Descubra las métricas de uso de API de la cuenta de Azure Maps:
> [!div class="nextstepaction"]
> [Visualización de métricas de uso](how-to-view-api-usage.md)

Explore otros ejemplos acerca de cómo integrar Azure AD con Azure Maps:
> [!div class="nextstepaction"]
> [Ejemplos de Azure Maps](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples/tree/master/src/ClientGrant)