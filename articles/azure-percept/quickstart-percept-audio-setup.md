---
title: Primeros pasos con Azure Percept Audio
description: Aprenda a conectar el dispositivo Azure Percept Audio con Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 575107859f56df742ab41a299269c250511022b3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664419"
---
# <a name="azure-percept-audio-setup"></a>Instalación de Azure Percept Audio

Azure Percept Audio está listo para usarse con Azure Percept DK. No es necesario realizar una instalación independiente.

## <a name="prerequisites"></a>Prerrequisitos

- Kit de desarrollo Azure Percept DK
- Azure Percept Audio
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el kit de desarrollo a una red Wi-Fi, creado una instancia de IoT Hub y conectado el kit a esta instancia.

## <a name="connecting-your-devices"></a>Conexión de dispositivos

1. Conecte el dispositivo Azure Percept Audio a la placa base de Azure Percept DK con el cable USB Micro Type-B a USB Type-A incluido. Conecte el extremo del cable Micro Type-B al módulo de sistema de audio y el extremo Type-A a la placa base de Percept DK.

1. Encienda el kit de desarrollo.

    - El indicador LED L01 del módulo de sistema de audio cambiará a una luz verde fija para indicar que el dispositivo está encendido.
    - El indicador LED L02 cambiará a una luz verde intermitente para indicar que el módulo de sistema de audio se está autenticando.

1. Espere a que se complete el proceso de autenticación; esto puede tardar hasta 3 minutos.

1. Estará listo para empezar a crear prototipos cuando vea algo de lo siguiente:

    - El indicador LED L01 se apaga y el L02 se vuelve blanco. Esto indica que la autenticación ha finalizado y que el kit de desarrollo aún no se ha configurado con una palabra clave.
    - Los tres indicadores LED se vuelven azules. Esto indica que la autenticación ha finalizado y que el kit de desarrollo se ha configurado con una palabra clave.

    > [!NOTE]
    > Póngase en contacto con el soporte técnico si el kit de desarrollo no se autentica.

## <a name="next-steps"></a>Pasos siguientes

Cree una [solución de voz sin código](./tutorial-no-code-speech.md).