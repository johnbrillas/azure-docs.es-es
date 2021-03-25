---
title: IT Service Management Connector en Azure Monitor
description: En este artículo, se proporciona información sobre cómo conectar los productos y servicios de ITSM al Conector de Administración de servicios de TI (ITSMC) en Azure Monitor para supervisar y administrar de forma centralizada los elementos de trabajo de ITSM.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 40e737a1ec5fb34cd22a08925143a100d36cdc6b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103009324"
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

## <a name="ip-ranges-for-itsm-partners-connections"></a>Intervalos IP para conexiones de asociados de ITSM
Para obtener una lista de las direcciones IP de ITSM con el fin de permitir las conexiones de ITSM desde las herramientas ITSM de los asociados, se recomienda que muestre el intervalo IP público completo de la región de Azure a la que pertenece el área de trabajo de Log Analytics. [Detalles aquí](https://www.microsoft.com/en-us/download/details.aspx?id=56519) En las regiones EUS/WEU/EUS2/WUS2/Centro y Sur de EE. UU., el cliente solo puede enumerar la etiqueta de red ActionGroup.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)
