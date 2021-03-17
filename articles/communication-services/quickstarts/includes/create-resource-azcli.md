---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 6ed6544b8014e973eaf92c763ca18687ad89e5a7
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495910"
---
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Creación de un recurso de Azure Communication Services

Para crear un recurso de Azure Communication Services, [inicie sesión en la CLI de Azure](/cli/azure/authenticate-azure-cli) y ejecute el siguiente comando:

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

Puede configurar el recurso de Communication Services con las opciones siguientes:

* El grupo de recursos
* El nombre del recurso de Communication Services
* La geografía a la que se asociará el recurso

En el paso siguiente, puede asignar etiquetas al recurso. Las etiquetas se pueden usar para organizar los recursos de Azure. Para obtener más información sobre las etiquetas, consulte la [documentación sobre las etiquetas de recursos](../../../azure-resource-manager/management/tag-resources.md).

## <a name="manage-your-communication-services-resource"></a>Administración del recurso de Communication Services

Para agregar etiquetas al recurso de Communication Services, ejecute los siguientes comandos:

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"
```

Para información sobre comandos adicionales, consulte [az communication](/cli/azure/ext/communication/communication).