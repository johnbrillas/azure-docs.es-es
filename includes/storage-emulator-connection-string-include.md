---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/28/2020
ms.author: tamram
ms.openlocfilehash: 85cfe3b062d7d9ef3a7bdcf29ef7d2125f8f3ae4
ms.sourcegitcommit: 31d242b611a2887e0af1fc501a7d808c933a6bf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97812756"
---
El emulador es compatible con una sola cuenta fija y una clave de autenticación ya conocida para la autenticación de clave compartida. Esta cuenta y clave son las únicas credenciales de clave compartida que se admiten para su uso con el emulador. Son las siguientes:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> La clave de autenticación admitida por el emulador está pensada para comprobar únicamente la funcionalidad de su código de autenticación de cliente. No responde a ningún propósito de seguridad. No puede utilizar la cuenta de almacenamiento y la clave de producción con el emulador. Se debe tener en cuenta que no se puede utilizar la cuenta de desarrollo con datos de producción.
>
> El emulador admite solo la conexión a través de HTTP. Sin embargo, HTTPS es el protocolo recomendado para obtener acceso a recursos en una cuenta de producción de Azure Storage.
>

#### <a name="connect-to-the-emulator-account-using-the-shortcut"></a>Conexión a la cuenta del emulador mediante el acceso directo

La manera más fácil de conectarse al emulador desde la aplicación consiste en configurar, dentro del archivo de configuración de la aplicación, una cadena de conexión que haga referencia al acceso directo `UseDevelopmentStorage=true`. El acceso directo equivale a la cadena de conexión completa para el emulador, que especifica el nombre de la cuenta, la clave de cuenta y los puntos de conexión del emulador para cada uno de los servicios de Azure Storage:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

El siguiente fragmento de código de .NET muestra cómo se puede usar el acceso directo desde un método que toma una cadena de conexión. Por ejemplo, el constructor [BlobContainerClient(String, String)](/dotnet/api/azure.storage.blobs.blobcontainerclient.-ctor#Azure_Storage_Blobs_BlobContainerClient__ctor_System_String_System_String_) toma una cadena de conexión.

```csharp
BlobContainerClient blobContainerClient = new BlobContainerClient("UseDevelopmentStorage=true", "sample-container");
blobContainerClient.CreateIfNotExists();
```

Asegúrese de que el emulador se está ejecutando antes de llamar al código del fragmento.
