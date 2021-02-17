---
title: Incorporación a la solución basada en agente Defender para IoT
description: Aprenda a incorporar y habilitar el servicio Defender para seguridad de IoT en Azure IoT Hub.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/20/2021
ms.author: shhazam
ms.openlocfilehash: 127e439a7740cb97cbe126071aaaa5245cd85782
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809140"
---
# <a name="onboard-to-defender-for-iot-agent-based-solution"></a>Incorporación a la solución basada en agente Defender para IoT

En este artículo se explica cómo habilitar el servicio Defender para IoT en la instancia existente de IoT Hub. Si actualmente no tiene una instancia de IoT Hub, consulte [Creación de una instancia de IoT Hub mediante Azure Portal](../iot-hub/iot-hub-create-through-portal.md) para comenzar.

Puede administrar la seguridad de IoT mediante IoT Hub en Defender para IoT. El portal de administración ubicado en IoT Hub le permite hacer lo siguiente: 

- Administración de la seguridad de IoT Hub.

- Administración básica de la seguridad de un dispositivo IoT sin necesidad de instalar un agente basado en la telemetría de IoT Hub. 

- Administración avanzada para la seguridad de un dispositivo IoT basado en el microagente.

> [!NOTE]
> Actualmente, Defender para IoT solo admite el nivel Estándar de las instancias de IoT Hub.

## <a name="onboard-to-defender-for-iot-in-iot-hub"></a>Incorporación de Defender para IoT en IoT Hub

Para todos los nuevos centros de IoT, Defender para IoT se establece en **Activado** de forma predeterminada. Puede comprobar que Defender para IoT está **Activado** durante el proceso de creación de IoT Hub.

Para comprobar si está **Activado**:

1. Acceda a Azure Portal.

1. Seleccione **IoT Hub** en la lista de servicios de Azure.

1. Seleccione **Crear**.

    :::image type="content" source="media/quickstart-onboard-iot-hub/create-iot-hub.png" alt-text="Seleccione el botón Crear en la barra de herramientas superior." lightbox="media/quickstart-onboard-iot-hub/create-iot-hub-expanded.png":::

1. Seleccione la pestaña **Administración** y compruebe que el botón de alternancia de **Defender para IoT** está establecido en **Activado**.

    :::image type="content" source="media/quickstart-onboard-iot-hub/management-tab.png" alt-text="Asegúrese de que el botón de alternancia de Defender para IoT esté establecido en Activado.":::

## <a name="onboard-defender-for-iot-to-an-existing-iot-hub"></a>Incorporación de Defender para IoT a una instancia de IoT Hub existente

Para supervisar la administración de identidades de dispositivo y los patrones de comunicación entre el dispositivo y la nube y viceversa, realice las siguientes acciones para iniciar el servicio: 

1. Vaya a IoT Hub. 

1. Seleccione el menú **Información general sobre seguridad**. 

1. Haga clic en Secure your IoT solution (Proteger la solución de IoT) y complete el formulario de incorporación. 


## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para configurar una solución...

> [!div class="nextstepaction"]
> [Creación de un módulo gemelo del microagente de Defender para IoT (versión preliminar)](quickstart-create-micro-agent-module-twin.md)
