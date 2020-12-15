---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0b3ccc31c9159b5d7b1615add89e8fdc308bf8df
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763460"
---
### <a name="publish-the-device-template"></a>Publicación de la plantilla de dispositivo

Para poder agregar un dispositivo a la aplicación, debe publicar la plantilla de dispositivo:

1. En la plantilla de dispositivo **LVA Edge Gateway v2** (Puerta de enlace LVA Edge v2), seleccione **Publish** (Publicar).

1. En la página **Publish this device template to the application** (Publicar esta plantilla de dispositivo en la aplicación), seleccione **Publicar** (Publicar).

**LVA Edge Gateway v2** (Puerta de enlace LVA Edge v2) ahora está disponible como un tipo de dispositivo que se puede usar en la página **Devices** (Dispositivos) de la aplicación.

## <a name="migrate-the-gateway-device"></a>Migración del dispositivo de puerta de enlace

El dispositivo **gateway-001** usa la plantilla de dispositivo **LVA Edge Gateway**. Para usar el nuevo manifiesto de implementación, migre el dispositivo a la nueva plantilla de dispositivo:

Para migrar el dispositivo **gateway-001**:

1. Vaya a la página **Devices** (Dispositivos) y seleccione el dispositivo **gateway-001** para resaltarlo en la lista.

1. Seleccione **Migrar**. Si el icono **Migrate** (Migrar) no está visible, seleccione **...** para ver más opciones.

    :::image type="content" source="media/iot-central-video-analytics-part4/migrate-device.png" alt-text="Migrar el dispositivo de puerta de enlace a una nueva versión":::

1. En la lista del cuadro de diálogo **Migrate** (Migrar), seleccione **LVA Edge Gateway v2** y, después, seleccione **Migrate** (Migrar).

Pocos segundos después la migración finaliza. El dispositivo usa ahora la plantilla de dispositivo **LVA Edge Gateway v2** con el manifiesto de implementación personalizado.

Ahora no hay ningún dispositivo que use la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge). Elimine esta plantilla de dispositivo:

1. Vaya a la página **Device templates** (Plantillas de dispositivos) y seleccione la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge).

1. Seleccione **Delete** (Eliminar) para eliminar dicha plantilla.

### <a name="get-the-device-credentials"></a>Obtención de las credenciales del dispositivo

Necesita las credenciales que permiten que el dispositivo se conecte a la aplicación de IoT Central. Para obtener las credenciales del dispositivo:

1. En la página **Devices** (Dispositivos), seleccione el dispositivo **gateway-001**.

1. Seleccione **Conectar**.

1. En la página **Device connection** (Conexión del dispositivo), anote en el archivo *scratchpad.txt* los valores de **ID Scope** (Ámbito de id.), **Device ID** (Id. de dispositivo) y **Primary Key** (Clave principal) del dispositivo. Utilizará estos valores más adelante.

1. Asegúrese de que el método de conexión se establece en **Shared access signature** (Firma de acceso compartido).

1. Seleccione **Cerrar**.

## <a name="next-steps"></a>Pasos siguientes

Ahora ha creado una aplicación de IoT Central con la plantilla de aplicación **Video analytics - object and motion detection** (Análisis de vídeo: detección de objetos y movimiento), ha creado una plantilla de dispositivo para el dispositivo de puerta de enlace y ha agregado un dispositivo de puerta de enlace a la aplicación.

Si desea probar la aplicación de análisis de vídeo con detección de objetos y movimiento con módulos IoT Edge que ejecutan una máquina virtual en la nube con secuencias de vídeo simuladas:

> [!div class="nextstepaction"]
> [Creación de una instancia de IoT Edge para análisis de vídeo (máquina virtual Linux)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-vm.md)

Si desea probar la aplicación de análisis de vídeo con detección de objetos y movimiento con módulos IoT Edge que ejecutan un dispositivo real con una cámara **ONVIF** real:

> [!div class="nextstepaction"]
> [Creación de una instancia de IoT Edge para análisis de vídeo (Intel NUC)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-nuc.md)
