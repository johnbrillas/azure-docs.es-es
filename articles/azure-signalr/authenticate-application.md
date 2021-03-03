---
title: Autenticación de una aplicación para acceder a Azure SignalR Service
description: En este artículo se proporciona información sobre cómo autenticar una aplicación con Azure Active Directory para acceder a Azure SignalR Service.
author: terencefan
ms.author: tefa
ms.service: signalr
ms.topic: conceptual
ms.date: 08/03/2020
ms.openlocfilehash: 597b69c1180ea1fb2a6812d648f8b8ad37707d66
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101092601"
---
# <a name="authenticate-an-application-with-azure-active-directory-to-access-azure-signalr-service"></a>Autenticación de una aplicación con Azure Active Directory para acceder a Azure SignalR Service
Microsoft Azure proporciona una administración integrada del control de acceso para recursos y aplicaciones basados en Azure Active Directory (Azure AD). Una ventaja clave de usar Azure AD con Azure SignalR Service es que ya no es necesario almacenar las credenciales en el código. En su lugar, puede solicitar un token de acceso de OAuth 2.0 desde la Plataforma de identidad de Microsoft. El nombre del recurso para solicitar un token es `https://signalr.azure.com/`. Azure AD autentica la entidad de seguridad (aplicación, grupo de recursos o entidad de servicio) mediante la ejecución de la aplicación. Si la autenticación se realiza correctamente, Azure AD devuelve un token de acceso a la aplicación y esta puede entonces usar el token de acceso para autorizar la solicitud a los recursos de Azure SignalR Service

Cuando un rol se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos a esa entidad de seguridad. El acceso puede tener como ámbito el nivel de suscripción, el grupo de recursos o el recurso de Azure SignalR. Las entidades de seguridad de Azure AD pueden asignar roles a un usuario, un grupo, una entidad de servicio de aplicación o una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md). 

