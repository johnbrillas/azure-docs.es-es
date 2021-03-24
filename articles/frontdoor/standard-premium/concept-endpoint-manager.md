---
title: 'Azure Front Door: Endpoint Manager'
description: En este artículo se ofrece información general sobre Endpoint Manager para Azure Front Door.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: c916be9a54d62e16f488c94f4fa88a2207fb8788
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098242"
---
# <a name="what-is-azure-front-door-standardpremium-preview-endpoint-manager"></a>¿Qué es Endpoint Manager para Azure Front Door Estándar/Premium (versión preliminar)?

> [!NOTE]
> * Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Vea la [documentación sobre Azure Front Door](../front-door-overview.md).

Endpoint Manager proporciona una información general de los puntos de conexión que ha configurado para Azure Front Door. Un punto de conexión es una agrupación lógica de dominios y sus configuraciones asociadas. Endpoint Manager le ayuda a administrar la colección de puntos de conexión de la operación CRUD (creación, lectura, actualización y eliminación). Puede administrar los siguientes elementos de los puntos de conexión mediante Endpoint Manager:

* Dominios
* Grupos de origen
* Rutas
* Seguridad

:::image type="content" source="../media/concept-endpoint-manager/endpoint-manager-1.png" alt-text="Captura de pantalla de Endpoint Manager sin configuraciones." lightbox="../media/concept-endpoint-manager/endpoint-manager-1-expanded.png":::

Endpoint Manager muestra el número de instancias de cada elemento que se crean dentro de un punto de conexión. También se mostrará el estado de asociación de cada elemento. Por ejemplo, puede crear varios dominios y grupos de origen, y asignar la asociación entre ellos con diferentes rutas.

> [!IMPORTANT]
> * Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [**Condiciones de uso complementarias de las versiones preliminares de Microsoft Azure**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="linked-view"></a>Vista vinculada

Con la vista vinculada en Endpoint Manager, puede identificar fácilmente la asociación entre los elementos de Azure Front Door como, por ejemplo:

* ¿Qué dominios están asociados al punto de conexión actual?
* ¿Qué grupo de origen está asociado a qué dominio?
* ¿Qué directiva del firewall de aplicaciones web está asociada a cada dominio?

:::image type="content" source="../media/concept-endpoint-manager/endpoint-manager-2.png" alt-text="Captura de pantalla de Endpoint Manager con configuraciones." lightbox="../media/concept-endpoint-manager/endpoint-manager-2-expanded.png":::

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [crear una instancia de Front Door Estándar/Premium](create-front-door-portal.md).
