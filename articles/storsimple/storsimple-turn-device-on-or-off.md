---
title: Encendido o apagado del dispositivo de la serie StorSimple 8000
description: Explica cómo encender un dispositivo de StorSimple nuevo, encender un dispositivo apagado o que perdió energía y apagar un dispositivo activo.
author: alkohli
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.topic: how-to
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8f4269235c494ff9dd8d1bf8e0ef940562f8927
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85515273"
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Activación o desactivación del dispositivo StorSimple serie 8000

## <a name="overview"></a>Información general
No se requiere apagar el dispositivo de Microsoft Azure StorSimple como parte del funcionamiento normal del sistema. Sin embargo, es posible que sea necesario encender un dispositivo nuevo o puede que se haya debido apagar un dispositivo. Por lo general, se requiere apagar un dispositivo en casos en que se debe reemplazar algún hardware con error, mover físicamente una unidad o quitar un dispositivo del servicio. En este tutorial, se describe el procedimiento necesario para encender y apagar el dispositivo de StorSimple en distintos escenarios.

## <a name="turn-on-a-new-device"></a>Activar un dispositivo nuevo
Los pasos para activar por primera vez un dispositivo StorSimple difieren si el modelo del dispositivo es 8100 u 8600. El modelo 8100 tiene un único gabinete principal, mientras que el modelo 8600 es un dispositivo de dos gabinetes con un gabinete principal y un gabinete EBOD. Las secciones siguientes abarcan los pasos detallados para ambos modelos.

