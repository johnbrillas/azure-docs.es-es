---
ms.openlocfilehash: a51121b9dd9c7dcb894399fd9ad5f49cc5e07f3a
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102486641"
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

