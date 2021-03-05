---
ms.openlocfilehash: 0fd97fe0eebb23130513409698b75d2ee7b98a6f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657666"
---
## <a name="add-managed-identity-to-your-communication-services-solution-js"></a>Incorporación de la identidad administrada a Communication Services (JS)

### <a name="install-the-client-library-packages"></a>Instalación de los paquetes de la biblioteca cliente

```console
npm install @azure/communication-identity
npm install @azure/communication-common
npm install @azure/communication-sms
npm install @azure/identity
```

### <a name="use-the-client-library-packages"></a>Uso de los paquetes de la biblioteca cliente

Agregue las siguientes directivas `import` al código para usar las bibliotecas de cliente de Azure Storage y Azure Identity.

```typescript
import { DefaultAzureCredential } from "@azure/identity";
import { CommunicationIdentityClient } from "@azure/communication-identity";
import { SmsClient } from "@azure/communication-sms";
```

En los ejemplos siguientes se usa [DefaultAzureCredential](/javascript/api/azure.identity.defaultazurecredential). Esta credencial es adecuada para entornos de producción y desarrollo.

Para registrar una aplicación en el entorno de desarrollo y configurar variables de entorno, consulte [Autorización del acceso con una identidad administrada](../managed-identity-from-cli.md).  

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Creación de una identidad y emisión de un token con una identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada y, a continuación, usar el cliente para emitir un token para un nuevo usuario:

```JavaScript
export async function createIdentityAndIssueToken(resourceEndpoint: string): Promise<CommunicationUserToken> {
     let credential = new DefaultAzureCredential();
     const client = new CommunicationIdentityClient(resourceEndpoint, credential);
     return await client.createUserWithToken(["chat"]);
}
```

### <a name="send-an-sms-with-managed-identity"></a>Envío de un SMS con identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada y, a continuación, usar el cliente para enviar un mensaje SMS:

```JavaScript
export async function sendSms(resourceEndpoint: string, fromNumber: any, toNumber: any, message: string) {
     let credential = new DefaultAzureCredential();
     const smsClient = new SmsClient(resourceEndpoint, credential);
     const sendRequest: SendRequest = { 
          from: fromNumber, 
          to: [toNumber], 
          message: message 
     };

     const response = await smsClient.send(
          sendRequest, 
          {} //Optional SendOptions
          );
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre la autenticación](../concepts/authentication.md)

Puede que también le interese:

- [Más información sobre el control de acceso basado en rol de Azure](../../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para JS](/javascript/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../../quickstarts/telephony-sms/send.md)
- [Más información acerca de SMS](../../concepts/telephony-sms/concepts.md)