* [Dispositivo nuevo solo con gabinete principal](#new-device-with-primary-enclosure-only)
* [Dispositivo nuevo con gabinete EBOD](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Dispositivo nuevo solo con gabinete principal
El modelo StorSimple 8100 es un dispositivo con un único gabinete. El dispositivo incluye módulos de alimentación y de refrigeración (PCM) redundantes. Ambos PCM deben estar instalados y conectados a diferentes fuentes de alimentación para garantizar una alta disponibilidad.

Realice los pasos siguientes para pasar los cables de alimentación del dispositivo.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Si desea consultar las instrucciones de cableado y configuración del dispositivo, vaya a [Instalar el dispositivo StorSimple 8100](storsimple-8100-hardware-installation.md). Asegúrese de seguir las instrucciones de manera exacta.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Dispositivo nuevo con gabinete EBOD
El modelo StorSimple 8600 tiene un gabinete principal y un gabinete EBOD. Esto requiere que se realice el cableado de alimentación y conectividad de SCSI conectadas en serie (SAS) de las unidades en conjunto.

Cuando configure este dispositivo por primera vez, realice primero los pasos para el cableado de SAS y luego complete los pasos para el cableado de alimentación.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Si desea consultar las instrucciones de cableado y configuración del dispositivo, vaya a [Instalar el dispositivo StorSimple 8600](storsimple-8600-hardware-installation.md). Asegúrese de seguir las instrucciones de manera exacta.

## <a name="turn-on-a-device-after-shutdown"></a>Activar un dispositivo después del apagado
Los pasos para activar un dispositivo StorSimple después del apagado son distintos si el modelo del dispositivo es 8100 u 8600. El modelo 8100 tiene un único gabinete principal, mientras que el modelo 8600 es un dispositivo de dos gabinetes con un gabinete principal y un gabinete EBOD.

* [Dispositivo solo con gabinete principal](#device-with-primary-enclosure-only)
* [Dispositivo con gabinete EBOD](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Dispositivo solo con gabinete principal
Después del apagado, utilice el procedimiento siguiente para encender un dispositivo StorSimple con un gabinete principal y sin gabinete EBOD.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Para activar un dispositivo solo con gabinete principal
1. Asegúrese de que el interruptor de alimentación de cada uno de los módulos de alimentación y refrigeración (PCM) se encuentran en la posición de apagado. Si los interruptores no se encuentran en la posición de apagado, póngalos en esa posición y espere que se apaguen las luces.
2. Gire el interruptor de alimentación de ambos PCM a la posición de encendido para encender el dispositivo. El sistema se debería encender.
3. Revise lo siguiente para comprobar que el dispositivo esté completamente encendido:
   
   1. Las luces LED de actividad correcta de ambos módulos PCM aparecen en verde.
   2. Las luces LED de ambos controladores son fijas y aparecen en verde.
   3. La luz LED azul de uno de los controladores aparece intermitente, lo que indica que el controlador está activo.
      
      Si alguna de estas condiciones no se cumple, el dispositivo no funciona correctamente. [Póngase en contacto con el soporte técnico de Microsoft](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Dispositivo con gabinete EBOD
Después del apagado, utilice el procedimiento siguiente para encender un dispositivo StorSimple con un gabinete principal y un gabinete EBOD. Realice cada paso de la secuencia tal como se describe. De lo contrario, podría haber una pérdida de datos.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Para encender un dispositivo con un gabinete principal y un gabinete EBOD
1. Asegúrese de que el gabinete EBOD está conectado al gabinete principal. Para obtener más información, consulte [Instalar el dispositivo StorSimple 8600](storsimple-8600-hardware-installation.md).
2. Asegúrese de que los módulos de alimentación y refrigeración (PCM) del gabinete EBOD y del gabinete principal se encuentran en la posición de apagado. Si los interruptores no se encuentran en la posición de apagado, póngalos en esa posición y espere que se apaguen las luces.
3. Gire el interruptor de alimentación de ambos PCM a la posición de encendido para encender primero el gabinete EBOD. Las luces LED de los PCM deben aparecer en verde. Una luz LED verde en el controlador EBOD de esta unidad indica que el gabinete EBOD está encendido.
4. Gire el interruptor de alimentación de ambos PCM a la posición de encendido para encender el gabinete principal. Ahora todo el sistema debería estar encendido.
5. Compruebe que las luces LED de SAS son verdes, lo que garantiza que la conexión entre el gabinete EBOD y el gabinete principal funciona correctamente.

## <a name="turn-on-a-device-after-a-power-loss"></a>Activar un dispositivo después de una pérdida de energía
Una interrupción o una pérdida de energía puede apagar un dispositivo StorSimple. El corte de energía puede producirse en uno o en ambos sistemas de alimentación. Los pasos de recuperación son distintos si el modelo del dispositivo es 8100 u 8600. El modelo 8100 tiene un único gabinete principal, mientras que el modelo 8600 es un dispositivo de dos gabinetes con un gabinete principal y un gabinete EBOD. En esta sección se describe el procedimiento de recuperación para cada uno de los escenarios.

* [Dispositivo solo con gabinete principal](#8100)
* [Dispositivo con gabinete EBOD](#8600)

### <a name="device-with-primary-enclosure-only"></a>Dispositivo solo con gabinete principal <a name="8100"></a>
El sistema puede funcionar de manera normal si se produce una pérdida de energía en uno de sus sistemas de alimentación. Sin embargo, para garantizar la alta disponibilidad del dispositivo, restaure la energía del sistema de alimentación tan pronto como sea posible.

Si hay un corte o una interrupción en la energía en ambos sistemas de alimentación, el sistema se apagará de manera ordenada y controlada. Cuando se restaura la energía, el sistema se enciende automáticamente.

### <a name="device-with-ebod-enclosure"></a>Dispositivo con gabinete EBOD <a name="8600"></a>
#### <a name="power-loss-on-one-power-supply"></a>Pérdida de energía en un sistema de alimentación
El sistema puede funcionar de manera normal si se produce una pérdida de energía en el gabinete principal o en el gabinete EBOD. Sin embargo, para garantizar la alta disponibilidad del dispositivo, restaure la energía del sistema de alimentación tan pronto como sea posible.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Pérdida de energía en ambos sistemas de alimentación en el gabinete principal y el gabinete EBOD
Si hay un corte o una interrupción en la energía en ambos sistemas de alimentación, el gabinete EBOD se apagará inmediatamente y el gabinete principal se apagará de manera ordenada y controlada. Cuando se restaure la energía, el dispositivo se iniciará automáticamente.

Si la energía se apaga manualmente, haga lo siguiente para restaurar la energía del sistema.

1. Encienda el gabinete EBOD.
2. Una vez que encienda el gabinete EBOD, encienda el gabinete principal.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Pérdida de energía en ambos sistemas de alimentación en el gabinete EBOD
Cuando instala los cables, debe asegurarse de que el gabinete EBOD nunca esté conectado solo a un PDU independiente. Si el gabinete EBOD y el gabinete principal presentan errores al mismo tiempo, el sistema se recuperará.

Si solo el gabinete EBOD presenta errores en ambos sistemas de alimentación, el sistema no se recuperará de manera automática. Haga lo siguiente para encender el sistema y restaurarlo a un estado correcto:

1. Si el gabinete principal está encendido, apague ambos módulos de alimentación y enfriamiento (PCM).
2. Espere unos minutos hasta que se apague el sistema.
3. Encienda el gabinete EBOD.
4. Una vez que encienda el gabinete EBOD, encienda el gabinete principal.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Activar un dispositivo tras la pérdida de la conexión entre el gabinete principal y EBOD
Si se pierde la conexión entre el controlador en espera y el controlador EBOD correspondiente, el dispositivo sigue funcionando. Si se pierde la conexión entre el controlador activo del sistema y el controlador EBOD correspondiente, se producirá la conmutación por error y el dispositivo debería seguir funcionando de manera normal.

Cuando se quitan ambos cables SAS o se interrumpe la conexión entre el gabinete EBOD y el gabinete principal, el dispositivo dejará de funcionar. En este punto, haga lo siguiente.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Para encender el dispositivo tras la pérdida de la conexión
1. Obtenga acceso a la parte posterior del dispositivo.
2. Si se interrumpe la conexión del cable SAS entre el gabinete EBOD y el gabinete principal, se apagarán todas las luces LED del canal de SAS del gabinete EBOD.
3. Apagué los módulos de alimentación y enfriamiento (PCM) del gabinete EBOD y del gabinete principal.
4. Espere hasta que se apaguen todas las luces que se encuentran en la parte posterior de ambos gabinetes.
5. Vuelva a insertar los cables SAS y asegúrese de que la conexión entre el gabinete EBOD y el gabinete principal sea correcta.
6. Encienda primero el gabinete EBOD; para ello, gire ambos interruptores del PCM a la posición de encendido.
7. Revise que la luz LED verde esté encendida para asegurarse de que el gabinete EBOD también lo está.
8. Encienda el gabinete principal.
9. Revise que la luz LED verde del controlador esté encendida para asegurarse de que el gabinete principal también lo está.
10. Revise que las luces LED del canal SAS (cuatro por cada controlador EBOD) estén todas encendidas para comprobar que la conexión del gabinete EBOD con el gabinete principal es correcta.

> [!IMPORTANT]
> Si los cables SAS son defectuosos o la conexión entre el gabinete EBOD y el gabinete principal no es buena, cuando encienda el sistema, entrará en el modo de recuperación. Por favor, [póngase en contacto con el soporte técnico de Microsoft](storsimple-8000-contact-microsoft-support.md) si ocurre esto.


## <a name="turn-off-a-running-device"></a>Apagar un dispositivo activo
Puede ser necesario apagar un dispositivo StorSimple activo si debe trasladarlo, quitarlo del servicio o si tiene un componente que no funciona correctamente y que se debe reemplazar. Los pasos son distintos si el modelo del dispositivo StorSimple es 8100 u 8600. El modelo 8100 tiene un único gabinete principal, mientras que el modelo 8600 es un dispositivo de dos gabinetes con un gabinete principal y un gabinete EBOD. En esta sección, se detallan los pasos pagara apagar un dispositivo activo.

* [Dispositivo con gabinete principal](#8100a)
* [Dispositivo con gabinete EBOD](#8600a)

### <a name="device-with-primary-enclosure"></a>Dispositivo con gabinete principal <a name="8100a"></a>
Para apagar el dispositivo de manera ordenada y controlada, use Azure Portal o Windows PowerShell para StorSimple. 

> [!IMPORTANT]
> No apague un dispositivo activo con el botón de inicio/apagado que se encuentra en la parte posterior del dispositivo.
> 
> Antes de apagar el dispositivo, asegúrese de que todos los componentes del mismo funcionen bien. En Azure Portal, vaya a **Dispositivos** > **Supervisar** > **Mantenimiento de hardware** y compruebe que todos los componentes tengan el estado verde. Esto solo sucede si el sistema funciona correctamente. Si el sistema se apaga para sustituir un componente en mal estado, verá un estado de error (rojo) o de degradado (amarillo) para el componente correspondiente en el **Estado de hardware**.
> 
> 

Después de acceder a Windows PowerShell para StorSimple o a Azure Portal, siga los pasos para [apagar un dispositivo de StorSimple](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure"></a>Dispositivo con gabinete EBOD <a name="8600a"></a>
> [!IMPORTANT]
> Antes de apagar el gabinete principal y el gabinete EBOD, asegúrese de que todos los componentes del dispositivo funcionen bien. En Azure Portal, vaya a **Dispositivos** > **Monitor** > **Hardware health** (Mantenimiento de hardware) y compruebe que todos los componentes tienen un estado correcto.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Para apagar un dispositivo activo con gabinete EBOD
1. Siga todos los pasos para [apagar un dispositivo de StorSimple](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) en el gabinete principal.
2. Una vez que se apaga el gabinete principal, apague el gabinete EBOD; para ello, gire ambos interruptores del módulo de alimentación y enfriamiento (PCM) a la posición de apagado.
3. Para comprobar que el EBOD se apagó, revise que todas las luces de la parte posterior del gabinete EBOD están apagadas.

> [!NOTE]
> Los cables SAS que se utilizan para conectar el gabinete EBOD con el gabinete principal no deben quitarse hasta después que se apague el sistema.

## <a name="next-steps"></a>Pasos siguientes
[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) .

