---
title: Desempaquetado y ensamblaje de los componentes de Azure Percept DK
description: Aprenda a desempaquetar, conectar y encender Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/16/2021
ms.custom: template-quickstart
ms.openlocfilehash: efa255ba38f7e00785335bf458ecc0ed91da646b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608187"
---
# <a name="quickstart-unbox-and-assemble-your-azure-percept-dk-components"></a>Inicio rápido: Desempaquetado y ensamblaje de los componentes de Azure Percept DK

Una vez que haya recibido su kit de desarrollo Azure Percept DK, consulte esta guía para obtener información sobre la conexión de los componentes y el encendido.

## <a name="prerequisites"></a>Prerrequisitos

- Azure Percept DK (kit de desarrollo)
- Destornillador P7 (opcional, se usa para fijar el conector del cable de alimentación a la placa base)

## <a name="unbox-and-assemble-your-device"></a>Desempaquetado y ensamblaje del dispositivo

1. Desempaquetado de los componentes de Azure Percept DK.

    El kit de desarrollo contiene una placa base, el módulo de sistema Azure Percept Vision, la caja de accesorios que contiene antenas y cables, y una tarjeta de bienvenida con una llave Allen.

1. Conecte los componentes del kit de desarrollo.

    > [!NOTE]
    > El puerto del adaptador de alimentación se encuentra en el lado derecho de la placa base. Los puertos restantes (2 USB-A, 1 USB-C y 1 Ethernet) y el botón de encendido se encuentran en el lado izquierdo de la placa base.

    1. Atornille las antenas de la red Wi-Fi en la placa base.

    1. Conecte el módulo de sistema de visión al puerto USB-C de la placa base con el cable USB-C.

    1. Conecte el cable de alimentación al adaptador correspondiente.

    1. Elimine cualquier embalaje de plástico restante de los dispositivos.

    1. Conecte el cable o el adaptador de alimentación a la placa base y a un enchufe de pared. Para fijar por completo el conector del cable de alimentación a la placa base, use un destornillador P7 (no incluido en el kit de desarrollo) para apretar los tornillos del conector.

    1. Después de conectar el cable de alimentación a un enchufe de pared, el dispositivo se enciende automáticamente. Se iluminará el botón de encendido del lado izquierdo de la placa base. Deje que transcurra algo de tiempo para que el dispositivo arranque.

        > [!NOTE]
        > El botón de encendido se usa para apagar o reiniciar el dispositivo mientras está conectado a una toma de corriente. En caso de que se produzca una interrupción del suministro eléctrico, el dispositivo se reiniciará automáticamente.

Para ver una demostración visual del ensamblado del kit de desarrollo, consulte el intervalo de 0:00 a 0:50 del siguiente vídeo:

</br>

> [!VIDEO https://www.youtube.com/embed/-dmcE2aQkDE]

## <a name="next-steps"></a>Pasos siguientes

Ahora que el kit de desarrollo está conectado y encendido, consulte el [tutorial de la experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md) para completar la instalación del dispositivo. La experiencia de instalación le permite conectar el kit de desarrollo a una red Wi-Fi, configurar un inicio de sesión mediante SSH, crear una instancia de IoT Hub y aprovisionar el kit de desarrollo en su cuenta de Azure. Una vez que haya completado la instalación del dispositivo, estará listo para empezar a crear prototipos.