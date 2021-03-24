---
title: Alertas personalizadas de seguridad basadas en agente
titleSuffix: Azure Defender for IoT
description: Obtenga información sobre las alertas de seguridad personalizables y la corrección recomendada mediante las características y el servicio del dispositivo de Defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/16/2021
ms.author: shhazam
ms.openlocfilehash: e885566dd067d70fd4800ca96b8729494464da85
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100641022"
---
# <a name="defender-for-iot-devices-custom-security-alerts"></a>Alertas de seguridad personalizadas de dispositivos de Defender para IoT

Defender para IoT analiza continuamente la solución de IoT mediante análisis avanzados e inteligencia de amenazas para alertarle de cualquier actividad malintencionada.

Se recomienda crear alertas personalizadas en función de su conocimiento sobre el comportamiento esperado del dispositivo. De este modo, se asegura de que las alertas actúan como indicadores eficaces del riesgo potencial en la implementación e infraestructura únicos de la organización.

Las siguientes listas de alertas de Defender para IoT se pueden definir en función del comportamiento esperado de su dispositivo IoT. Para obtener más información acerca de cómo personalizar cada alerta, consulte la [creación de alertas personalizadas](quickstart-create-custom-alerts.md).

## <a name="agent-based-security-custom-alerts"></a>Alertas personalizadas de seguridad basadas en agentes

| severity | Nombre de la alerta | Origen de datos | Descripción | Corrección sugerida |
|--|--|--|--|--|
| Bajo | Alerta personalizada: el número de conexiones activas está fuera del intervalo permitido. | Módulo de seguridad clásico para Azure RTOS | El número de conexiones activas dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente. | Investigue los registros del dispositivo. Averigüe dónde se originó la conexión y determine si es inofensiva o malintencionada. Si es malintencionada, quite el posible malware y reconozca el origen. Si es inofensiva, agregue el origen a la lista de conexiones permitidas. |
| Bajo | Alerta personalizada: se ha creado una conexión saliente a una dirección IP que no está permitida. | Módulo de seguridad clásico para Azure RTOS | Se ha creado una conexión saliente a una dirección IP que está fuera de la lista de direcciones IP permitidas. | Investigue los registros del dispositivo. Averigüe dónde se originó la conexión y determine si es inofensiva o malintencionada. Si es malintencionada, quite el posible malware y reconozca el origen. Si es inofensiva, agregue el origen a la lista de direcciones IP permitidas. |
| Bajo | Alerta personalizada: el número de inicios de sesión locales con errores está fuera del intervalo permitido. | Módulo de seguridad clásico para Azure RTOS | El número de inicios de sesión locales con errores dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente. |  |
| Bajo | Alerta personalizada: el inicio de sesión de un usuario que no está en la lista de usuarios permitidos. | Módulo de seguridad clásico para Azure RTOS | Un usuario local no incluido en la lista de usuarios permitidos ha iniciado sesión en el dispositivo. | Si va a guardar datos sin procesar, vaya a la cuenta de Log Analytics y use los datos para investigar el dispositivo, identifique el origen y, a continuación, corrija la lista de permitidos o bloqueados para esos valores. Si no va a guardar datos sin procesar, vaya al dispositivo y corrija la lista de permitidos o bloqueados para esos valores. |
| Bajo | Alerta personalizada: se ha ejecutado un proceso no permitido. | Módulo de seguridad clásico para Azure RTOS | Se ha ejecutado en el dispositivo un proceso que no se permite. | Si va a guardar datos sin procesar, vaya a la cuenta de Log Analytics y use los datos para investigar el dispositivo, identifique el origen y, a continuación, corrija la lista de permitidos o bloqueados para esos valores. Si no va a guardar datos sin procesar, vaya al dispositivo y corrija la lista de permitidos o bloqueados para esos valores. |
|

## <a name="next-steps"></a>Pasos siguientes

- Averigüe cómo puede [personalizar una alerta](quickstart-create-custom-alerts.md)
- [Información general](overview.md) del servicio Defender para IoT
- Aprenda a [acceder a los datos de seguridad](how-to-security-data-access.md)
- Más información sobre cómo [investigar un dispositivo](how-to-investigate-device.md)
