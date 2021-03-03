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
ms.openlocfilehash: 6f9405e1b402b029b628821824a1727dd825a031
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734051"
---
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Inserción de aplicación de Azure | 10 acciones de aplicación de Azure por grupo de acciones. | Igual que el predeterminado |
| Email | 1000 acciones de correo electrónico en un grupo de acciones.<br>no más de 100 mensajes de correo electrónico en una hora.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Igual que el predeterminado |
| ITSM | 10 acciones de ITSM en un grupo de acciones. | Igual que el predeterminado | 
| Aplicación lógica | 10 acciones de aplicación lógica en un grupo de acciones. | Igual que el predeterminado |
| Runbook | 10 acciones de runbook en un grupo de acciones. | Igual que el predeterminado |
| sms | 10 acciones de SMS en un grupo de acciones.<br>No más de 1 mensaje de texto cada 5 minutos.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Igual que el predeterminado |
| Voz | 10 acciones de voz en un grupo de acciones.<br>No más de 1 llamada de voz cada 5 minutos.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Igual que el predeterminado |
| webhook | 10 acciones de webhook en un grupo de acciones.  El número máximo de llamadas de webhook es 1500 por minuto y suscripción. Están disponibles otros límites en la [información específica de la acción](../articles/azure-monitor/alerts/action-groups.md#action-specific-information).  | Igual que el predeterminado |