---
title: Información general de Conector de Administración de servicios de TI
description: En este artículo se proporciona información general de Conector de Administración de servicios de TI (ITSMC).
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/16/2020
ms.custom: references_regions
ms.openlocfilehash: 93b6160b495b609e23e10b3f709d130792067423
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803789"
---
# <a name="it-service-management-connector-overview"></a>Información general de Conector de Administración de servicios de TI

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

El Conector de Administración de servicios de TI (ITSMC) le permite conectar Azure a un producto o servicio de Administración de servicios de TI (ITSM) compatibles.

Servicios de Azure como Azure Log Analytics y Azure Monitor proporcionan herramientas para detectar, analizar y solucionar problemas relacionados con los recursos de Azure y de otros proveedores. Sin embargo, los elementos de trabajo relacionados con un problema suelen residir en un producto o servicio de ITSM. ITSMC proporciona una conexión bidireccional entre las herramientas de Azure e ITSM para ayudarle a resolver problemas con mayor rapidez.

## <a name="configuration-steps"></a>Pasos de configuración

ITSMC admite conexiones con las siguientes herramientas ITSM:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

   >[!NOTE]
> Proponemos a nuestros clientes de Cherwell y Provance que utilicen la [acción de Webhook](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#webhook) en el punto de conexión a Cherwell y Provance como otra solución para la integración.

Con ITSMC, puede:

-  Crear elementos de trabajo en la herramienta de ITSM, en función de las alertas de Azure (alertas de métricas, de registro de actividad y de Log Analytics).
-  Sincronizar de forma opcional los datos de incidentes y solicitudes de cambio de la herramienta ITSM con un área de trabajo de Azure Log Analytics.

Para obtener información sobre los términos legales y la directiva de privacidad, vea la [declaración de privacidad de Microsoft](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Para empezar a usar ITSMC, siga estos pasos:

1. [Configure el entorno ITSM para aceptar alertas de Azure.](./itsmc-connections.md)
1. [Configuración de la solución ITSM de Azure](./itsmc-definition.md#add-it-service-management-connector)
1. [Configure el conector ITSM de Azure para el entorno ITSM.](./itsmc-definition.md#create-an-itsm-connection)
1. [Configure el grupo de acciones para aprovechar el conector ITSM.](./itsmc-definition.md#use-itsmc)

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)