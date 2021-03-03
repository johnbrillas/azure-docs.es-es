---
title: Implementación de una actualización con Device Update para Azure IoT Hub | Microsoft Docs
description: Implemente una actualización con Device Update para Azure IoT Hub.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: 0a11c8e8946229941c1fe60f7f2ce84d9fadb2ed
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678848"
---
# <a name="deploy-an-update-using-device-update-for-iot-hub"></a>Implementación de una actualización con Device Update para IoT Hub

Obtenga información sobre cómo implementar una actualización en un dispositivo IoT con Device Update para IoT Hub.

## <a name="prerequisites"></a>Prerrequisitos

* [Acceso a una instancia de IoT Hub con Device Update para IoT Hub habilitado](create-device-update-account.md). Se recomienda usar un nivel S1 (estándar), o superior, para su instancia de IoT Hub. 
* [Al menos una actualización se importó correctamente para el dispositivo aprovisionado.](import-update.md) 
* Un dispositivo IoT (o simulador) aprovisionado para Device Update en IoT Hub.
* [Se asignó una etiqueta al dispositivo IoT que intenta actualizar. El dispositivo forma parte de un grupo de actualizaciones como mínimo.](create-update-group.md)
* Exploradores admitidos:
  * [Microsoft Edge](https://www.microsoft.com/edge)
  * Google Chrome

## <a name="deploy-an-update"></a>Implementación de una actualización

1. Vaya a [Azure Portal](https://portal.azure.com).

2. Vaya a la hoja Device Update de su instancia de IoT Hub.

  :::image type="content" source="media/deploy-update/device-update-iot-hub.png" alt-text="IoT Hub" lightbox="media/deploy-update/device-update-iot-hub.png":::

3. Seleccione la pestaña Grupos en la parte superior de la página. [Obtenga más información](device-update-groups.md) sobre los grupos de dispositivos. 

  :::image type="content" source="media/deploy-update/updated-view.png" alt-text="Pestaña Grupos" lightbox="media/deploy-update/updated-view.png":::

4. Consulte la lista de grupos y el gráfico de Update Compliance. Debería ver una nueva actualización disponible para el grupo de dispositivos, con un vínculo a la actualización en Actualizaciones pendientes (puede que tenga que actualizar una vez). [Obtenga más información sobre Update Compliance.](device-update-compliance.md) 

5. Seleccione la actualización disponible.

6. Compruebe que el grupo correcto está seleccionado como grupo de destino. Programe la implementación y, a continuación, seleccione Deploy update (Implementar actualización).

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Select update" lightbox="media/deploy-update/select-update.png"::: (Seleccionar actualización)

7. Consulte el gráfico de compatibilidad. Debería ver que la actualización está en curso. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Actualización en curso" lightbox="media/deploy-update/update-in-progress.png":::

8. Una vez que el dispositivo se haya actualizado correctamente, debería ver que el gráfico de compatibilidad y la actualización de los detalles de implementación reflejan lo mismo. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Actualización correcta" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Supervisión de una implementación de actualizaciones

1. Seleccione la pestaña Implementaciones en la parte superior de la página.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Pestaña Implementaciones" lightbox="media/deploy-update/deployments-tab.png":::

2. Seleccione la implementación que creó para ver los detalles de la implementación.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Detalles de implementación" lightbox="media/deploy-update/deployment-details.png":::

3. Seleccione Actualizar para ver los detalles de estado más recientes. Continúe con este proceso hasta que el estado cambie a Correcto.


## <a name="retry-an-update-deployment"></a>Reintento de una implementación de actualizaciones

Si por algún motivo se produce un error en la implementación, puede volver a intentarla para los dispositivos en los que se han producido errores. 

1. Vaya a la pestaña Implementaciones y seleccione la implementación en la que se produjo el error. 

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Detalles de implementación" lightbox="media/deploy-update/deployment-details.png":::

2. Haga clic en el estado de dispositivo "Error" en el panel Información de implementación detallada.

3. Haga clic en "Retry failed devices" (Reintentar en dispositivos con errores) y acepte la notificación de confirmación. 

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas comunes](troubleshoot-device-update.md)
