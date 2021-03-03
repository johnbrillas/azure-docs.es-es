---
title: IT Service Management Connector en Azure Monitor
description: En este artículo, se proporciona información sobre cómo conectar los productos y servicios de ITSM al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: a372cdcd05267f3bdb093f676948a79c473ad955
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734781"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Conectar productos o servicios de ITSM con el Conector de Administración de servicios de TI
En este artículo se proporciona información sobre cómo configurar la conexión entre los productos y servicios de ITSM y el Conector de Administración de servicios de TI (ITSMC) en Log Analytics para administrar de forma centralizada los elementos de trabajo. Para obtener más información sobre ITSMC, vea [Información general](./itsmc-overview.md).

Se admiten los siguientes productos y servicios de ITSM. Seleccione un producto para ver información detallada sobre cómo conectarlo a ITSMC.

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

> [!NOTE]
> Proponemos a nuestros clientes de Cherwell y Provance que utilicen la [acción de Webhook](./action-groups.md#webhook) en el punto de conexión a Cherwell y Provance como otra solución para la integración.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)