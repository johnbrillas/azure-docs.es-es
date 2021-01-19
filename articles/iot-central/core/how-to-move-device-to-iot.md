---
title: Traslado de un dispositivo a Azure IoT Central desde IoT Hub
description: Traslado de un dispositivo a Azure IoT Central desde IoT Hub
author: TheRealJasonAndrew
ms.author: v-anjaso
ms.date: 12/20/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 7898f842529b81b80febff444c97b199fbebba3c
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98146428"
---
# <a name="how-to-transfer-a-device-to-azure-iot-central-from-iot-hub"></a>Transferencia de un dispositivo a Azure IoT Central desde IoT Hub

*Este artículo se aplica a los operadores y desarrolladores de dispositivos.*  

En este artículo se describe cómo transferir un dispositivo a una aplicación de Azure IoT Central desde una instancia de IoT Hub. 

Un dispositivo se conecta primero a un punto de conexión de DPS para recuperar la información necesaria para conectarse a la aplicación. De manera interna, la aplicación de IoT Central usa un centro de IoT para administrar la conectividad de los dispositivos.  

Un dispositivo puede conectarse a un centro de IoT directamente mediante una cadena de conexión o mediante DPS. [Azure IoT Hub Device Provisioning Service (DPS)](../../iot-dps/about-iot-dps.md) es la ruta a IoT Central.

## <a name="to-move-the-device"></a>Para mover el dispositivo

Para conectar un dispositivo a IoT Central desde la instancia de IoT Hub, un dispositivo debe actualizarse con:

* El [id. de ámbito](../../iot-dps/concepts-service.md) de la aplicación de IoT Central.
* Una clave derivada de la clave [SAS del grupo](concepts-get-connected.md) o el [certificado X.509](../../iot-hub/iot-hub-x509ca-overview.md).

Para interactuar con IoT Central, debe haber una plantilla de dispositivo que sirva como modelo de las propiedades, la telemetría y los comandos que implementa el dispositivo. Para obtener más información, consulte [Conexión a Azure IoT Central](concepts-get-connected.md) y [¿Qué son las plantillas de dispositivo?](concepts-device-templates.md)

## <a name="next-steps"></a>Pasos siguientes

Si es desarrollador de dispositivos, algunos de los pasos sugeridos son los siguientes:

- Revisar algún código de ejemplo que muestre cómo usar los tokens de SAS en [Tutorial: Creación y conexión de una aplicación cliente a una aplicación de Azure IoT Central](tutorial-connect-device.md)
- Aprender el [procedimiento para conectar dispositivos con certificados X.509 mediante el SDK de dispositivo de Node.js para la aplicación de IoT Central](how-to-connect-devices-x509.md)
- Aprender a [supervisar la conectividad de dispositivos mediante la CLI de Azure](./howto-monitor-devices-azure-cli.md)
- Aprender a [definir un nuevo tipo de dispositivo IoT en la aplicación de Azure IoT Central](./howto-set-up-template.md)
- Leer sobre los [dispositivos de Azure IoT Edge y Azure IoT Central](./concepts-iot-edge.md)