---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 02/05/2021
ms.openlocfilehash: b06b91e972fd07543cf02105360cb0400ef6b0f1
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99831559"
---
Uso de las respuestas del entorno de ejecución del agente de IoT Edge para solucionar los problemas relacionados con el proceso. A continuación, se ofrece una lista de las posibles respuestas:

* 200 - CORRECTO
* 400 - La configuración de implementación tiene un formato incorrecto o no es válida.
* 417 - El dispositivo no tiene un conjunto de configuración de implementación.
* 412 - La versión del esquema de la configuración de implementación no es válida.
* 406 - El dispositivo de IoT Edge está sin conexión o no envía informes de estado.
* 500 - Error en el entorno en tiempo de ejecución de Azure IoT Edge.

Para más información, consulte [Agente de IoT Edge](/azure/iot-edge/iot-edge-runtime?view=iotedge-2018-06&preserve-view=true#iot-edge-agent).

El siguiente error está relacionado con el servicio de IoT Edge en Azure Stack Edge Pro<!--/ Data Box Gateway--> cada dispositivo.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Los módulos de proceso tienen el estado Desconocido y no se pueden usar

#### <a name="error-description"></a>Descripción del error

Todos los módulos del dispositivo muestran el estado Desconocido y no se pueden usar. El estado Desconocido persiste tras un reinicio.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Solución propuesta

Elimine el servicio de IoT Edge y, a continuación, vuelva a implementar los módulos. Para obtener más información, consulte [Eliminación del servicio de IoT Edge](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).