> [!NOTE]
> Una definición de roles es una colección de permisos. El control de acceso basado en rol (RBAC) controla cómo se aplican estos permisos a través de la asignación de roles. Una asignación de roles consta de tres elementos: entidad de seguridad, definición de rol y ámbito. Para más información, consulte [Descripción de los distintos roles](../role-based-access-control/overview.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registro de la aplicación con un inquilino de Azure AD
El primer paso para usar Azure AD con el fin de autorizar los recursos de Azure SignalR Service es registrar la aplicación con un inquilino de Azure AD desde [Azure Portal](https://portal.azure.com/). Al registrar la aplicación, se facilita información sobre la aplicación a AD. Azure AD proporciona un id. de cliente (también denominado id. de aplicación) que se puede usar para asociar la aplicación con el runtime de Azure AD. Para más información sobre el identificador de cliente, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md). 

En las imágenes siguientes se muestran los pasos para registrar una aplicación web:

![Registro de una aplicación](./media/authenticate/app-registrations-register.png)

> [!Note]
> Si registra la aplicación como una aplicación nativa, puede especificar cualquier URI válido para el URI de redirección. Para las aplicaciones nativas, no es necesario que este valor sea una dirección URL real. Para las aplicaciones web, el URI de redirección debe ser un URI válido, ya que especifica la dirección URL a la que se proporcionan los tokens.

Una vez que ha registrado su aplicación, verá el **id. de la aplicación (o id. de cliente)** en **Configuración**:

![Id. de aplicación de la aplicación registrada](./media/authenticate/application-id.png)

Para más información sobre cómo registrar una aplicación con Azure AD, consulte [Integración de aplicaciones con Azure Active Directory](../active-directory/develop/quickstart-register-app.md).


### <a name="create-a-client-secret"></a>Creación de un secreto de cliente   
La aplicación necesita un secreto de cliente para demostrar su identidad al solicitar un token. Para agregar el secreto de cliente, siga estos pasos.

1. Vaya a su registro de aplicaciones en Azure Portal.
1. Seleccione la configuración de **Certificados y secretos**.
1. En **Secretos de cliente**, seleccione **Nuevo secreto de cliente** para crear un nuevo secreto.
1. Proporcione una descripción para el secreto y elija el intervalo de expiración deseado.
1. Copie inmediatamente el valor del nuevo secreto en una ubicación segura. El valor completo se le muestra solo una vez.

![Creación de un secreto de cliente](./media/authenticate/client-secret.png)

### <a name="upload-a-certificate"></a>Carga de un certificado

También puede cargar una certificación en lugar de crear un secreto de cliente.

![Carga de un certificado](./media/authenticate/certification.png)

## <a name="assign-azure-roles-using-the-azure-portal"></a>Asignación de roles de Azure mediante Azure Portal  
Para más información sobre cómo administrar el acceso a los recursos de Azure con Azure RBAC y Azure Portal, consulte [este artículo](..//role-based-access-control/role-assignments-portal.md). 

Después de determinar el ámbito adecuado de una asignación de roles, vaya a ese recurso en Azure Portal. Acceda a la configuración Control de acceso (IAM) del recurso y siga estas instrucciones para administrar las asignaciones de roles:

1. En [Azure Portal](https://portal.azure.com/), vaya al recurso de SignalR.
1. Seleccione **Control de acceso (IAM)** para mostrar la configuración del control de acceso de Azure SignalR. 
1. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles. Seleccione el botón **Agregar** de la barra de herramientas y luego seleccione **Agregar asignación de roles**. 

    ![Botón Agregar de la barra de herramientas](./media/authenticate/role-assignments-add-button.png)

1. En la página **Agregar asignación de roles**, siga estos pasos:
    1. Seleccione el **rol de Azure SignalR** que quiere asignar. 
    1. Realice una búsqueda para localizar la **entidad de seguridad** (usuario, grupo, entidad de servicio) a la que quiere asignar el rol.
    1. Seleccione **Guardar** para guardar la asignación de roles. 

        ![Asignación de un rol a una aplicación](./media/authenticate/assign-role-to-application.png)

    1. La identidad a la que ha asignado el rol aparece enumerada debajo de ese rol. Por ejemplo, la imagen siguiente muestra que los elementos `signalr-dev` y `signalr-service` de la aplicación se encuentran en el rol del servidor de aplicaciones de SignaIR. 
        
        ![Lista de asignación de roles](./media/authenticate/role-assignment-list.png)

Puede realizar este mismo procedimiento para asignar un rol que tenga como ámbito un grupo de recursos o una suscripción. Una vez que defina el rol y su ámbito, puede probar este comportamiento con ejemplos [en esta ubicación de GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac).

## <a name="sample-codes-while-configuring-your-app-server"></a>Códigos de ejemplo al configurar el servidor de aplicaciones.

Agregue las siguientes opciones cuando `AddAzureSignalR`:

```C#
services.AddSignalR().AddAzureSignalR(option =>
{
    option.ConnectionString = "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientSecret=<clientSecret>;tenantId=<tenantId>";
});
```

O simplemente configure `ConnectionString` en el archivo `appsettings.json`.

```json
{
"Azure": {
    "SignalR": {
      "Enabled": true,
      "ConnectionString": "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientSecret=<clientSecret>;tenantId=<tenantId>"
    }
  },
}
```

Al usar `certificate`, cambie `clientSecret` a `clientCert`, como sigue:

```C#
    option.ConnectionString = "Endpoint=https://<name>.signalr.net;AuthType=aad;clientId=<clientId>;clientCert=<clientCertFilepath>;tenantId=<tenantId>";
```

## <a name="next-steps"></a>Pasos siguientes
- Para más información acerca de RBAC, consulte [¿Qué es el control de acceso basado en rol (RBAC) de Azure?](../role-based-access-control/overview.md).
- Para información sobre cómo asignar y administrar las asignaciones de roles de Azure con Azure PowerShell, la CLI de Azure o la API de REST, consulte estos artículos:
    - [Administración del control de acceso basado en rol (RBAC) con Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)  
    - [Administración del control de acceso basado en rol (RBAC) con la CLI de Azure](../role-based-access-control/role-assignments-cli.md)
    - [Administración del control de acceso basado en rol (RBAC) con la API de REST](../role-based-access-control/role-assignments-rest.md)
    - [Administración del acceso a los recursos de Azure mediante RBAC y plantillas de Azure Resource Manager](../role-based-access-control/role-assignments-template.md)

Consulte los artículos relacionados siguientes:
- [Autenticación de una identidad administrada con Azure Active Directory para acceder a Azure SignalR Service](authenticate-managed-identity.md)
- [Autorización del acceso a Azure SignalR Service mediante Azure Active Directory](authorize-access-azure-active-directory.md)