---
title: Almacenamiento de confianza de Azure Media Services
description: En este tutorial, aprenderá a habilitar el almacenamiento de confianza para Azure Media Services, a usar identidades administradas para el almacenamiento de confianza y a proporcionar a los servicios de Azure acceso a una cuenta de almacenamiento cuando se utiliza un firewall o una VPN.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 2/8/2021
ms.openlocfilehash: 18cb4e3ada94822c2f4cb1ca7675310a37e44e84
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100590647"
---
# <a name="tutorial-media-services-trusted-storage"></a>Tutorial: Almacenamiento de confianza para Media Services

En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Habilitar el almacenamiento de confianza para Azure Media Services
> - Usar identidades administradas para el almacenamiento de confianza
> - Dar acceso a los servicios de Azure a una cuenta de almacenamiento cuando se utiliza el control de acceso a la red, como un firewall o una VPN

Con 2020-05-01 API, puede habilitar el almacenamiento de confianza asociando una identidad administrada a una cuenta de Media Services.

>[!NOTE]
>El almacenamiento de confianza solo está disponible en la API y actualmente no está habilitado en Azure Portal.

Media Services puede acceder automáticamente a su cuenta de almacenamiento mediante la autenticación del sistema. Media Services valida que la cuenta de Media Services y la cuenta de almacenamiento se encuentran en la misma suscripción. También se valida que el usuario que agrega la asociación tenga acceso a la cuenta de almacenamiento con RBAC de Azure Resource Manager.

Sin embargo, si quiere usar un control de acceso de red para proteger la cuenta de almacenamiento y habilitar el almacenamiento de confianza, se requiere la autenticación de [identidades administradas](concept-managed-identities.md). Esta permite que Media Services acceda a la cuenta de almacenamiento que se ha configurado con un firewall o una restricción de red virtual a través del acceso de almacenamiento de confianza.

## <a name="overview"></a>Información general

> [!IMPORTANT]
> Use 2020-05-01 API para todas las solicitudes de Media Services.

Estos son los pasos generales para crear un almacenamiento de confianza para Media Services:

1. Cree un grupo de recursos.
1. Cree una cuenta de almacenamiento.
1. Sondee la cuenta de almacenamiento hasta que esté lista. Cuando la cuenta de almacenamiento esté lista, la solicitud devolverá el identificador de la entidad de servicio.
1. Busque el identificador del rol *Colaborador de datos de Storage Blob*.
1. Llame al proveedor de autorización y agregue una asignación de roles.
1. Actualice la cuenta de Media Services para autenticarse en la cuenta de almacenamiento mediante la identidad administrada.
1. Elimine los recursos si no desea seguir utilizándolos y que le cobren por ellos.

<!-- Link to storage role contributor role access differences information -->

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita una suscripción de Azure.  Si no tiene ninguna suscripción a Azure, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/free/).

### <a name="get-your-tenant-id-and-subscription-id"></a>Obtención del identificador de inquilino y el identificador de suscripción

Si no sabe cómo obtener el identificador de inquilino y el identificador de suscripción, consulte [Cómo buscar el identificador de inquilino](how-to-set-azure-tenant.md) y [Búsqueda del identificador de inquilino](how-to-set-azure-tenant.md).

### <a name="create-a-service-principal-and-secret"></a>Creación de una entidad de servicio y el secreto

Si no sabe cómo crear una entidad de servicio y un secreto, consulte [Obtención de credenciales para acceder a la API de Media Services](access-api-howto.md).

## <a name="use-a-rest-client"></a>Uso de un cliente REST

Este script está pensado para su uso con un cliente de REST, como lo que está disponible en las extensiones de Visual Studio Code.  Adáptelo a un entorno de desarrollo.

## <a name="set-initial-variables"></a>Establecimiento de variables iniciales

Esta parte del script se usa en un cliente REST. Puede usar variables de manera diferente en el entorno de desarrollo.

```rest
### AAD details
@tenantId = your tenant ID
@servicePrincipalId = the service principal ID
@servicePrincipalSecret = the service principal secret

### AAD resources
@armResource = https%3A%2F%2Fmanagement.core.windows.net%2F
@graphResource = https%3A%2F%2Fgraph.windows.net%2F
@storageResource = https%3A%2F%2Fstorage.azure.com%2F

### Service endpoints
@armEndpoint = management.azure.com
@graphEndpoint = graph.windows.net
@aadEndpoint = login.microsoftonline.com

### ARM details
@subscription = your subscription id
@resourceGroup = the resource group you'll be creating
@storageName = the name of the storage you'll be creating
@accountName = the name of the account you'll be creating
@resourceLocation = East US (or the location that works best for your region)
```

