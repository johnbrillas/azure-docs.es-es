---
title: Solución de problemas del error 404103 de Azure IoT Hub DeviceNotOnline
description: Corrección del error 404103 DeviceNotOnline
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: e648428f924cfc33421c8591c41f7ac85b05a033
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "76960301"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

En este artículo se describen las causas y las soluciones de los errores **404103 DeviceNotOnline**.

## <a name="symptoms"></a>Síntomas

Un método directo para un dispositivo produce un error **404103 DeviceNotOnline** aunque este esté en línea. 

## <a name="cause"></a>Causa

Si está seguro de que el dispositivo está en línea y sigue apareciendo el error, es probable que la devolución de llamada del método directo no esté registrada en el dispositivo.

## <a name="solution"></a>Solución

Para configurar el dispositivo correctamente para las devoluciones de llamada de método directo, consulte [Control de un método directo en un dispositivo](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device).