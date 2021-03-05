---
ms.openlocfilehash: 9da0cbc3777359994fac3778dcc126c844754861
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657669"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Incorporación de la identidad administrada a Communication Services

### <a name="install-the-client-library-packages"></a>Instalación de los paquetes de la biblioteca cliente

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-client-library-packages"></a>Uso de los paquetes de la biblioteca cliente

Agregue `import` al código para que use la identidad de Azure.

```python
from azure.identity import DefaultAzureCredential
```

En los ejemplos siguientes se usa [DefaultAzureCredential](/python/api/azure.identity.defaultazurecredential). Esta credencial es adecuada para entornos de producción y desarrollo.

Para registrar una aplicación en el entorno de desarrollo y configurar variables de entorno, consulte [Autorización del acceso con una identidad administrada](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token"></a>Creación de una identidad y emisión de un token

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada y, a continuación, usar el cliente para emitir un token para un nuevo usuario:

```python
import azure.communication.identity 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Envío de un SMS con una identidad administrada de Azure

En el ejemplo de código siguiente se muestra cómo crear un objeto de cliente de servicio con una identidad administrada de Azure y, a continuación, usar el cliente para enviar un mensaje SMS:

```python
from azure.communication.sms import (
    PhoneNumberIdentifier,
    SendSmsOptions,
    SmsClient
)

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_phone_number=PhoneNumberIdentitifier(from_phone_number),
          to_phone_numbers=[PhoneNumberIdentifier(to_phone_number)],
          message=message_content,
          send_sms_options=SendSmsOptions(enable_delivery_report=True))  # optional property
     )
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre la autenticación](../../concepts/authentication.md)

Puede que también le interese:

- [Más información sobre el control de acceso basado en rol de Azure](../../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para Python](/net/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../../quickstarts/telephony-sms/send.md)
- [Más información acerca de SMS](../../concepts/telephony-sms/concepts.md)