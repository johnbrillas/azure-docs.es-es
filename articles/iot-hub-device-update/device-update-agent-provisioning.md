---
title: Aprovisionamiento de Device Update para Azure IoT Hub Agent | Microsoft Docs
description: Aprovisionamiento de Device Update para Azure IoT Hub Agent
author: ValOlson
ms.author: valls
ms.date: 2/16/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: 79bac3f057412f3973121f48cd735f72d0a97d04
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678845"
---
# <a name="device-update-agent"></a>Device Update Agent

Device Update para IoT Hub admite dos formas de actualizaciones: basada en imágenes y basada en paquetes. 

* Las actualizaciones con imágenes proporcionan un mayor nivel de confianza en el estado final del dispositivo. Normalmente, es más fácil replicar los resultados de una actualización basada en imágenes entre un entorno de preproducción y un entorno de producción, ya que no plantea los mismos desafíos que los paquetes y sus dependencias. Debido a su naturaleza atómica, también se puede adoptar fácilmente un modelo de conmutación por error A/B. 
* Las actualizaciones basadas en paquetes son actualizaciones dirigidas que solo modifican una aplicación o un componente específicos en el dispositivo. Esto supone un menor consumo de ancho de banda y ayuda a reducir el tiempo necesario para descargar e instalar la actualización. Normalmente, las actualizaciones basadas en paquetes implican menos tiempo de inactividad de los dispositivos al aplicar una actualización y evitan la sobrecarga asociada a la creación de imágenes. 

Siga los vínculos que se incluyen a continuación para compilar, ejecutar y modificar Device Update Agent.

## <a name="build-the-device-update-agent"></a>Compilación de Device Update Agent

Siga las instrucciones para [compilar](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-build-agent-code.md) Device Update Agent desde el código fuente.

## <a name="run-the-device-update-agent"></a>Ejecución de Device Update Agent

Una vez que el agente se compila correctamente, es el momento de [ejecutarlo](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-run-agent.md).

## <a name="modifying-the-device-update-agent"></a>Modificación de Device Update Agent

Ahora, realice los cambios necesarios para incorporar el agente a la imagen.  Consulte las instrucciones sobre cómo [modificar](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-modify-the-agent-code.m) Device Update Agent.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

Si tiene problemas, revise la [guía de solución de problemas](troubleshoot-device-update.md) de Device Update para IoT Hub como ayuda para resolverlos y recopilar la información necesaria que debe proporcionar a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

Use las imágenes y los archivos binarios pregenerados siguientes para obtener una demostración sencilla de Device Update para IoT Hub.  

[Actualización basada en imagen: Introducción a la imagen Yocto de referencia de Raspberry Pi 3 B+](device-update-raspberry-pi.md)

[Actualización basada en imagen: Introducción al agente de referencia del simulador de Ubuntu (18.04 x64)](device-update-simulator.md)

[Actualización basada en paquete: Introducción al agente de paquetes de Ubuntu Server 18.04 x64](device-update-ubuntu-agent.md)

