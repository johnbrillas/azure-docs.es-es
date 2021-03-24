---
title: Primeros pasos con Azure Percept Audio
description: Aprenda a conectar el dispositivo Azure Percept Audio con Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 660f03ce248a27a00fdd443964fbdba2fe3adeb0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102179279"
---
# <a name="azure-percept-audio-setup"></a>Instalación de Azure Percept Audio

Azure Percept Audio está listo para usarse con Azure Percept DK. No es necesario realizar una instalación independiente.

## <a name="prerequisites"></a>Prerrequisitos

- Kit de desarrollo Azure Percept DK
- Azure Percept Audio
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el kit de desarrollo a una red Wi-Fi, creado una instancia de IOT Hub y conectado el kit a esta instancia.
- Altavoces o auriculares que se pueden conectar a un conector de audio de 3,5 mm (opcional)

## <a name="connecting-your-devices"></a>Conexión de dispositivos

1. Conecte el dispositivo Azure Percept Audio a la placa base de Azure Percept DK con el cable micro USB tipo B a USB tipo A incluido. Conecte el extremo del cable micro USB a la placa del intermediador (desarrollador) y el extremo tipo A a la placa base de Percept DK.
1. (Opcional) Conecte el altavoz o los auriculares al dispositivo Azure Percept DK mediante el conector de auriculares, que está etiquetado como "Line Out" (Salida de línea). Esto le permitirá oír las respuestas de audio del asistente para voz. Aunque no conecte un altavoz o unos auriculares, podrá ver las respuestas como texto en la ventana de demostración. 

1. Encienda el kit de desarrollo. El LED L02 en el placa del intermediador pasará a parpadear en blanco para indicar que el dispositivo se ha encendido y que el módulo de sistema de audio se está autenticando.

1. Espere a que se complete el proceso de autenticación; esto puede tardar hasta 3 minutos.

1. Estará listo para empezar a crear prototipos cuando vea algo de lo siguiente:

    - El LED L02 cambiará a blanco fijo. Esto indica que la autenticación ha finalizado y que el kit de desarrollo aún no se ha configurado con una palabra clave.
    - Los tres indicadores LED se vuelven azules. Esto indica que la autenticación ha finalizado y que el kit de desarrollo se ha configurado con una palabra clave.

## <a name="next-steps"></a>Pasos siguientes

Creación de una [solución de voz sin código](./tutorial-no-code-speech.md) en [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).
