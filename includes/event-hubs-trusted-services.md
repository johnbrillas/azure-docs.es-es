---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 01/05/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 743d7d46474b4ba55df50398f29cbdb964b5ff08
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97978902"
---
## <a name="trusted-microsoft-services"></a>Servicios de Microsoft de confianza
Al activar la opción **¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?** , los siguientes servicios adquieren acceso a los recursos de Event Hubs.

| Servicio de confianza | Escenarios de uso admitidos | 
| --------------- | ------------------------- | 
| Azure Event Grid | Permite que Azure Event Grid envíe eventos a Event Hubs en el espacio de nombres de Event Hubs. También debe seguir estos pasos: <ul><li>Habilite la identidad asignada por el sistema para un tema o un dominio</li><li>Agregue la identidad al rol Remitente de datos de Azure Event Hubs en el espacio de nombres de Event Hubs</li><li>A continuación, configure la suscripción de eventos que emplea un centro de eventos como punto de conexión para usar la identidad asignada por el sistema.</li></ul> <p>Para obtener más información, vea [Entrega de evento con una identidad administrada](../articles/event-grid/managed-service-identity.md)</p>|
| Azure Monitor (configuración de diagnósticos) | Permite que Azure Monitor envíe información de diagnóstico a Event Hubs en el espacio de nombres de Event Hubs. Azure Monitor puede leer desde el centro de eventos y escribir datos en el centro de eventos. |
| Azure Stream Analytics | Permite que un trabajo de Azure Stream Analytics lea datos de centros de eventos del espacio de nombres de Event Hubs ([entrada](../articles/stream-analytics/stream-analytics-add-inputs.md)) o escriba datos en ellos ([salida](../articles/stream-analytics/event-hubs-output.md)). <p>**Importante**: El trabajo de Stream Analytics debe estar configurado para usar una **identidad administrada** para tener acceso al centro de eventos. Para obtener más información, consulte [Uso de identidades administradas para acceder a Event Hubs desde un trabajo de Azure Stream Analytics (versión preliminar)](../articles/stream-analytics/event-hubs-managed-identity.md). </p>|
| Azure IoT Hub | Permite que IoT Hub envíe mensajes a centros de eventos del espacio de nombres de Event Hubs. También debe seguir estos pasos: <ul><li>Habilitación de la identidad asignada por el sistema para el centro de IoT</li><li>Agregue la identidad al rol Remitente de datos de Azure Event Hubs en el espacio de nombres de Event Hubs.</li><li>A continuación, configure la instancia de IoT Hub que usa un centro de eventos como punto de conexión personalizado para utilizar la autenticación basada en identidad.</li></ul>
