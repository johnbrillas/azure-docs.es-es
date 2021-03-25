---
title: Desencadenador de Azure IoT Hub para Azure Functions
description: Aprenda a responder a los eventos enviados a una secuencia de eventos de IoT Hub en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 5c9309834b407ee56d29e38afd965ac947fc8a4f
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102612293"
---
# <a name="azure-iot-hub-trigger-for-azure-functions"></a>Desencadenador de Azure IoT Hub para Azure Functions

En este artículo se explica cómo usar enlaces de Azure Functions para IoT Hub. La compatibilidad de IoT Hub se basa en el [enlace de Azure Event Hubs](functions-bindings-event-hubs.md).

Para obtener información sobre los detalles de instalación y configuración, vea la [información general](functions-bindings-event-iot.md).

> [!IMPORTANT]
> Aunque los ejemplos de código siguientes usan la API de Event Hubs, la sintaxis proporcionada es aplicable para las funciones de IoT Hub.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs-trigger.md)]

## <a name="hostjson-properties"></a>Propiedades de host.json

El archivo [host.json](functions-host-json.md#eventhub) contiene la configuración que controla el comportamiento del desencadenador del centro de eventos. Consulte la sección de [configuración de host.json](functions-bindings-event-iot.md#hostjson-settings) para más información sobre las opciones de configuración disponibles.

## <a name="next-steps"></a>Pasos siguientes

- [Escribir eventos en una secuencia de eventos (enlace de salida)](./functions-bindings-event-iot-output.md)
