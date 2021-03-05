---
ms.openlocfilehash: 2c816f005dea452c3ffa2889aa7d2742a5762759
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657674"
---
## <a name="add-managed-identity-to-your-communication-services-solution-net"></a>Incorporación de la identidad administrada a su solución de Communication Services (.Net)

### <a name="install-the-client-library-packages"></a>Instalación de los paquetes de la biblioteca cliente

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>Uso de los paquetes de la biblioteca cliente

Agregue las siguientes directivas `using` al código para usar las bibliotecas de cliente de Azure Storage y Azure Identity.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Sms;
```

En los ejemplos siguientes se usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential). Esta credencial es adecuada para entornos de producción y desarrollo.

Las variables de entorno `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` y `AZURE_TENANT_ID` son necesarias para crear un objeto `DefaultAzureCredential`. Para crear una aplicación registrada en el entorno de desarrollo y configurar variables de entorno, consulte [Autorización del acceso con una identidad administrada](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Creación de una identidad y emisión de un token con una identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con tokens de Azure Active Directory.

A continuación, use el cliente para emitir un token para un nuevo usuario:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndGetTokenAsync(Uri resourceEdnpoint)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();

          var tokenResponse = await client.GetTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Envío de un SMS con identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio SMS con una identidad administrada y, a continuación, usar el cliente para enviar un mensaje SMS:

```csharp
     public async Task SendSms(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
          // You can find your endpoint and access key from your resource in the Azure portal
          String resourceEndpoint = "https://<RESOURCE_NAME>.communication.azure.com";

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

- [Más información sobre el control de acceso basado en rol de Azure](../../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para .NET](/dotnet/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../telephony-sms/send.md)
- [Más información acerca de SMS](../../concepts/telephony-sms/concepts.md)
