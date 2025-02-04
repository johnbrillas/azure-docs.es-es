---
title: Reemplazo de la batería del dispositivo Microsoft Azure StorSimple de la serie 8000
description: Describe cómo quitar, reemplazar y mantener el módulo de baterías de reserva en el dispositivo StorSimple.
author: alkohli
ms.service: storsimple
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 17a6a07d117557cdee23634bd211a14cf90d49e3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96023778"
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Reemplazar el módulo de baterías de reserva en el dispositivo StorSimple

## <a name="overview"></a>Información general
El Módulo de alimentación y refrigeración (PCM) del gabinete primario en el dispositivo StorSimple de Microsoft Azure tiene un paquete de baterías adicional. Este paquete proporciona energía para que el dispositivo StorSimple pueda guardar los datos si hay una pérdida de alimentación de CA para el gabinete principal. A este paquete de baterías se le conoce como *módulo de baterías de reserva*. El módulo de baterías de reserva solo existe para el gabinete principal del dispositivo StorSimple (el gabinete de EBOD no contiene un módulo de baterías de reserva).

Este tutorial explica cómo realizar lo siguiente:

* Quitar el módulo de baterías de reserva
* Instalar un nuevo módulo de baterías de reserva
* Mantener el módulo de baterías de reserva

> [!IMPORTANT]
> Antes de quitar y reemplazar un módulo de baterías de reserva, revise la información de seguridad de [Introducción al reemplazo de componentes de hardware de StorSimple](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-backup-battery-module"></a>Quitar el módulo de baterías de reserva
El módulo de baterías de reserva para el dispositivo StorSimple es una unidad reemplazable en situ. Antes de instalar en el PCM, el módulo de baterías debería almacenarse en su paquete original. Realice los siguientes pasos para extraer la batería de reserva.

#### <a name="to-remove-the-backup-battery-module"></a>Para quitar el módulo de baterías de reserva
1. En Azure Portal, vaya a la hoja del servicio Administrador de dispositivos de StorSimple. Vaya a **Dispositivos** y seleccione el dispositivo en la lista de dispositivos. Vaya a **Supervisar** > **Mantenimiento de hardware**. En **Componentes compartidos**, mire el estado de la batería.
2. Identifique el PCM en el que ha fallado la batería. La Figura 1 muestra la parte posterior del dispositivo StorSimple.
   
    ![Backplane de módulos del gabinete principal del dispositivo](./media/storsimple-battery-replacement/IC740994.png)
   
    **Figura 1** Vista posterior del dispositivo primario que muestra los módulos PCM y del controlador
   
   | Etiqueta | Descripción |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Controlador 0 |
   | 4 |Controlador 1 |
   
    Tal como se muestra el número 3 en la Figura 2, el LED indicador de supervisión en PCM 0 que corresponde a **Error de batería** debe estar encendido.
   
    ![Backplane de supervisión de LED indicadores de supervisión del PCM del dispositivo](./media/storsimple-battery-replacement/IC740992.png)
   
    **Figura 2** Parte posterior del PCM que muestra los LED indicadores de supervisión
   
   | Etiqueta | Descripción |
   |:--- |:--- |
   | 1 |Error de corriente alterna |
   | 2 |Error del ventilador |
   | 3 |Error de la batería |
   | 4 |PCM correcto |
   | 5 |Error de alimentación de CD |
   | 6 |Batería en funcionamiento |
3. Para quitar el PCM con una batería que no funciona, siga los pasos descritos en [Quitar un PCM](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm).
4. Después de quitar el PCM, eleve y gire el asas del módulo de batería hacia arriba como se indica en la siguiente figura, y tire de él para quitar la batería.
   
    ![Quitar batería del PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    **Figura 3** Quitar la batería del PCM
5. Coloque el módulo en el paquete de unidad reemplazable en campo.
6. Devolver la unidad defectuosa a Microsoft para el mantenimiento y control adecuado.

## <a name="install-a-new-backup-battery-module"></a>Instalar un nuevo módulo de baterías de reserva
Realice los pasos siguientes para instalar el módulo de baterías de sustitución en el PCM del receptáculo principal del dispositivo StorSimple.

#### <a name="to-install-the-battery-module"></a>Para instalar el módulo de batería
1. Coloque el módulo de baterías de reserva en la orientación correcta en el PCM.
2. Presione el asa del módulo de batería por completo para asentar el conector.
3. Sustituya el PCM en el gabinete principal siguiendo las directrices de [Reemplazar un Módulo de alimentación y refrigeración en su dispositivo StorSimple](storsimple-8000-power-cooling-module-replacement.md).
4. Una vez completada la sustitución, vaya al dispositivo y después a **Supervisar** > **Mantenimiento de hardware** en Azure Portal. Compruebe el estado de la batería para asegurarse de que la instalación fue correcta. Un estado verde indica que el estado de la batería es correcto.

## <a name="maintain-the-backup-battery-module"></a>Mantener el módulo de baterías de reserva
En el dispositivo StorSimple, el módulo de baterías de reserva proporciona energía al controlador durante un evento de pérdida de alimentación. Permite que el dispositivo StorSimple guarde los datos críticos antes de apagar el equipo de una manera controlada. Con dos baterías totalmente cargadas en los PCM, el sistema puede controlar dos eventos de pérdida consecutivos.

En Azure Portal, **Mantenimiento de hardware** en la hoja **Supervisar** indica si la batería funciona mal o si se aproxima el final del ciclo de vida. El estado de la batería se indica en **Batería en PCM 0** o **Batería en PCM 1** en **Componentes compartidos**. Esta hoja muestra un estado **DEGRADADO** cuando se aproxima el final del ciclo de vida, y **ERROR** cuando se alcanza el final del ciclo de vida.

> [!NOTE]
> La batería puede mostrar **ERROR** cuando simplemente necesita cargarse.


Si aparece el estado **DEGRADADO** , se recomienda lo siguiente:

* Puede que el sistema haya experimentado una pérdida de energía reciente o las baterías pueden estar llevando a cabo el mantenimiento periódico. Observe el sistema durante 12 horas antes de continuar.
  
  * Si el estado sigue siendo **DEGRADADO** después de 12 horas de conexión continua a alimentación de CA con los controladores y PCM en ejecución, a continuación, la batería debe reemplazarse. [Póngase en contacto con Microsoft Support](storsimple-8000-contact-microsoft-support.md) para obtener un módulo de baterías de reserva de reemplazo.
  * Si el estado cambia a CORRECTO después de 12 horas, la batería funciona y solo necesita una carga de mantenimiento.
* Si no ha habido una pérdida de alimentación de CA asociada y el PCM está encendido y conectado a CA, debe reemplazar la batería. [Póngase en contacto con Microsoft Support](storsimple-8000-contact-microsoft-support.md) para obtener un módulo de baterías de reserva de reemplazo.

> [!IMPORTANT]
> Deseche la batería que no funciona según las normas nacionales y regionales.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre el [Reemplazo de los componentes de hardware de StorSimple](storsimple-8000-hardware-component-replacement.md).

