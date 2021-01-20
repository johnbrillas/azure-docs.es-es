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
ms.openlocfilehash: 963e49ea0e5536be0fca6d565b439aef4a08793d
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98605111"
---
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Inserción de aplicación de Azure | 10 acciones de aplicación de Azure por grupo de acciones. | Igual que el predeterminado |
| Email | 1000 acciones de correo electrónico en un grupo de acciones.<br>no más de 100 mensajes de correo electrónico en una hora.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Igual que el predeterminado |
| ITSM | 10 acciones de ITSM en un grupo de acciones. | Igual que el predeterminado | 
| Aplicación lógica | 10 acciones de aplicación lógica en un grupo de acciones. | Igual que el predeterminado |
| Runbook | 10 acciones de runbook en un grupo de acciones. | Igual que el predeterminado |
| sms | 10 acciones de SMS en un grupo de acciones.<br>No más de 1 mensaje de texto cada 5 minutos.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Igual que el predeterminado |
| Voz | 10 acciones de voz en un grupo de acciones.<br>No más de 1 llamada de voz cada 5 minutos.<br>Consulte también el artículo [sobre las limitaciones de velocidad](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Igual que el predeterminado |
| webhook | 10 acciones de webhook en un grupo de acciones.  El número máximo de llamadas de webhook es 1500 por minuto y suscripción. Están disponibles otros límites en la [información específica de la acción](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Igual que el predeterminado |
