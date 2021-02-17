---
title: Recomendaciones de seguridad
description: Sepa en qué consiste el concepto de recomendaciones de seguridad y cómo se usan en Defender para IoT.
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
ms.date: 01/25/2021
ms.author: shhazam
ms.openlocfilehash: 10246277715d23f42eb0bd17d19794b8d4503f3f
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100521873"
---
# <a name="security-recommendations"></a>Recomendaciones de seguridad

Defender para IoT analiza los dispositivos IoT y los recursos de Azure y hace recomendaciones de seguridad para reducir la superficie expuesta a ataques.
Las recomendaciones de seguridad requieren una acción y su objetivo es ayudar a los clientes a cumplir los procedimientos recomendados de seguridad.

En este artículo, encontrará una lista de recomendaciones que se pueden activar en IoT Hub o en los dispositivos IoT.

## <a name="agent-based-recommendations"></a>Recomendaciones basadas en agente

Las recomendaciones de dispositivo proporcionan información y sugerencias para mejorar la posición de seguridad de los dispositivos.

| severity | Nombre | Origen de datos | Descripción |
|--|--|--|--|
| Media | Puertos abiertos en el dispositivo | Módulo de seguridad clásico | Se encontró un punto de conexión de escucha en el dispositivo. |
| Media | Se encontró una directiva de firewall permisiva en una de las cadenas. | Módulo de seguridad clásico | Se encontró una directiva de firewall permitida en las cadenas de firewall principales (ENTRADA/SALIDA). La directiva debe denegar todo el tráfico de manera predeterminada y definir reglas para permitir la comunicación necesaria hacia y desde el dispositivo. |
| Media | Se encontró una regla de firewall permisiva en la cadena de entrada | Módulo de seguridad clásico | Se encontró una regla en el firewall que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos. |
| Media | Se encontró una regla de firewall permisiva en la cadena de salida | Módulo de seguridad clásico | Se encontró una regla en el firewall que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos. |
| Media | Error de validación de línea base de sistema operativo | Módulo de seguridad clásico | El dispositivo no cumple las [pruebas comparativas de CIS Linux](https://www.cisecurity.org/cis-benchmarks/). |

### <a name="agent-based-operational-recommendations"></a>Recomendaciones operativas basadas en agente

Las recomendaciones operativas proporcionan información y sugerencias para mejorar la configuración de los agentes de seguridad.

| severity | Nombre | Origen de datos | Descripción |
|--|--|--|--|
| Bajo | El agente envía mensajes sin utilizar | Módulo de seguridad clásico | 10 % o más de los mensajes de seguridad tenían un tamaño inferior a 4 KB durante las últimas 24 horas. |
| Bajo | Configuración de gemelo de seguridad no óptima | Módulo de seguridad clásico | La configuración del gemelo de seguridad no es óptima. |
| Bajo | Conflicto de configuración de gemelo de seguridad | Módulo de seguridad clásico | Se identificaron conflictos en la configuración del gemelo de seguridad. |  |


## <a name="built-in-recommendations-in-iot-hub"></a>Recomendaciones integradas en IoT Hub

Las alertas de recomendación proporcionan información y sugerencias relativas a las acciones para mejorar la posición de seguridad del entorno.

| severity | Nombre | Origen de datos | Descripción |
|--|--|--|--|
| Alto | Credenciales de autenticación idénticas utilizadas por varios dispositivos | IoT Hub | Hay varios dispositivos que usan las mismas credenciales de autenticación de IoT Hub. Este proceso puede indicar que hay un dispositivo ilegítimo que suplanta un dispositivo legítimo. El uso de credenciales duplicadas aumenta el riesgo de suplantación del dispositivo a manos de un individuo malintencionado. |
| Media | La directiva del filtro de IP predeterminada debe ser Denegar. | IoT Hub | La configuración del filtro IP debe tener reglas definidas para el tráfico permitido y debe denegar el resto del tráfico de forma predeterminada. |
| Media | La regla del filtro IP incluye un amplio intervalo de IP | IoT Hub | Un intervalo IP de origen de la regla de filtro IP permitido es demasiado grande. Las reglas excesivamente permisivas podrían exponer su instancia de IoT Hub a agentes malintencionados. |
| Bajo | Habilitar registros de diagnóstico en IoT Hub | IoT Hub | Habilite los registros y consérvelos por hasta un año. Conservar los registros le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |

## <a name="next-steps"></a>Pasos siguientes

- [Información general](overview.md) del servicio Defender para IoT
- Aprenda a [acceder a los datos de seguridad](how-to-security-data-access.md)
- Más información sobre cómo [investigar un dispositivo](how-to-investigate-device.md)
