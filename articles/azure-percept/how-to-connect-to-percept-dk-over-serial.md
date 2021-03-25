---
title: Conexión serie a Azure Percept DK
description: Obtenga información acerca de cómo configurar una conexión serie a Azure Percept DK con PuTTY y un cable serie de USB a TTL.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 93b8ab0ce53202402e86b059abe3c600590d549e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101660537"
---
# <a name="connect-to-your-azure-percept-dk-over-serial"></a>Conexión serie a Azure Percept DK

Siga los pasos que se indican a continuación para configurar una conexión serie a Azure Percept DK mediante [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

> [!WARNING]
> **No** intente conectar su dispositivo DevKit a través de serie excepto en casos de error extremos (por ejemplo, si su dispositivo ya no funciona). La retirada del alojamiento de la placa base para conectar el cable serie es muy complicada y podría provocar la rotura de los cables de la antena Wi-Fi.

## <a name="prerequisites"></a>Requisitos previos

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- PC host
- Azure Percept DK
- [Cable serie USB a TTL](https://www.adafruit.com/product/954)

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/usb-serial-cable.png" alt-text="Cable serie USB a TTL.":::

## <a name="initiate-the-serial-connection"></a>Inicio de la conexión serie

1. Si la placa base está conectada a un raíl de 80/20, quítela del raíl con la llave Allen (incluida en la tarjeta de bienvenida del dispositivo DevKit).

1. Quite los tornillos situados en la parte inferior del alojamiento de la placa base y extraiga la placa base.

    > [!WARNING]
    > Al quitar la placa base, se romperán los cables de la antena Wi-Fi. **No** continúe con la conexión serie a menos que sea el último recurso para recuperar el dispositivo.

1. Quite el disipador térmico.

1. Quite la placa del jumper de los conectores GPIO.

    > [!TIP]
    > Antes de retirar la placa del jumper, fíjese en cómo está orientada. Por ejemplo, dibuje una flecha o pegue un adhesivo en la placa del jumper apuntando hacia el circuito como referencia. La placa del jumper no está codificada y puede conectarse al revés por error al volver a montar la placa base.

1. Conecte el [cable serie USB a TTL](https://www.adafruit.com/product/954) a los conectores GPIO de la placa base, como se muestra a continuación. Observe que el cable rojo no está conectado.

    - Conecte el cable negro (GND) al conector 6.
    - Conecte el cable blanco (RX) al conector 8.
    - Conecte el cable verde (TX) al conector 10.

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/serial-connection-carrier-board.png" alt-text="Conexiones de conectores serie de la placa base.":::

1. Encienda el dispositivo DevKit y conecte el lado USB del cable serie al PC.

1. En Windows, vaya a **Inicio** -> **Configuración de Windows Update** -> **Ver actualizaciones opcionales** -> **Actualizaciones de controladores**. Busque una actualización de serie a USB en la lista, active la casilla situada junto a ella y haga clic en **Descargar e instalar**.  

1. A continuación, abra el administrador de dispositivos de Windows (**Inicio** -> **Administrador de dispositivos**). Vaya a **Puertos** y haga clic en **USB a UART** para abrir **Propiedades**. Tenga en cuenta a qué puerto COM está conectado el dispositivo.

1. Haga clic en la pestaña **Configuración de puerto**. Asegúrese de que **Bits por segundo** esté establecido en 115200.

1. Abra PuTTY. Escriba lo siguiente y haga clic en **Abrir** para conectarse a su dispositivo DevKit a través de serie:

    1. Línea serie: COM[n.º de puerto]
    1. Velocidad: 115200
    1. Tipo de conexión: Serie

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/putty-serial-session.png" alt-text="Ventana de sesión de PuTTY con parámetros de serie seleccionados.":::

## <a name="next-steps"></a>Pasos siguientes

Para actualizar un dispositivo que no se pueda arrancar a través de serie con el [cable serie de USB a TTL](https://www.adafruit.com/product/954), consulte la guía de actualización USB para situaciones no estándar.

[comment]: # (Agregar el vínculo a la guía de actualización USB cuando esté disponible.)