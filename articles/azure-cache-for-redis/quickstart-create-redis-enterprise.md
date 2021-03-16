---
title: 'Inicio rápido: Creación de una caché de Redis Enterprise'
description: En este inicio rápido, aprenderá a crear una instancia de Azure Cache for Redis en los niveles Enterprise
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 02/08/2021
ms.openlocfilehash: 63daa7cda2f10448e9a0f8e250412137dc2af892
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563904"
---
# <a name="quickstart-create-a-redis-enterprise-cache"></a>Inicio rápido: Creación de una caché de Redis Enterprise

Los niveles Enterprise de Azure Cache for Redis proporcionan una instancia de [Redis Enterprise](https://redislabs.com/redis-enterprise/) totalmente integrada y administrada en Azure. Actualmente están disponibles en forma de versión preliminar. Hay dos nuevos niveles en esta versión preliminar:
* Enterprise, que usa memoria volátil (DRAM) en una máquina virtual para almacenar datos.
* Enterprise Flash, que usa memoria volátil y no volátil (NVMe o SSD) para almacenar los datos.

## <a name="prerequisites"></a>Prerrequisitos

Necesitará una suscripción a Azure antes de empezar. Si aún no la tiene, cree una [cuenta](https://azure.microsoft.com/). Para más información, consulte [Consideraciones especiales para los niveles Enterprise](cache-overview.md#special-considerations-for-enterprise-tiers).

## <a name="create-a-cache"></a>Creación de una caché
1. Para crear una caché, inicie sesión en Azure Portal mediante el vínculo de la invitación a la versión preliminar y seleccione **Crear un recurso**.

1. En la página **Nuevo**, seleccione **Base de datos** y, a continuación, seleccione **Azure Cache for Redis**.
   
   :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Selección de Azure Cache for Redis":::
   
1. En la página **Nueva instancia de Redis Cache**, configure las opciones de la nueva caché.
   
   | Configuración      | Valor sugerido  | Descripción |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Suscripción** | Desplácese hacia abajo y seleccione su suscripción. | La suscripción en la que se creará esta nueva instancia de Azure Cache for Redis. | 
   | **Grupos de recursos** | Desplácese hacia abajo y seleccione un grupo de recursos o **Crear nuevo** y escriba un nombre nuevo para el grupo de recursos. | Nombre del grupo de recursos en el que se van a crear la caché y otros recursos. Al colocar todos los recursos de la aplicación en un grupo de recursos, puede administrarlos o eliminarlos fácilmente. | 
   | **Nombre DNS** | Escriba un nombre único global. | El nombre de la memoria caché debe ser una cadena de entre 1 y 63 caracteres, y solo puede contener números, letras o guiones. El nombre debe comenzar y terminar por un número o una letra y no puede contener guiones consecutivos. El *nombre de host* de la instancia de caché será *\<DNS name>.<Azure region>.redisenterprise.cache.azure.net*. | 
   | **Ubicación** | Desplácese hacia abajo y seleccione una ubicación. | Los niveles Enterprise están disponibles en regiones limitadas de Azure durante la versión preliminar. |
   | **Tipo de caché** | Despliegue el menú y seleccione el nivel *Enterprise* o *Enterprise Flash* y un tamaño. |  El nivel determina el tamaño, rendimiento y características disponibles para la memoria caché. |
   
   :::image type="content" source="media/cache-create/enterprise-tier-basics.png" alt-text="Pestaña Básico del nivel Enterprise":::

   > [!NOTE] 
   > Asegúrese de activar la casilla situada bajo "Términos" antes de continuar.
   >

1. Seleccione **Siguiente: Redes** y omítalo.

1. Seleccione **Siguiente: Avanzado** y establezca **Directiva de agrupación en clústeres** en **Enterprise**. Habilite **Non-TLS access only** (Solo acceso no TLS) si tiene previsto conectarse a la nueva memoria caché sin usar TLS. Sin embargo, esto no se recomienda.

   :::image type="content" source="media/cache-create/enterprise-tier-advanced.png" alt-text="Pestaña Avanzado del nivel Enterprise":::

1. Seleccione **Siguiente: Etiquetas** y omítalo.

1. Seleccione **Siguiente: Review + create** (Revisar y crear).

   :::image type="content" source="media/cache-create/enterprise-tier-summary.png" alt-text="Pestaña Revisar y crear del nivel Enterprise":::

1. Revise la configuración y haga clic en **Crear**.
   
   La caché tarda un tiempo en crearse. Puede supervisar el progreso en la página **Información general** de Azure Cache for Redis. Cuando **Estado** se muestra como **En ejecución**, la memoria caché está lista para su uso.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, aprenderá a crear una instancia de Azure Cache for Redis con el nivel Enterprise.

> [!div class="nextstepaction"]
> [Creación de una aplicación web ASP.NET que usa Azure Redis Cache.](./cache-web-app-howto.md)

