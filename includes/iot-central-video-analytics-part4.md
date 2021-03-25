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
ms.openlocfilehash: d9c2aea284a2ab84b5d45fe35a35785adfc88123
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99832080"
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

