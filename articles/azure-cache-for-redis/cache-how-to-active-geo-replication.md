---
title: Configuración de la replicación geográfica activa para las instancias de Azure Cache for Redis Enterprise
description: Aprenda a replicar las instancias de Azure Cache for Redis Enterprise entre regiones de Azure.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.author: yegu
ms.openlocfilehash: d52998e477e44f89a35a70f7e2b5d49cbc68e386
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660488"
---
# <a name="configure-active-geo-replication-for-enterprise-azure-cache-for-redis-instances-preview"></a>Configuración de la replicación geográfica activa para las instancias de Azure Cache for Redis Enterprise (versión preliminar)

En este artículo, aprenderá a configurar una instancia de Azure Cache con replicación geográfica activa mediante Azure Portal.

La replicación geográfica activa agrupa dos o más instancias de Azure Cache for Redis Enterprise en una sola caché que abarca varias regiones de Azure. Todas las instancias actúan como instancia principal local. Una aplicación decide qué instancias se van a utilizar para las solicitudes de lectura y escritura.

## <a name="create-or-join-an-active-geo-replication-group"></a>Creación de un grupo de replicación geográfica activa o unión con él

> [!IMPORTANT]
> La replicación geográfica activa debe estar habilitada en el momento en que se crea una caché de Azure Cache for Redis.
>
>

1. En la interfaz de usuario de creación de **Nueva caché en Redis**, haga clic en **Configurar** para configurar la **Replicación geográfica activa** en la pestaña **Opciones avanzadas**.

    ![Configuración de replicación geográfica activa](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-not-configured.png)

1. Cree un nuevo grupo de replicación, para una primera instancia de caché, o seleccione uno existente en la lista.

    ![Vincular cachés](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-new-group.png)

1. Haga clic en **Configurar** para finalizar.

    ![Vincular cachés](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-configured.png)

1. Repita los pasos anteriores para cada instancia de caché adicional en el grupo de replicación geográfica.

## <a name="remove-from-an-active-geo-replication-group"></a>Eliminación de un grupo de replicación geográfica activa

Para quitar una instancia de caché de un grupo de replicación geográfica activa, solo tiene que eliminar la instancia. Las instancias restantes se volverán a configurar automáticamente.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las características de Azure Cache for Redis.

* [Niveles de servicio de Azure Cache for Redis](cache-overview.md#service-tiers)
* [Alta disponibilidad en Azure Cache for Redis](cache-high-availability.md)
