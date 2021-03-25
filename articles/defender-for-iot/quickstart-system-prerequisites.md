---
title: Requisitos previos del sistema
description: Obtenga los requisitos previos del sistema necesarios para ejecutar Azure Defender para IoT.
ms.date: 11/30/2020
ms.topic: quickstart
ms.openlocfilehash: 78d5948554ae531c4b2f77d67bb916d5290db943
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104780946"
---
# <a name="system-prerequisites"></a>Requisitos previos del sistema
En este artículo se enumeran los requisitos previos del sistema necesarios para ejecutar Azure Defender para IoT.

## <a name="minimum-requirements"></a>Requisitos mínimos

- Conmutadores de red que admitan la supervisión del tráfico a través del puerto SPAN.
- Dispositivos de hardware para sensores NTA.
- El rol Colaborador en la suscripción de Azure. Solo se requiere durante la incorporación para definir los dispositivos confirmados y la conexión a Azure Sentinel.
- Rol **Colaborador** en IoT Hub (niveles Gratis o Estándar), para la administración conectada en la nube. Asegúrese de que la característica **Azure Defender para IoT** está habilitada.
- En cuanto a la compatibilidad con Defender-IoT-micro-agent en el nivel de dispositivo, los agentes de Defender para IoT admiten una lista cada vez mayor de dispositivos y plataformas. Consulte la [lista de plataformas admitidas](how-to-deploy-agent.md).

## <a name="supported-service-regions"></a>Regiones de servicio admitidas

Defender para IoT enruta todo el tráfico de todas las regiones europeas hasta el centro de datos regional de Oeste de Europa. Enruta el tráfico de las regiones restantes al centro de datos regional de Centro de EE. UU.

Para más información, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="see-also"></a>Consulte también

- [Identificación de los dispositivos necesarios](how-to-identify-required-appliances.md)
- [Acerca de la configuración de red de Azure Defender para IoT](how-to-set-up-your-network.md)
