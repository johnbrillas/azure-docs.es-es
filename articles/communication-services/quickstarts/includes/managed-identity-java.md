---
ms.openlocfilehash: bb3925c5c642f2a6d00f8f5b7fe6471676c23c30
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102486646"
---
## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Incorporación de la identidad administrada a Communication Services (Java)

### <a name="install-the-client-library-packages"></a>Instalación de los paquetes de la biblioteca cliente
En el archivo pom.xml, agregue los siguientes elementos de dependencia al grupo de dependencias.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="use-the-client-library-packages"></a>Uso de los paquetes de la biblioteca cliente

Agregue las siguientes directivas `import` al código para usar las bibliotecas cliente de identidades de Azure y las de Azure Communication Services.

```java
import com.azure.identity.*;
import com.azure.communication.sms.*;
import com.azure.communication.identity.*;
import com.azure.communication.common.*;
```

En los ejemplos siguientes se usa [DefaultAzureCredential](/java/api/azure.identity.defaultazurecredential). Esta credencial es adecuada para entornos de producción y desarrollo.

Las variables de entorno `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID` y `AZURE_TENANT_ID` son necesarias para crear un objeto `DefaultAzureCredential`. Para crear una aplicación registrada en el entorno de desarrollo y configurar variables de entorno, consulte [Autorización del acceso con una identidad administrada](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Creación de una identidad y emisión de un token con una identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada.
A continuación, use el cliente para emitir un token para un nuevo usuario:

```java
     public AccessToken createIdentityAndGetTokenAsync() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          CommunicationUserIdentifier user = communicationIdentityClient.createUser();
          AccessToken userToken = communicationIdentityClient.getToken(user, new ArrayList<>(Arrays.asList(CommunicationTokenScope.CHAT)));
          return userToken;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Envío de un SMS con identidad administrada

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada y, a continuación, usar el cliente para enviar un mensaje SMS:

```java
     public SendSmsResponse sendSms() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          SmsClient smsClient = new SmsClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          // Send the message and check the response for a message id
          SendSmsResponse response = smsClient.sendMessage(
               new PhoneNumberIdentifier("<leased-phone-number>"),
               to,
               "your message",
               options /* Optional */
          );
          return response;
    }
```

