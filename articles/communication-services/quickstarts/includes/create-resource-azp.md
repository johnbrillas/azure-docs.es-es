---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: ab30b19160294b334c8c5abdfbcc1dc0da370f48
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495911"
---
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Creación de un recurso de Azure Communication Services

Para crear un recurso de Azure Communication Services, inicie sesión en [Azure Portal](https://portal.azure.com). En la esquina superior izquierda de la página, seleccione **+ Crear un recurso**. 

:::image type="content" source="../media/create-a-communication-resource/create-resource-plus-sign.png" alt-text="Captura de pantalla que resalta el botón Crear un recurso en Azure Portal.":::

Escriba **Comunicación** en la entrada **Buscar en Marketplace** o en la barra de búsqueda de la parte superior del portal.

:::image type="content" source="../media/create-a-communication-resource/searchbar-communication-portal.png" alt-text="Captura de pantalla que muestra una búsqueda de Communication Services en la barra de búsqueda.":::

Seleccione **Communication Services** en los resultados y, a continuación, elija **Agregar**.

:::image type="content" source="../media/create-a-communication-resource/add-communication-portal.png" alt-text="Captura de pantalla que muestra el panel de Azure y resalta el botón Agregar.":::

Ahora puede configurar el recurso de Communication Services. En la primera página del proceso de creación, se le pedirá que especifique lo siguiente:

* La suscripción
* El grupo de recursos (puede crear uno nuevo o elegir uno existente).
* El nombre del recurso de Communication Services
* La geografía a la que se asociará el recurso

En el paso siguiente, puede asignar etiquetas al recurso. Las etiquetas se pueden usar para organizar los recursos de Azure. Para obtener más información sobre las etiquetas, consulte la [documentación sobre las etiquetas de recursos](../../../azure-resource-manager/management/tag-resources.md).

Por último, puede revisar la configuración y **crear** el recurso. Tenga en cuenta que la implementación tardará unos minutos en completarse.

## <a name="manage-your-communication-services-resource"></a>Administración del recurso de Communication Services

Para administrar el recurso de Communication Services, vaya a [Azure Portal](https://portal.azure.com) y busque y seleccione **Azure Communication Services**.

En la página **Communication Services**, seleccione el nombre del recurso.

La página **Información general** del recurso contiene opciones para la administración básica, como examinar, detener, iniciar, reiniciar y eliminar. Puede encontrar más opciones de configuración en el menú de la izquierda de la página de recursos.