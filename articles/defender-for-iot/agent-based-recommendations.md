---
title: Recomendaciones basadas en agente
titleSuffix: Azure Defender for IoT
description: Sepa en qué consiste el concepto de recomendaciones de seguridad y cómo se usan en dispositivos de Defender para IoT.
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
ms.date: 02/16/2021
ms.author: shhazam
ms.openlocfilehash: c7407db1460c2d0a83f2262a3348c81b4de300a7
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100641027"
---
# <a name="security-recommendations-for-iot-devices"></a>Recomendaciones de seguridad para dispositivos IoT

Defender para IoT analiza los dispositivos IoT y los recursos de Azure y hace recomendaciones de seguridad para reducir la superficie expuesta a ataques.
Las recomendaciones de seguridad requieren una acción y su objetivo es ayudar a los clientes a cumplir los procedimientos recomendados de seguridad.

En este artículo, encontrará una lista de recomendaciones que se pueden activar en los dispositivos IoT.

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

## <a name="next-steps"></a>Pasos siguientes

- [Información general](overview.md) del servicio Defender para IoT
- Aprenda a [acceder a los datos de seguridad](how-to-security-data-access.md)
- Más información sobre cómo [investigar un dispositivo](how-to-investigate-device.md)
