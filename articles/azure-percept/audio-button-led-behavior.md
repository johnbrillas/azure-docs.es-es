---
title: Botón y comportamiento de los indicadores LED de Azure Percept Audio
description: Obtenga más información sobre el botón y los estados de los indicadores LED de Azure Percept Audio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 1d956e33a84b5509c16400c8f5f8e813d116411a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102095755"
---
# <a name="azure-percept-audio-button-and-led-behavior"></a>Botón y comportamiento de los indicadores LED de Azure Percept Audio

Consulte la siguiente guía para obtener información sobre el botón y los estados de los indicadores LED de Azure Percept Audio.

## <a name="button-behavior"></a>Comportamiento del botón

Puede usar los botones para controlar el comportamiento del dispositivo.

|Estado del botón|  Comportamiento|
|------------|----------|
|Silencio|  Presione para desactivar o activar el sonido de la matriz de micrófonos. Al pulsar el botón, el evento de botón se desactiva o activa.|
|PTT/PTS|   Presione PTT para omitir el estado de detección de palabras clave y activar el estado de escucha de comandos. Vuelva a presionar esta tecla para detener el diálogo activo del agente y volver al estado de detección de palabras clave. Al pulsar el botón, el evento de botón se desactiva o activa. PTS solo funciona cuando se presiona el botón mientras el agente está hablando, no cuando el agente está escuchando o pensando.|

## <a name="led-behavior"></a>Comportamiento del LED

Puede usar indicadores LED para comprender en qué estado se encuentra el dispositivo.

|LED|   Estado del indicador LED|  Estado del módulo de sistema de escucha|
|---|------------|----------------| 
|L02|   1 blanco, estático activado |Encendido |
|L02|   1 blanco, intermitencia de 0,5 Hz|  Autenticación en curso |
|L01 & L02 & L03|   3 azules, estático activado|     Esperando palabra clave|
|L01 & L02 & L03|   Intermitencia de la matriz LED, 20 fps | Escuchando o hablando|
|L01 & L02 & L03|   Parpadeo rápido de la matriz LED, 20 fps|    Pensando|
|L01 & L02 & L03|   3 rojos, estático activado | Silencio|

## <a name="next-steps"></a>Pasos siguientes

Para obtener sugerencias de solución de problemas para el dispositivo de Azure Percept Audio, consulte esta [guía](./troubleshoot-audio-accessory-speech-module.md).