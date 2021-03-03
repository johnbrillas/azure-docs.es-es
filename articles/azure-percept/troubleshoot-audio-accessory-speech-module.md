---
title: Solución de problemas de los módulos de audio y voz de Azure Percept
description: Vea sugerencias para la solución de algunos de los problemas más comunes que pueden surgir durante la experiencia de incorporación.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 1e2ad920afb55066f07430568f976154f6d7cae1
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678874"
---
# <a name="azure-percept-audio-and-speech-module-troubleshooting"></a>Solución de problemas de los módulos de audio y voz de Azure Percept

Siga las instrucciones que se indican a continuación para solucionar problemas de la aplicación del asistente para voz.

## <a name="collecting-speech-module-logs"></a>Recopilación de registros del módulo de voz

Para ejecutar estos comandos, [conéctese al punto de acceso Wi-Fi de Azure Percept DK y al kit de desarrollo mediante SSH](./how-to-ssh-into-percept-dk.md), y escriba los comandos en el terminal SSH.

```console
 iotedge logs azureearspeechclientmodule
```

Para redirigir cualquier salida a un archivo .txt para su posterior análisis, use la sintaxis siguiente:

```console
[command] > [file name].txt
```

Después de redirigir la salida a un archivo .txt, copie el archivo en el equipo host a través de SCP:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

[ruta de acceso del archivo de host local] hace referencia a la ubicación del equipo host en el que desea copiar el archivo .txt. [nombre de usuario remoto] es el nombre de usuario de SSH elegido durante la [experiencia de instalación](./quickstart-percept-dk-set-up.md). Si no ha configurado un inicio de sesión de SSH durante la configuración rápida, el nombre del usuario remoto es "root".

## <a name="checking-runtime-status-of-the-speech-module"></a>Comprobación del estado del entorno de ejecución del módulo de voz

Compruebe si el estado del entorno de ejecución de **azureearspeechclientmodule** aparece como **En ejecución**. Para buscar el estado del entorno de ejecución de los módulos del dispositivo, abra [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) y vaya a **Todos los recursos** ->  **\<your IoT hub>**  -> **IoT Edge** ->  **\<your device ID>** . Haga clic en la pestaña **Módulos** para ver el estado del entorno de ejecución de todos los módulos instalados.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Página del dispositivo Edge en Azure Portal.":::

Si el estado del entorno de ejecución de **azureearspeechclientmodule** no aparece como **en ejecución**, haga clic en **Establecer módulos** -> **azureearspeechclientmodule**. En la página **Configuración del módulo**, establezca **Estado deseado** como **En ejecución** y haga clic en **Actualizar**.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/firmware-desired-status-stopped.png" alt-text="Pantalla Establecer módulos de Azure Portal.":::

## <a name="understanding-ear-som-led-indicators"></a>Descripción de los indicadores LED del módulo de sistema de escucha

Puede usar indicadores LED para comprender en qué estado se encuentra el dispositivo. Normalmente, el módulo tarda aproximadamente 2 minutos en inicializarse por completo después de *encenderse*. A medida que recorra los pasos de inicialización, verá:

1. 1 luz verde a la izquierda: el dispositivo está encendido. 
2. 1 luz verde a la izquierda y el indicador LED del centro parpadean en color verde: la autenticación está en curso. 
3. Los tres indicadores LED cambiarán a azul una vez que el dispositivo esté autenticado y listo para usarse.

|Estado del indicador LED                  |Estado del módulo de sistema de escucha            |
|----------------------------|---------------------------|
|1 LED verde (LED izquierdo)         |encendido |
|1 LED verde (LED izquierdo) <br> 1 LED intermitente verde (LED central) |autenticación en curso |
|3 LED apagados                      |inicialización completada |
|3 LED azules                     |listo para su uso |
|3 LED intermitentes azules            |palabra clave reconocida |
|3 LED que parpadean rápidamente en azul              |procesar |
|3 LED rojos                      |silenciado |

## <a name="next-steps"></a>Pasos siguientes

Consulte la [guía general de solución de problemas](./troubleshoot-dev-kit.md) para más información sobre la solución de problemas de Azure Percept DK.