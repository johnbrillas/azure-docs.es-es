---
title: Descripción del archivo de configuración de Device Update para Azure IoT Hub | Microsoft Docs
description: Descripción del archivo de configuración de Device Update para Azure IoT Hub.
author: ValOlson
ms.author: valls
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 39ecd9d6d0deec942d5c8cce9357c779a4b328d3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101660548"
---
# <a name="device-update-for-iot-hub-configuration-file"></a>Archivo de configuración de Device Update para IoT Hub

El archivo "adu-conf.txt" es un archivo opcional que se puede crear para administrar las siguientes configuraciones.

* AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["manufacturer"]
* AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["model"]
* DeviceInformation.manufacturer
* DeviceInformation.model
* Cadena de conexión de dispositivo (si el agente de Device Update no la conoce).

## <a name="purpose"></a>Propósito
Primero, el agente de Device Update intentará obtener los valores de `manufacturer` y `model` del dispositivo que se usarán para el [nivel de interfaz](device-update-agent-overview.md#the-interface-layer). Si se produce un error, el agente de Device Update buscará el archivo "adu-conf.txt" y usará los valores que contiene. Si ambos intentos generan un error, el agente de Device Update usará los valores [predeterminados](https://github.com/Azure/iot-hub-device-update/blob/main/CMakeLists.txt).

Más información sobre las interfaces [ADU Core](https://github.com/Azure/iot-hub-device-update/tree/main/src/agent/adu_core_interface) y [Device Information](https://github.com/Azure/iot-hub-device-update/tree/main/src/agent/device_info_interface).

## <a name="file-location"></a>Ubicación del archivo

En el sistema Linux, en la partición o el disco denominado `adu`, cree un archivo de texto denominado "adu-conf.txt" con los campos siguientes.

## <a name="list-of-fields"></a>Lista de campos

|Nombre|Descripción|
|-----------|--------------------|
|connection_string|Cadena de conexión aprovisionada previamente que el dispositivo puede usar para conectarse a IoT Hub. Nota: No es necesario si está aprovisionando el agente de Device Update a través del [servicio de identidad de Azure IoT](https://azure.github.io/iot-identity-service/).|
|aduc_manufacturer|Indicado por la interfaz `AzureDeviceUpdateCore:4.ClientMetadata:4` para clasificar el dispositivo y establecer como destino la implementación de la actualización.|
|aduc_model|Indicado por la interfaz `AzureDeviceUpdateCore:4.ClientMetadata:4` para clasificar el dispositivo y establecer como destino la implementación de la actualización.|
|fabricante|El agente de Device Update lo indica como parte de la interfaz `DeviceInformation`.|
|model|El agente de Device Update lo indica como parte de la interfaz `DeviceInformation`.|

## <a name="example-adu-conftxt-file-contents"></a>Contenido del archivo "adu-conf.txt" de ejemplo

```markdown

connection_string = `HostName=<yourIoTHubName>;DeviceId=<yourDeviceId>;SharedAccessKey=<yourSharedAccessKey>`
aduc_manufacturer = <value to send through `AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["manufacturer"]`
aduc_model = <value to send through `AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["model"]`
manufacturer = <value to send through `DeviceInformation.manufacturer`
model = <value to send through `DeviceInformation.manufacturer`
```
