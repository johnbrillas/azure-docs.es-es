---
title: Solución del error 403004 DeviceMaximumQueueDepthExceeded de Azure IoT Hub
description: Corrección del error 403004 DeviceMaximumQueueDepthExceeded
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 421066ef30e23a79b26f97939cdfffb5be83afb5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92148233"
---
# <a name="403004-devicemaximumqueuedepthexceeded"></a>403004 DeviceMaximumQueueDepthExceeded

En este artículo se describen las causas y las soluciones de los errores **403004 DeviceMaximumQueueDepthExceeded**.

## <a name="symptoms"></a>Síntomas

Al intentar enviar un mensaje de la nube al dispositivo, se produce en la solicitud un error **403004** o **DeviceMaximumQueueDepthExceeded**.

## <a name="cause"></a>Causa

La causa subyacente es que el número de mensajes puestos en cola para el dispositivo supera el [límite de la cola (50)](./iot-hub-devguide-quotas-throttling.md#other-limits).

La razón más probable de que se encuentre en este límite es que usa HTTPS para recibir el mensaje, lo que conduce al sondeo continuo mediante `ReceiveAsync` y ello da lugar a que IoT Hub limite la solicitud.

## <a name="solution"></a>Solución

El patrón admitido para los mensajes de nube a dispositivo con HTTP es dispositivos conectados de forma intermitente que buscan mensajes con poca frecuencia (menos de cada 25 minutos). Para reducir la probabilidad de que se encuentre en el límite de la cola, cambie a AMQP o a MQTT para los mensajes de la nube al dispositivo.

O bien, mejore la lógica del lado del dispositivo para completar, rechazar o abandonar los mensajes en cola rápidamente o acortar el período de vida, o considere la posibilidad de enviar menos mensajes. Consulte [Período de vida de un mensaje C2D](./iot-hub-devguide-messages-c2d.md#message-expiration-time-to-live).

Por último, considere el uso de la [API para purgar la cola](/azure/iot-hub/iot-c-sdk-ref/iothub-registrymanager-h/iothubregistrymanager-deletedevice) a fin de limpiar periódicamente los mensajes pendientes antes de que se alcance el límite.