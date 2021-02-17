---
title: 'Inicio rápido: Configuración y habilitación de un módulo de seguridad para Azure RTOS'
description: Aprenda a incorporar y habilitar el módulo de seguridad para el servicio Azure RTOS en Azure IoT Hub.
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
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: 3054981bbbff45666297399033663d1830ad9e4e
ms.sourcegitcommit: 2501fe97400e16f4008449abd1dd6e000973a174
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99820439"
---
# <a name="quickstart-security-module-for-azure-rtos-preview"></a>Inicio rápido: Módulo de seguridad para Azure RTOS (versión preliminar)

En este artículo se proporciona una explicación de los requisitos previos al comienzo y se explica cómo habilitar el módulo de seguridad para el servicio Azure RTOS en IoT Hub. Si actualmente no tiene una instancia de IoT Hub, consulte [Creación de una instancia de IoT Hub mediante Azure Portal](../iot-hub/iot-hub-create-through-portal.md) para comenzar.

## <a name="prerequisites"></a>Requisitos previos 

### <a name="supported-devices"></a>Dispositivos compatibles

- Kit de detección ST STM32F746G
- NXP i.MX RT1060 EVK
- Microchip SAM E54 Xplained Pro EVK

Descargue, compile y ejecute uno de los archivos .zip para la placa y la herramienta específicas (IAR, semi IDE o PC) que prefiera del [recurso Security Module for Azure RTOS de GitHub](https://github.com/azure-rtos/azure-iot-preview/releases).

### <a name="azure-resources"></a>Recursos de Azure

La siguiente fase de introducción es preparar los recursos de Azure. Necesitará un centro de IoT y, preferiblemente, un área de trabajo de Log Analytics. Para el centro de IoT, necesitará la cadena de conexión de IoT Hub para conectarse al dispositivo. 
  
### <a name="iot-hub-connection"></a>Conexión de IoT Hub

Se necesita una conexión de IoT Hub para comenzar. 

1. Abra **IoT Hub** en Azure Portal.

1. Vaya a **Dispositivos IoT**.

1. Seleccione **Crear**.

1. Copie la cadena de conexión de IoT en el [archivo de configuración](how-to-azure-rtos-security-module.md).

Las credenciales de conexión se toman de la configuración **HOST_NAME**, **DEVICE_ID** y **DEVICE_SYMMETRIC_KEY** de la aplicación de usuario.

El módulo de seguridad para Azure RTOS usa conexiones de middleware de IoT de Azure basadas en el protocolo **MQTT**.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el siguiente artículo para finalizar la configuración y la personalización de la solución.

> [!div class="nextstepaction"]
> [Configuración de Security Module for Azure RTOS](how-to-azure-rtos-security-module.md)