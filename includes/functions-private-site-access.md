---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 3c9679f3d66d58c7a6c847b54c84438c979ecd39
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936873"
---
Un [punto de conexión privado de Azure](../articles/private-link/private-endpoint-overview.md) es una interfaz de red que nos conecta de forma privada y segura a un servicio por medio de la tecnología Azure Private Link.  Los puntos de conexión privados emplean una dirección IP privada de la red virtual, lo que hace que el servicio se incluya en la red virtual.

Puede usar el punto de conexión privado para las funciones hospedadas en los planes [Premium](../articles/azure-functions/functions-premium-plan.md) y [App Service](../articles/azure-functions/dedicated-plan.md).

Al crear una conexión de punto de conexión privado entrante para las funciones, también necesitará un registro DNS para resolver la dirección privada.  De manera predeterminada, se creará automáticamente un registro DNS privado al crear un punto de conexión privado mediante Azure Portal.

Para más información, consulte cómo [usar puntos de conexión privados para aplicaciones web](../articles/app-service/networking/private-endpoint.md).