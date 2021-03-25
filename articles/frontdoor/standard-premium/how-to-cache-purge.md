---
title: Purga de la caché en Azure Front Door Estándar/Prémium (versión preliminar)
description: Este artículo le ayuda a entender cómo purgar la caché en Azure Front Door Estándar/Premium.
services: frontdoor
author: duongau
manager: KumudD
ms.service: frontdoor
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: aacbf2ceab8580727b1885bf6533cd74a7c4e60a
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098187"
---
# <a name="cache-purging-in-azure-front-door-standardpremium-preview"></a>Purga de la caché en Azure Front Door Estándar/Prémium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

Azure Front Door Estándar/Premium almacena en caché los recursos hasta que el período de vida de esos recursos expira. Siempre que un cliente solicita un recurso con un período de vida expirado, el entorno de Azure Front Door recupera una nueva copia actualizada de este para atender la solicitud y almacena en caché la versión actualizada.

El procedimiento recomendado consiste en asegurarse de que los usuarios obtengan siempre la copia más reciente de los recursos. La manera de lograrlo es hacer una versión de los recursos para cada actualización y publicarlos como nuevas direcciones URL. Azure Front Door Estándar/Premium recuperará inmediatamente los nuevos recursos en las siguientes solicitudes de los clientes. A veces puede que quiera purgar contenido almacenado en caché de todos los nodos perimetrales y forzarlos todos para recuperar nuevos recursos actualizados. La razón por la que desea purgar el contenido almacenado en caché es porque ha realizado nuevas actualizaciones en la aplicación o desea actualizar los recursos que contienen información incorrecta.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

Repase el [almacenamiento en caché de Azure Front Door](concept-caching.md) para comprender cómo funciona el almacenamiento en caché.

## <a name="configure-cache-purge"></a>Configuración de la purga de la memoria caché

1. Vaya a la página de información general del perfil de Azure Front Door con los recursos que desea purgar y seleccione **Purgar caché**.

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-1.png" alt-text="Captura de pantalla de la página información general de purga de la caché.":::

1. Seleccione el punto de conexión y el dominio que quiere purgar de los nodos perimetrales. *(Puede seleccionar más de un dominio)*

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-2.png" alt-text="Captura de pantalla de la página de purga de la caché.":::

1. Para borrar todos los recursos, seleccione **Purgar todos los recursos de los dominios seleccionados**. De lo contrario, en **Rutas de acceso**, escriba la ruta de acceso de cada recurso que quiera purgar.

Estos formatos se admiten en las listas de rutas de acceso que se van a purgar:

* **Purga de ruta de acceso única**: purgue recursos concretos mediante la especificación de la ruta de acceso completa del recurso (sin el protocolo y el dominio) con la extensión de archivo, por ejemplo, /pictures/strasbourg.png.
* **Purga de dominio raíz**: purgue la raíz del punto de conexión con "/*" en la ruta de acceso.

Las purgas de la memoria caché en Azure Front Door Estándar/Premium no distinguen mayúsculas y minúsculas. Además, es independiente de la cadena de consulta, lo que significa que purgar una dirección URL purgará todas sus variaciones de la cadena de consulta. 

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [crear una instancia de Front Door Estándar/Premium](create-front-door-portal.md).
