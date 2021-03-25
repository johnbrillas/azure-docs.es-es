---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 10/01/2020
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 071f2849a877f4ea1e8a84eff6ccfb8343be3ec7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101734071"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Alertas de métricas (clásico) |100 reglas de alertas activas por suscripción. | Llame al soporte técnico. |
| Alertas de métricas |5000 reglas de alertas activas por suscripción tanto en la nube pública de Azure, como en las nubes de Azure China 21Vianet y Azure Government. Si alcanza este límite, examine si puede usar [alertas de varios recursos del mismo tipo](../articles/azure-monitor/alerts/alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor).<br/>5000 series temporales de métricas por regla de alertas. | Llame al soporte técnico. |
| Alertas de registros de actividad | 100 reglas de alerta activas por suscripción (este número no se puede aumentar). | Igual que el predeterminado. |
| Alertas de registro | 512 reglas de alertas activas por suscripción. 200 reglas de alertas activas por recurso. | Llame al soporte técnico. |
| Longitud de la descripción de las reglas de alertas y las reglas de acción| Alertas de búsqueda de registros: 4096 caracteres<br/>Todas las demás, 2048 caracteres | Igual que el predeterminado. |