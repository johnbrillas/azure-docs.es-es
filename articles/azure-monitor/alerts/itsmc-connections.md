---
title: IT Service Management Connector en Azure Monitor
description: En este artículo, se proporciona información sobre cómo conectar los productos y servicios de ITSM al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 046fcb9d7473de5666b3acb25cbcaa1f9549e679
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039502"
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