---
title: Requisitos de red de Device Update para IoT Hub | Microsoft Docs
description: Device Update para IoT Hub usa varios puertos de red para propósitos diferentes.
author: philmea
ms.author: philmea
ms.date: 1/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: e72ff144a56f44ccaa695b7dab328e42052fce39
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678864"
---
# <a name="ports-used-with-device-update-for-iot-hub"></a>Puertos utilizados con Device Update para IoT Hub
ADU usa varios puertos de red para propósitos diferentes.

## <a name="default-ports"></a>Puertos predeterminados

Propósito|Número de puerto |
---|---
Descarga desde redes y redes CDN  | 80 (protocolo HTTP)
Descarga desde MCC y redes CDN | 80 (protocolo HTTP)
Conexión de ADU Agent a Azure IoT Hub  | 8883 (protocolo MQTT)

## <a name="use-azure-iot-hub-supported-protocols"></a>Uso de protocolos admitidos por Azure IoT Hub
ADU Agent se puede modificar para usar cualquiera de los protocolos admitidos por Azure IoT Hub.

[Más información](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols#:~:text=Table%202%20%20%20,%201%20more%20rows) sobre la lista actual de protocolos admitidos.