## <a name="get-a-token-for-azure-resource-manager"></a>Obtención de un token para Azure Resource Manager

```rest
// @name getArmToken
POST https://{{aadEndpoint}}/{{tenantId}}/oauth2/token
Accept: application/json
Content-Type: application/x-www-form-urlencoded

resource={{armResource}}&client_id={{servicePrincipalId}}&client_secret={{servicePrincipalSecret}}&grant_type=client_credentials
```

## <a name="get-a-token-for-the-graph-api"></a>Obtención de un token para Graph API

Esta parte del script se usa en un cliente REST. Puede usar variables de manera diferente en el entorno de desarrollo.

```rest
// @name getGraphToken
POST https://{{aadEndpoint}}/{{tenantId}}/oauth2/token
Accept: application/json
Content-Type: application/x-www-form-urlencoded

resource={{graphResource}}&client_id={{servicePrincipalId}}&client_secret={{servicePrincipalSecret}}&grant_type=client_credentials
```

## <a name="get-the-service-principal-details"></a>Obtención de los detalles de la entidad de servicio

```rest
// @name getServicePrincipals
GET https://{{graphEndpoint}}/{{tenantId}}/servicePrincipals?$filter=appId%20eq%20'{{servicePrincipalId}}'&api-version=1.6
x-ms-client-request-id: cae3e4f7-17a0-476a-a05a-0dab934ba959
Authorization:  Bearer {{getGraphToken.response.body.access_token}}
```

## <a name="store-the-service-principal-id"></a>Almacenamiento del identificador de la entidad de servicio

```rest
@servicePrincipalObjectId = {{getServicePrincipals.response.body.value[0].objectId}}
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

```rest
// @name createResourceGroup
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}
    ?api-version=2016-09-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
    "location": "{{resourceLocation}}"
}
```

## <a name="create-storage-account"></a>Crear cuenta de almacenamiento

```rest
// @name createStorageAccount
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}
    ?api-version=2019-06-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
    "sku": {
    "name": "Standard_GRS"
    },
    "kind": "StorageV2",
    "location": "{{resourceLocation}}",
    "properties": {
    }
}
```

## <a name="get-the-storage-account-status"></a>Obtención del estado de la cuenta de almacenamiento

La cuenta de almacenamiento tardará un rato en estar lista, por lo que esta solicitud sondea su estado.  Repita esta solicitud hasta que la cuenta de almacenamiento esté lista.

```rest
// @name getStorageAccountStatus
GET {{createStorageAccount.response.headers.Location}}
Authorization: Bearer {{getArmToken.response.body.access_token}}
```

## <a name="get-the-storage-account-details"></a>Obtención de los detalles de la cuenta de almacenamiento

Cuando la cuenta de almacenamiento esté lista, obtenga las propiedades de la cuenta de almacenamiento.

```rest
// @name getStorageAccount
GET https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}
    ?api-version=2019-06-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
```

## <a name="get-a-token-for-arm"></a>Obtención de un token para ARM

```rest
// @name getStorageToken
POST https://{{aadEndpoint}}/{{tenantId}}/oauth2/token
Accept: application/json
Content-Type: application/x-www-form-urlencoded

