---
title: Creación de entidades de Azure Service Bus mediante programación | Microsoft Docs
description: En este artículo se explica cómo aprovisionar dinámicamente o mediante programación las entidades y los espacios de nombres de Service Bus.
ms.devlang: dotnet
ms.topic: article
ms.date: 01/13/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 57192ab2ee1624cb18de832ac91c95290da727df
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98539871"
---
# <a name="dynamically-provision-service-bus-namespaces-and-entities"></a>Aprovisionamiento dinámico de espacios de nombres y entidades de Service Bus 
Las bibliotecas de administración de Azure Service Bus pueden aprovisionar dinámicamente las entidades y los espacios de nombres de Service Bus. Esto posibilita los escenarios complejos de implementación y mensajería, y hace posible determinar mediante programación qué entidades aprovisionar. Estas bibliotecas están actualmente disponibles para .NET.

## <a name="overview"></a>Introducción
Hay tres bibliotecas de administración disponibles para crear y administrar entidades de Service Bus. Son las siguientes:

- [Azure.Messaging.ServiceBus.Administration](#azuremessagingservicebusadministration)
- [Microsoft.Azure.ServiceBus.Management](#microsoftazureservicebusmanagement)
- [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus)

Todos estos paquetes admiten operaciones de creación, obtención, enumeración, eliminación y actualización en **colas, temas y suscripciones**. Sin embargo, solo [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus) admite operaciones de creación, actualización, enumeración, obtención y eliminación en **espacios de nombres**, así como enumerar y volver a generar claves de SAS, etc. 

La biblioteca Microsoft.Azure.Management.ServiceBus solo funciona con la autenticación de Azure Active Directory (Azure AD) y no admite el uso de una cadena de conexión. En cambio, las otras dos bibliotecas (Azure.Messaging.ServiceBus y Microsoft.Azure.ServiceBus) admiten el uso de una cadena de conexión para la autenticación con el servicio y son más fáciles de usar. Entre estas bibliotecas, Azure.Messaging.ServiceBus es la más reciente y eso es lo que se recomienda usar.

En las siguientes secciones, se proporcionan más detalles de estas bibliotecas. 

## <a name="azuremessagingservicebusadministration"></a>Azure.Messaging.ServiceBus.Administration
Puede usar la clase [ServiceBusAdministrationClient](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient) en el espacio de nombres [Azure.Messaging.ServiceBus.Administration](/dotnet/api/azure.messaging.servicebus.administration) para administrar los espacios de nombres, las colas, los temas y las suscripciones. Este es el código de ejemplo. Para ver un ejemplo completo, consulte el [ejemplo de CRUD ](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/servicebus/Azure.Messaging.ServiceBus/tests/Samples/Sample07_CrudOperations.cs).

```csharp
using System;
using System.Threading.Tasks;

using Azure.Messaging.ServiceBus.Administration;

namespace adminClientTrack2
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var adminClient = new ServiceBusAdministrationClient(connectionString);
            bool queueExists = await adminClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                var options = new CreateQueueOptions(QueueName)
                {
                    MaxDeliveryCount = 3                    
                };
                await adminClient.CreateQueueAsync(options);
            }


            bool topicExists = await adminClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                var options = new CreateTopicOptions(TopicName)
                {
                    MaxSizeInMegabytes = 1024
                };
                await adminClient.CreateTopicAsync(options);
            }

            bool subscriptionExists = await adminClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                var options = new CreateSubscriptionOptions(TopicName, SubscriptionName)
                {
                    DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0)
                };
                await adminClient.CreateSubscriptionAsync(options);
            }
        }
    }
}

```


## <a name="microsoftazureservicebusmanagement"></a>Microsoft.Azure.ServiceBus.Management 
Puede usar la clase [ManagementClient](/dotnet/api/microsoft.azure.servicebus.management.managementclient) en el espacio de nombres [Microsoft.Azure.ServiceBus.Management](/dotnet/api/microsoft.azure.servicebus.management) para administrar los espacios de nombres, las colas, los temas y las suscripciones. Este es el código de ejemplo: 

> [!NOTE]
> Se recomienda usar la clase `ServiceBusAdministrationClient` de la biblioteca `Azure.Messaging.ServiceBus.Administration`, que es el SDK más reciente. Para más información, consulte la [primera sección](#azuremessagingservicebusadministration). 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.ServiceBus.Management;

namespace SBusManagementClient
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var managementClient = new ManagementClient(connectionString);
            bool queueExists = await managementClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                QueueDescription qd = new QueueDescription(QueueName);
                qd.MaxSizeInMB = 1024;
                qd.MaxDeliveryCount = 3;
                await managementClient.CreateQueueAsync(qd);
            }


            bool topicExists = await managementClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                TopicDescription td = new TopicDescription(TopicName);
                td.MaxSizeInMB = 1024;
                td.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                await managementClient.CreateTopicAsync(td);
            }

            bool subscriptionExists = await managementClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                SubscriptionDescription sd = new SubscriptionDescription(TopicName, SubscriptionName);
                sd.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                sd.MaxDeliveryCount = 3;
                await managementClient.CreateSubscriptionAsync(sd);
            }
        }
    }
}
```


## <a name="microsoftazuremanagementservicebus"></a>Microsoft.Azure.Management.ServiceBus 
Esta biblioteca forma parte del SDK del plano de control basado en Azure Resource Manager. 

### <a name="prerequisites"></a>Requisitos previos

Para comenzar a usar esta biblioteca, debe autenticarse con el servicio de Azure Active Directory (Azure AD). Azure AAD requiere que se autentique como una entidad de servicio que proporciona acceso a los recursos de Azure. Para más información sobre cómo crear una entidad de servicio, consulte uno de los siguientes artículos:  

* [Uso de Azure Portal para crear una aplicación de Active Directory y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md)
* [Uso de Azure PowerShell para crear a una entidad de servicio para acceder a recursos](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Uso de la CLI de Azure para crear a una entidad de servicio para acceder a recursos](/cli/azure/create-an-azure-service-principal-azure-cli)

Estos tutoriales le proporcionan valores para `AppId` (identificador de cliente), `TenantId` y `ClientSecret` (clave de autenticación), que usan las bibliotecas de administración con fines de autenticación. Debe tener como mínimo los permisos [**propietario de datos**](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner) o [**colaborador de Azure Service Bus**](../role-based-access-control/built-in-roles.md#contributor) para el grupo de recursos en el que quiere realizar la ejecución.

### <a name="programming-pattern"></a>Modelo de programación

El patrón para manipular los recursos de Service Bus sigue un protocolo común:

1. Obtenga un token de Azure AD utilizando la biblioteca **Microsoft.IdentityModel.Clients.ActiveDirectory**:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.azure.com/", new ClientCredential(clientId, clientSecret));
   ```
2. Cree el objeto `ServiceBusManagementClient`:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Establezca los parámetros de `CreateOrUpdate` en los valores especificados:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Ejecute la llamada:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

### <a name="complete-code-to-create-a-queue"></a>Código completo para crear una cola
Este es el código de ejemplo para crear una cola de Service Bus. Para ver un ejemplo completo, consulte el [ejemplo de administración de .NET en GitHub](https://github.com/Azure-Samples/service-bus-dotnet-management/). 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.azure.com/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

## <a name="fluent-library"></a>Biblioteca de Fluent
Para obtener un ejemplo del uso de la biblioteca de Fluent para administrar entidades de Service Bus, vea [este ejemplo](https://github.com/Azure/azure-libraries-for-net/tree/master/Samples/ServiceBus). 

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes temas de referencia: 

- [Azure.Messaging.ServiceBus.Administration](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient)
- [Microsoft.Azure.ServiceBus.Management](/dotnet/api/microsoft.azure.servicebus.management.managementclient)
- [Microsoft.Azure.Management.ServiceBus](/dotnet/api/microsoft.azure.management.servicebus.servicebusmanagementclient)
- [Fluent](/dotnet/api/microsoft.azure.management.servicebus.fluent)