---
title: Descripción de recursos de Device Update para Azure IoT Hub | Microsoft Docs
description: Descripción de recursos de Device Update para Azure IoT Hub
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ba43889b885252f68bb3b4b158b5626411aac3d5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101678821"
---
# <a name="device-update-resources"></a>Recursos de Device Update

Para usar Device Update para IoT Hub, debe crear una cuenta de actualización de dispositivos y un recurso de instancia. 

## <a name="device-update-account"></a>Cuenta de Device Update

Una cuenta de Device Update es un recurso que se crea en la suscripción de Azure. En el nivel de la cuenta de Device Update, puede seleccionar la región en la que se creará dicha cuenta. También puede establecer permisos para autorizar a los usuarios que tendrán acceso a Device Update.


## <a name="device-update-instance"></a>Instancia de Device Update
Después de crear una cuenta, debe crear una instancia de Device Update. Una instancia es un contenedor lógico que contiene las actualizaciones y las implementaciones asociadas a un IoT Hub específico. Device Update usa IoT Hub como un directorio de dispositivos y un canal de comunicación con dispositivos. 

Durante la versión preliminar pública, se pueden crear dos cuentas de Device Update por suscripción. Además, se pueden crear dos instancias de Device Update por cuenta.

## <a name="configuring-device-update-linked-iot-hub"></a>Configuración de una instancia de Device Update vinculada a IoT Hub 

Para que Device Update puede recibir notificaciones de cambios de IoT Hub, debe integrarse con el centro de eventos "integrado". Al hacer clic en el botón "Configurar IoT Hub" de la instancia, se configuran las rutas de mensajes y la directiva de acceso necesarias para comunicarse con los dispositivos IoT. 

Las siguientes rutas de mensajes están configuradas para Device Update:

|   Nombre de ruta    | Consulta de enrutamiento  | Descripción  |
| :--------- | :---- |:---- |
|  DeviceUpdate.DigitalTwinChanges | true |Escucha los eventos de cambios del gemelo digital  |
|  DeviceUpdate.DeviceLifeCycle | opType = 'deleteDeviceIdentity'  | Escucha los dispositivos que se han eliminado |
|  DeviceUpdate.TelemetryModelInformation | iothub-interface-id = "urn:azureiot:ModelDiscovery:ModelInformation:1 | Escucha los nuevos tipos de dispositivos |
|  DeviceUpdate.DeviceTwinEvents| (opType = 'updateTwin' OR opType = 'replaceTwin') AND IS_DEFINED($body.tags.ADUGroup) | Escucha los nuevos grupos de actualización de dispositivos |

## <a name="next-steps"></a>Pasos siguientes

[Creación de recursos de actualización de dispositivos](./create-device-update-account.md)
