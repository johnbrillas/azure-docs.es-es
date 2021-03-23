---
title: Problemas conocidos de Azure Percept
description: Aprenda más sobre los problemas conocidos de Azure Percept y sus soluciones alternativas.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: reference
ms.date: 03/03/2021
ms.openlocfilehash: a04e53c8444a01bc42f3ce71393fc842f3419e74
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102193366"
---
# <a name="known-issues"></a>Problemas conocidos

Si se produce alguno de estos problemas, no es necesario abrir una incidencia del error. Si tiene problemas con cualquiera de las soluciones alternativas, abra una incidencia.

|Área|Descripción del problema|Solución alternativa|
|-------|---------|---------|
| Experiencia de incorporación | No se puede realizar la experiencia de incorporación a menos que se configure la red Wi-Fi del dispositivo (error de inicio de sesión de Azure). | 1. Conectarse mediante SSH al punto de acceso del dispositivo (10.1.1.1) <br> 2. Identificar y copiar la dirección IP Ethernet del dispositivo <br> 3. Conectarse a la experiencia de incorporación mediante la dirección URL basada en la IP Ethernet copiada |
| Experiencia de incorporación | A veces, al hacer clic en los vínculos del CLUF durante la experiencia de incorporación, no se abre una nueva página web. | Copie el vínculo y ábralo en una ventana del explorador independiente. |
| Experiencia de incorporación | No se puede trabajar con la experiencia de incorporación cuando se conecta a una zona activa Wi-Fi para móviles. | Conecte el dispositivo directamente a SoftAP, a una red Wi-Fi o a una red a través de Ethernet. |
| Wi-Fi/SoftAP | A veces, SoftAP puede desconectarse o desaparecer. | Estamos investigando el problema.  Al reiniciar el dispositivo, lo normal es que vuelva a aparecer. |
| Wi-Fi | El botón de hardware que activa o desactiva la red Wi-Fi o SoftAP en ocasiones no funciona. | Siga presionando el botón o reinicie el dispositivo. |
| Wi-Fi | Los usuarios pueden ver un mensaje después de conectarse a la red Wi-Fi que dice que esta red Wi-Fi usa un estándar de seguridad antiguo. | La zona activa o el punto SoftAP del kit de desarrollo usa el algoritmo de cifrado WEP. |
| Wi-Fi | No se puede conectar con SoftAP desde el equipo con Windows 10 y aparece el siguiente mensaje de error: <br> "No se puede conectar a esta red". | Reinicie el kit de desarrollo y el equipo. |
| Actualización de dispositivos | Los contenedores no se ejecutan después de una actualización de OTA. | Conéctese mediante SSH al dispositivo y reinicie el contenedor de IoT Edge con este comando `systemctl restart iotedge`. Esta acción reiniciará todos los contenedores. |
| Actualización de dispositivos | Los usuarios pueden recibir un mensaje que indica que se ha producido un error en la actualización, incluso si se realizó correctamente. | Confirme que el dispositivo se ha actualizado desplazándose hasta el dispositivo gemelo de este dispositivo en IoT Hub. Este problema se ha corregido después de la primera actualización. |
| Actualización de dispositivos | Los usuarios pueden perder su configuración de conexión Wi-Fi después de la primera actualización. | Recorra la experiencia de incorporación después de la actualización para configurar la conexión Wi-Fi. Este problema se ha corregido después de la primera actualización. |
| Actualización de dispositivos | Después de realizar una actualización de OTA, los usuarios ya no pueden iniciar sesión a través de SSH mediante las cuentas de usuario creadas anteriormente, y no se pueden crear nuevos usuarios de SSH mediante la experiencia de incorporación. Este problema afecta a los sistemas que realizan actualizaciones de OTA desde las siguientes versiones de imagen preinstaladas: 2020.110.114.105 y 2020.109.101.105. | Para recuperar los perfiles de usuario, siga estos pasos después de la actualización de OTA: <br> [Conéctese mediante SSH a su kit de desarrollo](./how-to-ssh-into-percept-dk.md) con el nombre de usuario "raíz". Si ha deshabilitado el inicio de sesión de usuario "raíz" de SSH a través de la experiencia de incorporación, debe volver a habilitarlo. Ejecute este comando después de conectarse correctamente: <br> ```mkdir -p /var/custom-configs/home; chmod 755 /var/custom-configs/home``` <br> Para recuperar los datos particulares del usuario anterior, ejecute el siguiente comando: <br> ```mkdir -p /tmp/prev-rootfs && mount /dev/mmcblk0p3 /tmp/prev-rootfs && [ ! -L /tmp/prev-rootfs/home ] && cp -a /tmp/prev-rootfs/home/* /var/custom-configs/home/. && echo "User home migrated!"; umount /tmp/prev-rootfs``` |
| Actualización de dispositivos | Después de realizar una actualización de OTA, se pierden los grupos de actualización. | Actualice la etiqueta del dispositivo siguiendo [estas instrucciones](https://docs.microsoft.com/azure/azure-percept/how-to-update-over-the-air#create-a-device-update-group). |
| Instalador del paquete de herramientas de desarrollo | Se puede producir un error en la instalación opcional de Caffe si Docker no se ejecuta correctamente en el sistema. | Asegúrese de que Docker está instalado y en ejecución y vuelva a intentar la instalación de Caffe. |
| Instalador del paquete de herramientas de desarrollo | La instalación opcional de CUDA no se completará correctamente en sistemas incompatibles. | Antes de ejecutar el instalador, compruebe la compatibilidad del sistema con CUDA. |
| Docker, red, IoT Edge | Si la red interna usa 172.x.x.x, los contenedores de Docker no podrán conectarse a Edge. | Agregue una sección especial bip al archivo /etc/docker/daemon.json de la siguiente manera: `{    "bip": "192.168.168.1/24"}` |
|Azure Percept Studio | Los vínculos de vista de transmisiones dentro de Azure Percept Studio no abren una nueva ventana que muestra la transmisión web del dispositivo. | 1. Abra [Azure Portal](https://portal.azure.com) y seleccione **IoT Hub**. <br> 2. Haga clic en el centro de IoT al que está conectado el dispositivo. <br> 3. En **Administración de dispositivos automática**, seleccione **IoT Edge** en la página de su centro de IoT. <br> 4. Seleccione el dispositivo de la lista. <br> 5. Seleccione **Establecer módulos** en la parte superior de la página del dispositivo. <br> 6. Haga clic en el icono de papelera junto a **HostIpModule** para eliminar el módulo. <br> 7. Para confirmar la acción, haga clic en **Revisar y crear** y, luego, en **Crear**. <br> 8. Abra [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) y haga clic en **Dispositivos** en el panel de menú izquierdo. <br> 9. Seleccione el dispositivo de la lista. <br> 10. En la pestaña **Visión**, haga clic en **View your device stream** (Ver transmisiones del dispositivo). El dispositivo descargará una nueva versión de HostIpModule y abrirá una pestaña del explorador con la transmisión web del dispositivo. |