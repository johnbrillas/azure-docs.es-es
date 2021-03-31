---
title: Introducción a Azure IoT industrial
description: En este artículo se proporciona información general de IoT industrial. En él se explican los componentes de seguridad y conectividad de la zona de producción en IIoT.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: overview
ms.date: 3/22/2021
ms.openlocfilehash: 940391d26b5a8455fef01c8094cd08e05ab51290
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104787480"
---
# <a name="what-is-industrial-iot-iiot"></a>¿Qué es IoT industrial (IIoT)?

IIoT (Internet de las cosas industrial) mejora la eficiencia industrial gracias a la aplicación de IoT al sector de fabricación.

![IoT industrial](media/overview-what-is-Industrial-IoT/icon-255-px.png)

## <a name="improve-industrial-efficiencies"></a>Mejora de la eficiencia industrial
Mejore la productividad operativa y la rentabilidad con Azure IoT industrial. Reduzca el proceso lento de acceder a los recursos in situ. Conecte y supervise sus equipos y dispositivos industriales en la nube, incluidas sus máquinas que ya funcionan en la planta de producción. Analice sus datos de IoT para extraer conclusiones que le ayuden a aumentar el rendimiento del sitio entero.

## <a name="industrial-iot-components"></a>Componentes de IoT industrial

**Dispositivos IoT Edge**: un dispositivo IoT Edge se compone del entorno de ejecución de Edge y de módulos de Edge. 
- Los *módulos de Edge* son contenedores de Docker, que son la unidad de cálculo más pequeña, como OPC Publisher y OPC Twin. 
- El *dispositivo de Edge* se usa para implementar estos módulos, que actúan como mediadores entre el servidor de OPC UA e IoT Hub en la nube. Puede encontrar más información sobre IoT Edge [aquí](https://azure.microsoft.com/services/iot-edge/).

**IoT Hub** actúa como centro de mensajes común para la comunicación bidireccional entre la aplicación de IoT y los dispositivos que administra. Es una plataforma como servicio, de nube, abierta y flexible que admite los SDK de código abierto y diversos protocolos. Puede encontrar más información sobre IoT Hub [aquí](https://azure.microsoft.com/services/iot-hub/).

**Módulos de Edge industrial**
- *OPC Publisher*: OPC Publisher se ejecuta dentro de IoT Edge. Se conecta a los servidores de OPC UA y publica los datos de telemetría codificados en JSON desde estos servidores en formato "pub/sub" de OPC UA para Azure IoT Hub. Se pueden usar todos los protocolos de transporte admitidos por el SDK de cliente de Azure IoT Hub, es decir, HTTPS, AMQP y MQTT.
- *OPC Twin*: consta de microservicios que usan Azure IoT Edge e IoT Hub para conectar la nube y la red de fábrica. OPC Twin proporciona la detección, el registro y el control remoto de dispositivos industriales mediante las API REST. OPC Twin no requiere un SDK de OPC Unified Architecture (OPC UA). Es independiente del lenguaje de programación y puede incluirse en un flujo de trabajo sin servidor.
- *Detección*: el módulo de detección, representado por la identidad del programa de detección, proporciona servicios de detección en el perímetro, que incluyen la detección de servidores de OPC UA. Si la detección está configurada y habilitada, el módulo enviará los resultados de un sondeo de examen a través de la ruta de acceso de telemetría de IoT Edge e IoT Hub al servicio de incorporación. El servicio procesa los resultados y actualiza todas las identidades relacionadas en el Registro.


**Detección, registro y administración de recursos industriales con Azure**: Azure IoT industrial permite a los operarios de planta detectar los servidores habilitados para OPC UA en una red de fábrica y registrarlos en Azure IoT Hub. El personal de operaciones puede suscribirse y reaccionar a eventos en la planta de producción desde cualquier lugar del mundo. Las API REST de microservicios reflejan el lado perimetral de los servicios de OPC UA. Están protegidas con la autenticación y la autorización de OAUTH respaldadas por Azure Active Directory (AAD). De esta forma, las aplicaciones en la nube pueden examinar los espacios de direcciones de servidor o las variables de lectura y escritura, y ejecutar métodos mediante HTTPS y cargas JSON sencillas de OPC UA.

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido qué es IoT industrial, puede leer sobre la plataforma de IoT industrial y OPC Publisher:

> [!div class="nextstepaction"]
> [¿Qué es la plataforma de IoT industrial?](overview-what-is-industrial-iot-platform.md)

> [!div class="nextstepaction"]
> [¿Qué es OPC Publisher?](overview-what-is-opc-publisher.md)