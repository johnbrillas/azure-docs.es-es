---
title: Uso de identidades administradas en Communication Services (.NET)
titleSuffix: An Azure Communication Services quickstart
description: Las identidades administradas le permiten autorizar a Azure Communication Services el acceso desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones y otros recursos.
services: azure-communication-services
author: stefang931
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 12/04/2020
ms.author: gistefan
ms.reviewer: mikben
ms.openlocfilehash: 9fd8a17deeb49d836ff5902042bdb88696e29f31
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416865"
---
# <a name="use-managed-identities-net"></a>Uso de las identidades administradas (.NET)

Comience a trabajar con Azure Communication Services mediante identidades administradas en .NET. Las bibliotecas cliente de administración y SMS de Communication Services admiten la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

En este inicio rápido se muestra cómo autorizar el acceso a las bibliotecas cliente de administración y SMS desde un entorno de Azure que admita identidades administradas. También se describe cómo probar el código en un entorno de desarrollo.

## <a name="prerequisites"></a>Prerrequisitos

 - Una cuenta de Azure con una suscripción activa. [Cree su cuenta de forma gratuita](https://azure.microsoft.com/free).
 - Un recurso activo de Communication Services y una cadena de conexión. [Cree un recurso de Communication Services](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp).

## <a name="setting-up"></a>Instalación

### <a name="enable-managed-identities-on-a-virtual-machine-or-app-service"></a>Habilitación de identidades administradas en una máquina virtual o en un servicio de aplicaciones

Las identidades administradas deben estar habilitadas en los recursos de Azure que va a autorizar. Para aprender a habilitar las identidades administradas para los recursos de Azure, consulte uno de estos artículos:

- [Azure Portal](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Bibliotecas cliente de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Servicios de aplicaciones](../../app-service/overview-managed-identity.md)

#### <a name="assign-azure-roles-with-the-azure-portal"></a>Asignación de roles de Azure mediante Azure Portal

1. Acceda a Azure Portal.
1. Vaya al recurso Azure Communication Services.
1. Vaya al menú Control de acceso (IAM) -> + Agregar -> Agregar asignación de roles.
1. Seleccione el rol "Colaborador" (este es el único rol admitido).
1. Seleccione "Identidad administrada asignada por el usuario" (o una identidad administrada asignada por el sistema) y elija la identidad deseada. Guarde la selección.

![Rol de la identidad administrada](media/managed-identity-assign-role.png)

#### <a name="assign-azure-roles-with-powershell"></a>Asignación de roles de Azure con PowerShell

Para asignar roles y permisos con PowerShell, consulte [Incorporación o eliminación de asignaciones de roles de Azure con Azure PowerShell](../../../articles/role-based-access-control/role-assignments-powershell.md).

## <a name="add-managed-identity-to-your-communication-services-solution"></a>Incorporación de la identidad administrada a Communication Services

### <a name="install-the-client-library-packages"></a>Instalación de los paquetes de la biblioteca cliente

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Configuration
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>Uso de los paquetes de la biblioteca cliente

Agregue las siguientes directivas `using` al código para usar las bibliotecas de cliente de Azure Storage y Azure Identity.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Configuration;
using Azure.Communication.Sms;
```

En los ejemplos siguientes se usa [DefaultAzureCredential](https://docs.microsoft.com/dotnet/api/azure.identity.defaultazurecredential). Esta credencial es adecuada para entornos de producción y desarrollo.

### <a name="create-an-identity-and-issue-a-token"></a>Creación de una identidad y emisión de un token

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con tokens de Azure Active Directory y, a continuación, usar el cliente para emitir un token para un nuevo usuario:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndIssueTokenAsync(Uri resourceEdnpoint) 
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();
     
          var tokenResponse = await client.IssueTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-azure-active-directory-tokens"></a>Envío de un SMS con tokens de Azure Active Directory

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con tokens de Azure Active Directory y, a continuación, usar el cliente para enviar un mensaje SMS:

```csharp

     public async Task SendSmsAsync(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre la autenticación](../concepts/authentication.md)

Puede que también le interese:

- [Más información sobre el control de acceso basado en rol de Azure](../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para .NET](https://docs.microsoft.com/dotnet/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../quickstarts/telephony-sms/send.md)
- [Más información acerca de SMS](../concepts/telephony-sms/concepts.md)
