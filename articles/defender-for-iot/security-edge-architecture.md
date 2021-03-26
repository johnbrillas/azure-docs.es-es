---
title: Microagente de Defender para IoT para IoT Edge de Defender para IoT
description: Conozca la arquitectura y las funcionalidades del microagente de Defender para IoT para IoT Edge de Azure Defender para IoT.
ms.topic: conceptual
ms.date: 09/09/2020
ms.openlocfilehash: 81eb8816e1bcf74a9e27e34d14465102599c7d5d
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104782663"
---
# <a name="azure-defender-for-iot-edge-defender-iot-micro-agent"></a>Microagente de Defender para IoT de Azure Defender para IoT

[Azure IoT Edge](../iot-edge/index.yml) proporciona capacidades de enorme eficacia para administrar y procesar flujos de trabajo empresariales en el perímetro.
El papel esencial que IoT Edge desempeña en entornos de IoT lo hace especialmente atrayente para atacantes malintencionados.

El microagente de Defender para IoT de Defender para IoT proporciona una solución de seguridad completa para los dispositivos IoT Edge.
El módulo de Defender para IoT recopila, agrega y analiza los datos de seguridad sin procesar del sistema operativo y del sistema de contenedores para generar alertas y recomendaciones de seguridad que requieren acción.

Al igual que los agentes de seguridad de Defender para IoT de los dispositivos IoT, el módulo de Defender para IoT Edge se puede personalizar profundamente a través de su módulo gemelo.
Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

El microagente de Defender para IoT para IoT Edge de Azure Defender para IoT ofrece las características siguientes:

- Recopila eventos de seguridad sin procesar del sistema operativo subyacente (Linux) y de los sistemas de contenedor de IoT Edge.

  Consulte [Configuración del agente de Defender para IoT](how-to-agent-configuration.md) para obtener más información sobre los recopiladores de datos de seguridad disponibles.

- Análisis de manifiestos de implementación de IoT Edge.

- Agrega eventos de seguridad sin procesar a los mensajes enviados a través de [IoT Edge Hub](../iot-edge/iot-edge-runtime.md#iot-edge-hub).

- Quita la configuración usando el gemelo del microagente de Defender para IoT.

  Consulte [Configuración de un agente de Defender para IoT](how-to-agent-configuration.md) para obtener más información.

El microagente de Defender para IoT para IoT Edge de Azure Defender para IoT se ejecuta en un modo con privilegios en IoT Edge.
El modo con privilegios es necesario para permitir que el módulo supervise el sistema operativo y otros módulos de IoT Edge.

## <a name="module-supported-platforms"></a>Plataformas admitidas por el módulo

El microagente de Defender para IoT para IoT Edge de Azure Defender para IoT solo está disponible actualmente en Linux.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha conocido la arquitectura y las funcionalidades del microagente de Defender para IoT para IoT Edge de Azure Defender para IoT.

Para seguir obteniendo información sobre la implementación de Defender para IoT, lea los siguientes artículos:

- Implemente el [microagente de Defender para IoT para IoT Edge](how-to-deploy-edge.md).
- Aprenda a [configurar el microagente de Defender para IoT](how-to-agent-configuration.md).
- Consulte [Defender para IoT Horizon](resources-manage-proprietary-protocols.md) de Defender para IoT.
- Obtenga información sobre cómo [habilitar el servicio Defender para IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Obtenga más información sobre el servicio en las [preguntas más frecuentes de Defender para IoT](resources-frequently-asked-questions.md)