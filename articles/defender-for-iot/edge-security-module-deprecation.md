---
title: Compatibilidad y retirada de características
titleSuffix: Azure Defender for IoT
description: Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/21/2021
ms.service: azure
ms.topic: how-to
ms.openlocfilehash: 2779a73d3a5f77e3a3b144309baf6d2788565443
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809659"
---
# <a name="feature-support-and-retirement"></a>Compatibilidad y retirada de características

En este artículo se describen las características de Azure Defender para IoT y la compatibilidad con diferentes funcionalidades dentro de Defender para IoT.

## <a name="defender-for-iot-c-c-and-edge-security-module-deprecation"></a>Módulo de seguridad de C, C# y Edge de Defender para IoT en desuso

El nuevo microagente reemplazará al módulo de seguridad de C, C# y Edge.  

El nuevo microagente se basa el conocimiento y la experiencia obtenidos no solo durante el desarrollo del módulo de seguridad existente, sino también de los comentarios de los clientes y asociados, y tiene cuatro mejoras importantes: 

- **Valor de seguridad de profundidad**: el nuevo agente se ejecutará en el nivel de host, lo que proporcionará más visibilidad a las operaciones subyacentes del dispositivo y mayor cobertura de seguridad.

- **Mayor rendimiento de los dispositivos y menor superficie**: se consigue gracias a una RAM y a una superficie de memoria ROM pequeñas, así como un bajo consumo de la CPU.  

- **Plug and play**: el nuevo microagente ya no tiene dependencias en el nivel de kernel y todas sus dependencias de software se proporcionan como parte de su paquete. El microagente admite una arquitectura de CPU común.

- **Fácil de implementar**: El microagente admite diferentes modelos de distribución, a través de código fuente y en forma de paquete binario. 

### <a name="timeline"></a>Escala de tiempo 

Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022. 

## <a name="micro-agent-preview-support"></a>Compatibilidad con la versión preliminar del microagente

Durante la versión preliminar, el microagente puede experimentar cambios importantes sin previo aviso.

## <a name="next-steps"></a>Pasos siguientes

Consulte [API del sensor y la consola de administración de Defender para IoT](references-work-with-defender-for-iot-apis.md).