resource={{storageResource}}&client_id={{servicePrincipalId}}&client_secret={{servicePrincipalSecret}}&grant_type=client_credentials
```

## <a name="create-a-media-services-account-with-a-system-assigned-managed-identity"></a>Creación de una cuenta de Media Services con una identidad administrada asignada por el sistema

```rest
// @name createMediaServicesAccount
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Media/mediaservices/{{accountName}}?api-version=2020-05-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
  "identity": {
      "type": "SystemAssigned"
  },
  "properties": {
    "storageAccounts": [
      {
        "id": "{{getStorageAccountStatus.response.body.id}}"
      }
    ],
    "encryption": {
      "type": "SystemKey"
    }
  },
  "location": "{{resourceLocation}}"
}
```

## <a name="get-the-storage-storage-blob-data-role-definition"></a>Obtención de la definición de rol de datos de Storage Blob

```rest
// @name getStorageBlobDataContributorRoleDefinition
GET https://management.azure.com/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}/providers/Microsoft.Authorization/roleDefinitions?$filter=roleName%20eq%20%27Storage%20Blob%20Data%20Contributor%27&api-version=2015-07-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
```

### <a name="set-the-storage-role-assignment"></a>Establecimiento de la asignación de roles de almacenamiento

La asignación de roles indica que la entidad de servicio de la cuenta de Media Services tiene el rol de almacenamiento *Colaborador de datos de Storage Blob*.  Esto puede tardar unos minutos y es importante esperar o la cuenta de Media Services no se configurará correctamente.

```rest
PUT https://management.azure.com/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}/providers/Microsoft.Authorization/roleAssignments/{{$guid}}?api-version=2020-04-01-preview
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "roleDefinitionId": "/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}/providers/Microsoft.Authorization/roleDefinitions/{{getStorageBlobDataContributorRoleDefinition.response.body.value[0].name}}",
    "principalId": "{{createMediaServicesAccount.response.body.identity.principalId}}"
  }
}
```

## <a name="give-managed-identity-bypass-access-to-the-storage-account"></a>Concesión de acceso de omisión de la identidad administrada a la cuenta de almacenamiento

Esta acción cambia el acceso de la identidad administrada por el sistema a la identidad administrada. De esta manera, la cuenta de almacenamiento puede acceder a la cuenta de almacenamiento a través de un firewall, ya que los servicios de Azure pueden acceder a la cuenta de almacenamiento independientemente de las reglas de acceso de IP (ACL).

De nuevo, espere hasta que se haya asignado el rol en la cuenta de almacenamiento o la cuenta de Media Services se configurará incorrectamente.

```rest
// @name setStorageAccountFirewall
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}
    ?api-version=2019-06-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
    "sku": {
    "name": "Standard_GRS"
    },
    "kind": "StorageV2",
    "location": "{{resourceLocation}}",
    "properties": {
      "minimumTlsVersion": "TLS1_2",
      "networkAcls": {
        "bypass": "AzureServices",
        "virtualNetworkRules": [],
        "ipRules": [],
        "defaultAction": "Deny"
      }
    }
}
```

## <a name="update-the-media-services-account-to-use-the-managed-identity"></a>Actualizar la cuenta de Media Services para usar la identidad administrada

Es posible que sea necesario reintentar esta solicitud varias veces, ya que la asignación de roles de almacenamiento puede tardar unos minutos en propagarse.

```rest
// @name updateMediaServicesAccountWithManagedStorageAuth
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Media/mediaservices/{{accountName}}?api-version=2020-05-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
  "identity": {
      "type": "SystemAssigned"
  },
  "properties": {
    "storageAccounts": [
      {
        "id": "{{getStorageAccountStatus.response.body.id}}"
      }
    ],
    "storageAuthentication": "ManagedIdentity",
    "encryption": {
      "type": "SystemKey"
    }
  },
  "location": "{{resourceLocation}}"
}
```

## <a name="test-access"></a>Prueba del acceso

Pruebe el acceso mediante la creación de un recurso en la cuenta de almacenamiento.

```rest
// @name createAsset
PUT https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Media/mediaservices/{{accountName}}/assets/testasset{{index}}withoutmi?api-version=2018-07-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
Content-Type: application/json; charset=utf-8

{
}
```

## <a name="delete-resources"></a>Eliminar recursos

Si no desea mantener los recursos que ha creado y seguir pagando por ellos, elimínelos.

```rest
### Clean up the Storage account
DELETE https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Storage/storageAccounts/{{storageName}}
    ?api-version=2019-06-01
Authorization: Bearer {{getArmToken.response.body.access_token}}

### Clean up the Media Services account
DELETE https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Media/mediaservices/{{accountName}}?api-version=2020-05-01
Authorization: Bearer {{getArmToken.response.body.access_token}}

### Clean up the Media Services account
GET https://{{armEndpoint}}/subscriptions/{{subscription}}/resourceGroups/{{resourceGroup}}/providers/Microsoft.Media/mediaservices/{{accountName}}?api-version=2020-05-01
Authorization: Bearer {{getArmToken.response.body.access_token}}
```

## <a name="next-steps"></a>Pasos siguientes

[Cómo crear un recurso](how-to-create-asset.md)
