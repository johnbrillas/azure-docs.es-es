---
title: Conexión de Azure Percept DK a través de SSH
description: Aprenda a configurar una conexión SSH en Azure Percept DK con PuTTY
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 8d150228be2cf6deff3bc2fd0a0599cca70d24ac
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660576"
---
# <a name="connect-to-your-azure-percept-dk-over-ssh"></a>Conexión de Azure Percept DK a través de SSH

Siga los pasos que se indican a continuación para configurar una conexión SSH a Azure Percept DK mediante [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

## <a name="prerequisites"></a>Requisitos previos

- Un equipo host basado en Windows, Linux u OS X con funcionalidad Wi-Fi
- Un cliente SSH
    - Si el equipo host ejecuta Windows, [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) es un cliente SSH efectivo y se utilizará a lo largo de esta guía.
    - Si el equipo host ejecuta Linux u OS X, los servicios SSH están incluidos en esos sistemas operativos y se pueden ejecutar sin una aplicación cliente independiente. Consulte la documentación del producto del sistema operativo para más información sobre cómo ejecutar servicios SSH.
- Azure Percept DK

## <a name="initiate-the-ssh-connection"></a>Iniciar la conexión SSH

1. Encendido del kit de desarrollo Azure Percept DK

1. Si el kit de desarrollo ya está conectado a una red a través de Ethernet o Wi-Fi, vaya al paso siguiente. En caso contrario, conecte el equipo host directamente al punto de acceso Wi-Fi del kit de desarrollo, como cuando se conecta a cualquier otra red Wi-Fi:
    - **nombre de red**: scz-xxxx (donde "xxxx" son los últimos cuatro dígitos de la dirección de red MAC del kit de desarrollo)
    - **contraseña**: puede encontrarla en la tarjeta de bienvenida que se incluye en el kit de desarrollo.

    > [!WARNING]
    > Mientras está conectado al punto de acceso Wi-Fi de Azure Percept DK, el equipo host perderá temporalmente su conexión a Internet. Las llamadas de videoconferencias activas, el streaming web o cualquier otra experiencia basada en red se interrumpirán hasta que se complete el paso 3 de la incorporación de Azure Percept DK.

1. Abra PuTTY. Escriba lo siguiente y haga clic en **Abrir** para la conexión SSH en el kit de desarrollo:

    1. Nombre de host: 10.1.1.1
    1. Puerto: 22
    1. Tipo de conexión: SSH

    > [!NOTE]
    > **Nombre de host** es la dirección IP del dispositivo. Si el kit de desarrollo está conectado a su punto de acceso Wi-Fi, la dirección IP será 10.1.1.1. Si el kit de desarrollo está conectado a través de Ethernet, utilice la dirección IP local del dispositivo, que puede obtener en el centro o el enrutador Ethernet. Si el dispositivo está conectado a través de Wi-Fi, debe utilizar la dirección IP que se ha proporcionado durante la [incorporación de Azure Percept DK](./quickstart-percept-dk-set-up.md).

    :::image type="content" source="./media/how-to-ssh-into-percept-dk/ssh-putty.png" alt-text="Imagen.":::

1. Inicie sesión en el terminal de PuTTY. Si ha configurado un nombre de usuario y una contraseña de SSH durante la configuración rápida, escriba esas credenciales de inicio de sesión cuando se le solicite. En caso contrario, escriba lo siguiente:  

    1. login as: root
    1. Contraseña: p@ssw0rd

    :::image type="content" source="./media/how-to-ssh-into-percept-dk/putty-terminal.png" alt-text="Ventana de terminal de PuTTY.":::  

## <a name="next-steps"></a>Pasos siguientes

Después de conectarse correctamente a Azure Percept DK mediante SSH, puede realizar una serie de tareas, entre las que se incluyen la solución de problemas, las actualizaciones de USB y la ejecución de DiagTool o SoftAP Tool.


