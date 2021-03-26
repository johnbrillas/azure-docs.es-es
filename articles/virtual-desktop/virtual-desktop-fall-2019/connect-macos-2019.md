---
title: 'Conexión a Windows Virtual Desktop (clásico) desde macOS: Azure'
description: Conexión a Windows Virtual Desktop (clásico) mediante el cliente macOS.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a0d2bd6a07cfc9e5ebe205322550c9e898022846
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "88008447"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-macos-client"></a>Conexión a Windows Virtual Desktop (clásico) con el cliente macOS

> Se aplica a: macOS 10.12, o cualquier versión posterior

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop (clásico), que no admite objetos de Windows Virtual Desktop para Azure Resource Manager. Si está tratando de administrar objetos de Windows Virtual Desktop para Azure Resource Manager, consulte [este artículo](../connect-macos.md).

Puede acceder a los recursos de Windows Virtual Desktop desde sus dispositivos macOS con nuestro cliente, que se puede descargar. En esta guía se le indicará cómo configurar el cliente.

## <a name="install-the-client"></a>Instalar el cliente

Para empezar, [descargue](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) e instale el cliente en su dispositivo macOS.

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Suscríbase a la fuente que le proporcionó el administrador para obtener la lista de recursos administrados que tiene a su disposición en el dispositivo macOS.

Para suscribirse a una fuente:

1. Seleccione **Agregar área de trabajo** en la página principal para conectarse al servicio y recuperar los recursos.
2. Escriba la dirección URL de la fuente. Puede ser una dirección URL o una dirección de correo electrónico:
   - Si usa una dirección URL, use la que le proporcionó el administrador. Normalmente, la dirección URL es <https://rdweb.wvd.microsoft.com>.
   - Para usar un correo electrónico, escribe tu dirección de correo: Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador configuró el servidor de esa forma.
3. Seleccione **Agregar**.
4. Inicie sesión con su cuenta de usuario cuando se le solicite.

Después de iniciar sesión, verás una lista de los recursos disponibles.

Una vez suscrito a una fuente, el contenido de la fuente se actualizará automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el cliente macOS, consulte el documento [Introducción al cliente de macOS](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).