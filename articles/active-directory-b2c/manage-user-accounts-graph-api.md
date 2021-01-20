---
title: Administración de usuarios con Microsoft Graph API
titleSuffix: Azure AD B2C
description: Procedimiento para administrar los usuarios de un inquilino de Azure AD B2C llamando a Microsoft Graph API y mediante una identidad de aplicación para automatizar el proceso.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/13/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff3cd858de86d21637f4a7a9ab9d9a83c7022f5a
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98178881"
---
# <a name="manage-azure-ad-b2c-user-accounts-with-microsoft-graph"></a>Administrar cuentas de usuario de Azure AD B2C con Microsoft Graph

Microsoft Graph permite administrar cuentas de usuario en el directorio de Azure AD B2C, ya que proporciona métodos de creación, lectura, actualización y eliminación en Microsoft Graph API. Puede migrar un almacén de usuarios existente a un inquilino de Azure AD B2C y realizar otras operaciones de administración de cuentas de usuario llamando a Microsoft Graph API.

En las secciones siguientes, se presentan los aspectos clave de la administración de usuarios de Azure AD B2C con Microsoft Graph API. Las operaciones, los tipos y las propiedades de Microsoft Graph API que se presentan aquí son un subconjunto de las que aparecen en la documentación de referencia de Microsoft Graph API.

## <a name="register-a-management-application"></a>Registrar una aplicación de administración

Antes de que cualquier aplicación de administración de usuarios o script que escriba pueda interactuar con los recursos de su inquilino de Azure AD B2C, necesita un registro de aplicación que conceda permisos para hacerlo.

Siga los pasos de este artículo de procedimientos para crear un registro de aplicación que pueda usar la aplicación de administración:

[Administrar Azure AD B2C con Microsoft Graph](microsoft-graph-get-started.md)

## <a name="user-management-microsoft-graph-operations"></a>Operaciones de Microsoft Graph para la administración de usuarios

Las siguientes operaciones de administración de usuarios están disponibles en [Microsoft Graph API](/graph/api/resources/user):

- [Obtención de una lista de usuarios](/graph/api/user-list)
- [Creación de un usuario](/graph/api/user-post-users)
- [Obtención de un usuario](/graph/api/user-get)
- [Actualizar usuario](/graph/api/user-update)
- [Eliminar un usuario](/graph/api/user-delete)


## <a name="code-sample-how-to-programmatically-manage-user-accounts"></a>Ejemplo de código: Administración de cuentas de usuario mediante programación

Este ejemplo de código es una aplicación de consola .NET Core que usa el [SDK de Microsoft Graph](/graph/sdks/sdks-overview) para interactuar con Microsoft Graph API. En su código se muestra cómo llamar a la API para administrar usuarios mediante programación en un inquilino de Azure AD B2C.
Puede [descargar el archivo de ejemplo](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management/archive/master.zip) (*.zip), [examinar el repositorio](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) de GitHub o clonar el repositorio:

```cmd
git clone https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management.git
```

Después de obtener el ejemplo de código, configúrelo para su entorno y, a continuación, compile el proyecto:

1. Abra el proyecto en [Visual Studio](https://visualstudio.microsoft.com) o en [Visual Studio Code](https://code.visualstudio.com).
1. Abra `src/appsettings.json`.
1. En la sección `appSettings`, reemplace `your-b2c-tenant` por el nombre de su inquilino, y `Application (client) ID` y `Client secret` por los valores para el registro de la aplicación de administración (consulte la sección de este artículo [Registrar una aplicación de administración](#register-a-management-application)).
1. Abra una ventana de consola en el clon local del repositorio, cambie al directorio `src` y, a continuación, compile el proyecto:
    ```console
    cd src
    dotnet build
    ```
1. Ejecute la aplicación con el comando `dotnet`:

    ```console
    dotnet bin/Debug/netcoreapp3.1/b2c-ms-graph.dll
    ```

La aplicación muestra una lista de los comandos que se pueden ejecutar. Por ejemplo, obtener todos los usuarios, obtener un solo usuario, eliminar un usuario, actualizar la contraseña de un usuario y realizar una importación en bloque.

### <a name="code-discussion"></a>Discusión de código

En el código de ejemplo se usa el [SDK de Microsoft Graph](/graph/sdks/sdks-overview), que se ha diseñado para simplificar la creación de aplicaciones de alta calidad, eficientes y resistentes que tienen acceso a Microsoft Graph.

Cualquier solicitud a Microsoft Graph API requiere un token de acceso para la autenticación. La solución usa el paquete de NuGet [Microsoft.Graph.Auth](https://www.nuget.org/packages/Microsoft.Graph.Auth/) que proporciona un contenedor basado en escenario de autenticación de la biblioteca de autenticación de Microsoft (MSAL) para su uso con el SDK de Microsoft Graph.

El método `RunAsync` en el archivo _Program.cs_:

1. Lee la configuración de la aplicación del archivo _appsettings.json_.
1. Inicializa el proveedor de autenticación mediante el flujo de [concesión de credenciales de cliente de OAuth 2.0](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md). Con el flujo de concesión de credenciales de cliente, la aplicación puede obtener un token de acceso para llamar a Microsoft Graph API.
1. Configura el cliente del servicio de Microsoft Graph con el proveedor de autenticación:

    ```csharp
    // Read application settings from appsettings.json (tenant ID, app ID, client secret, etc.)
    AppSettings config = AppSettingsFile.ReadFromJsonFile();

    // Initialize the client credential auth provider
    IConfidentialClientApplication confidentialClientApplication = ConfidentialClientApplicationBuilder
        .Create(config.AppId)
        .WithTenantId(config.TenantId)
        .WithClientSecret(config.ClientSecret)
        .Build();
    ClientCredentialProvider authProvider = new ClientCredentialProvider(confidentialClientApplication);

    // Set up the Microsoft Graph service client with client credentials
    GraphServiceClient graphClient = new GraphServiceClient(authProvider);
    ```

A continuación, se usa la instancia inicializada de *GraphServiceClient* en _GraphService.cs_ para realizar las operaciones de administración de usuarios. Por ejemplo, obtener una lista de las cuentas de usuario en el inquilino:

```csharp
public static async Task ListUsers(GraphServiceClient graphClient)
{
    Console.WriteLine("Getting list of users...");

    // Get all users (one page)
    var result = await graphClient.Users
        .Request()
        .Select(e => new
        {
            e.DisplayName,
            e.Id,
            e.Identities
        })
        .GetAsync();

    foreach (var user in result.CurrentPage)
    {
        Console.WriteLine(JsonConvert.SerializeObject(user));
    }
}
```

El documento [Realizar llamadas API con los SDK de Microsoft Graph](/graph/sdks/create-requests) incluye información sobre cómo leer y escribir información de Microsoft Graph, usar `$select` para controlar las propiedades devueltas, proporcionar parámetros de consulta personalizados y usar los parámetros de consulta `$filter` y `$orderBy`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un índice completo de las operaciones de Microsoft Graph API compatibles con los recursos de Azure AD B2C, vea [Operaciones de Microsoft Graph disponibles para Azure AD B2C](microsoft-graph-operations.md).

<!-- LINK -->

[graph-objectIdentity]: /graph/api/resources/objectidentity
[graph-user]: (https://docs.microsoft.com/graph/api/resources/user)
