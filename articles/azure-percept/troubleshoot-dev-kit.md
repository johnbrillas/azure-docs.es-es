---
title: Solución de problemas generales de Azure Percept DK e IoT Edge
description: Obtenga sugerencias para la solución de algunos de los problemas más comunes que se encuentran durante la experiencia de incorporación.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: a6d099e8d267c9fe03e0bb676276e7a4ab8157ab
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102521533"
---
# <a name="azure-percept-dk-dev-kit-troubleshooting"></a>Solución de problemas de Azure Percept DK (kit de desarrollo)

Consulte las instrucciones siguientes para obtener sugerencias de solución de problemas generales de Azure Percept DK.

## <a name="general-troubleshooting-commands"></a>Comandos de solución de problemas generales

Para ejecutar estos comandos, 
1. Conéctese a la [Wi-Fi AP del kit de desarrollo](./quickstart-percept-dk-set-up.md).
1. [Conéctese mediante SSH al kit de desarrollo](./how-to-ssh-into-percept-dk.md)
1. Escriba los comandos en el terminal de SSH.

Para redirigir cualquier salida a un archivo .txt para su posterior análisis, use la sintaxis siguiente:

```console
[command] > [file name].txt
```

Después de redirigir la salida a un archivo .txt, copie el archivo en el equipo host a través de SCP:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

```[local host file path]``` hace referencia a la ubicación del equipo host en el que desea copiar el archivo .txt. ```[remote username]``` es el nombre de usuario de SSH elegido durante la [experiencia de instalación](./quickstart-percept-dk-set-up.md). Si no ha configurado un inicio de sesión de SSH durante la configuración rápida, el nombre del usuario remoto es ```root```.

Para más información sobre los comandos de Azure IoT Edge, consulte la [documentación de solución de problemas de dispositivos de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

|Categoría:         |Comando:                    |Función:                  |
|------------------|----------------------------|---------------------------|
|SO                |```cat /etc/os-release```         |comprobar la versión de la imagen de Mariner |
|SO                |```cat /etc/os-subrelease```      |comprobar la versión de la imagen derivada |
|SO                |```cat /etc/adu-version```        |comprobar la versión de ADU |
|Temperatura       |```cat /sys/class/thermal/thermal_zone0/temp``` |comprobar la temperatura del kit de desarrollo |
|Wi-Fi             |```journalctl -u hostapd.service``` |comprobar los registros de SoftAP|
|Wi-Fi             |```journalctl -u wpa_supplicant.service``` |comprobar los registros de los servicios de Wi-Fi |
|Wi-Fi             |```journalctl -u ztpd.service```  |comprobar los registros del servicio de aprovisionamiento de Wi-Fi Zero Touch |
|Wi-Fi             |```journalctl -u systemd-networkd``` |comprobar los registros de la pila de red de Mariner |
|Wi-Fi             |```/data/misc/wifi/hostapd_virtual.conf``` |comprobar los detalles de configuración del punto de acceso Wi-fi |
|OOBE              |```journalctl -u oobe -b```       |comprobar los registros de la configuración rápida |
|Telemetría         |```azure-device-health-id```      |buscar identificador HW_ID de telemetría única |
|Azure IoT Edge          |```sudo iotedge check```          |ejecutar comprobaciones de configuración y conectividad para problemas comunes |
|Azure IoT Edge          |```sudo iotedge logs [container name]``` |comprobar los registros de contenedor, como los módulos de voz y visión |
|Azure IoT Edge          |```sudo iotedge support-bundle --since 1h``` |recopilar registros del módulo, registros del administrador de seguridad de Azure IoT Edge, registros del motor de contenedor, salida JSON de ```iotedge check``` y otra información de depuración útil de la última hora |
|Azure IoT Edge          |```sudo journalctl -u iotedge -f``` |ver los registros del administrador de seguridad de Azure IoT Edge |
|Azure IoT Edge          |```sudo systemctl restart iotedge``` |reiniciar el demonio de seguridad de Azure IoT Edge |
|Azure IoT Edge          |```sudo iotedge list```           |enumerar los módulos de Azure IoT Edge implementados |
|Otros             |```df [option] [file]```          |mostrar información sobre el espacio total o disponible en los sistemas de archivos especificados |
|Otros             |```ip route get 1.1.1.1```        |mostrar la información de la interfaz y la dirección IP del dispositivo |
|Otros             |```ip route get 1.1.1.1 \| awk '{print $7}'``` <br> ```ifconfig [interface]``` |mostrar solo la dirección IP del dispositivo |


Los comandos de Wi-Fi ```journalctl``` se pueden combinar en el siguiente comando único:

```console
journalctl -u hostapd.service -u wpa_supplicant.service -u ztpd.service -u systemd-networkd -b
```

## <a name="docker-troubleshooting-commands"></a>Comandos de solución de problemas de Docker

|Comando:                        |Función:                  |
|--------------------------------|---------------------------|
|```docker ps``` |[muestra qué contenedores se están ejecutando](https://docs.docker.com/engine/reference/commandline/ps/) |
|```docker images``` |[muestra qué imágenes hay en el dispositivo](https://docs.docker.com/engine/reference/commandline/images/)|
|```docker rmi [image id] -f``` |[elimina una imagen del dispositivo](https://docs.docker.com/engine/reference/commandline/rmi/) |
|```docker logs -f edgeAgent``` <br> ```docker logs -f [module_name]``` |[toma los registros de contenedor del módulo especificado](https://docs.docker.com/engine/reference/commandline/logs/) |
|```docker image prune``` |[quita todas las imágenes pendientes](https://docs.docker.com/engine/reference/commandline/image_prune/) |
|```watch docker ps``` <br> ```watch ifconfig [interface]``` |comprueba el estado de descarga del contenedor de Docker |

## <a name="usb-updating"></a>Actualización de USB

|Error:                                    |Solución:                                               |
|------------------------------------------|--------------------------------------------------------|
|LIBUSB_ERROR_XXX en la unidad flash a través de UUU |Este error es el resultado de un error de conexión USB durante la actualización de UUU. Si el cable USB no está conectado correctamente a los puertos USB del equipo o a la unidad PE-10X, se producirá un error con este formato. Intente desconectar y volver a conectar ambos extremos del cable USB y mueva el cable para garantizar una conexión segura. Esto casi siempre soluciona el problema. |

## <a name="azure-percept-dk-carrier-board-led-states"></a>Estados de los indicadores LED de la placa base de Azure Percept DK

Hay tres pequeños indicadores LED en la parte superior de la carcasa de la placa base. Un icono de nube está impreso junto al indicador LED 1, un icono de Wi-Fi junto al LED 2 y un icono de signo de exclamación junto al LED 3. Consulte la tabla siguiente para obtener información sobre cada estado de LED.

|LED             |State      |Descripción                      |
|----------------|-----------|---------------------------------|
|LED 1 (IoT Hub) |Encendido (luz fija) |El dispositivo está conectado a una instancia de IoT Hub. |
|LED 2 (Wi-Fi)   |Luz intermitente lenta |El dispositivo está listo para que Wi-Fi Easy Connect pueda configurarlo y anuncia su presencia a un configurador. |
|LED 2 (Wi-Fi)   |Luz intermitente rápida |La autenticación se realizó correctamente y la asociación del dispositivo está en curso. |
|LED 2 (Wi-Fi)   |Encendido (luz fija) |La autenticación y la asociación se realizaron correctamente; el dispositivo está conectado a una red Wi-Fi. |
|LED 3           |N/D         |El indicador LED no está en uso. |


