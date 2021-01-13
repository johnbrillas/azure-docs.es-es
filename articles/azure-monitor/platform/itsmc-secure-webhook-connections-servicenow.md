---
title: 'Conector de Administración de servicios de TI: exportación segura en Azure Monitor: configuración con ServiceNow'
description: En este artículo se muestra cómo conectar los productos o servicios de ITSM con ServiceNow en Exportación segura de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: fc384ffbc246f5ce9fa84b8161cbc4a5226fa5c8
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97837297"
---
# <a name="connect-servicenow-to-azure-monitor"></a>Conexión de ServiceNow a Azure Monitor

En las secciones siguientes se proporcionan detalles sobre cómo conectar el producto ServiceNow y Exportación segura en Azure.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que se cumplen los requisitos previos siguientes:

* Azure AD está registrado.
* Tiene la versión compatible de ServiceNow Event Management, o ITOM (versión Orlando o posterior).

## <a name="configure-the-servicenow-connection"></a>Configuración de la conexión de ServiceNow

1. Use el vínculo https://(nombre de instancia).service-now.com/api/sn_em_connector/em/inbound_event?source=azuremonitor del URI de la definición de exportación segura.

2. Siga las instrucciones correspondientes a la versión:
   * [París](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Nueva York](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/task/azure-events-authentication.html)