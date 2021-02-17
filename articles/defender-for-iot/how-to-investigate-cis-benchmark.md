---
title: Investigación de la recomendación de puntos de referencia de CIS
titleSuffix: Azure Defender for IoT
description: Realice investigaciones básicas y avanzadas en función de las recomendaciones de línea base del sistema operativo.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/21/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 2f68ebedb229f7295bc9c5dcc3b3349808970e8c
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809651"
---
# <a name="investigate-os-baseline-based-on-cis-benchmark-recommendation"></a>Investigación de la recomendación de línea base del sistema operativo (basada en el punto de referencia CIS) 

Realice investigaciones básicas y avanzadas en función de las recomendaciones de línea base del sistema operativo.

## <a name="basic-os-baseline-security-recommendation-investigation"></a>Investigación básica de la recomendación de seguridad del sistema operativo  

Para investigar las recomendaciones de línea base del sistema operativo, vaya al portal de Azure Defender para IoT, en **IoT Hub**. Para más información, consulte cómo [investigar las recomendaciones de seguridad](quickstart-investigate-security-recommendations.md).

## <a name="advanced-os-baseline-security-recommendation-investigation"></a>Investigación de las recomendaciones avanzadas de seguridad del sistema operativo  

En esta sección se describe cómo conocer mejor los resultados de las pruebas de la línea base del sistema operativo y consultar eventos en Azure Log Analytics.  

La investigación de las recomendaciones avanzadas de seguridad de línea base del sistema operativo solo se admite mediante Log Analytics. Conecte defender para IoT a un área de trabajo de Log Analytics antes de continuar. Para más información sobre las recomendaciones avanzadas de seguridad de línea base del sistema operativo, consulte cómo [configurar una solución basada en agente de Azure Defender para IoT](how-to-configure-agent-based-solution.md).

Para consultar las alertas de los eventos de seguridad de IoT en Log Analytics:

1. Vaya a la página **Alertas**.

1. Seleccione **Investigate recommendations in Log Analytics workspace** (Investigar recomendaciones en el área de trabajo de Log Analytics).

Para consultar las recomendaciones de los eventos de seguridad de IoT en Log Analytics:

1. Vaya a la página **Recomendaciones**.

1. Seleccione **Investigate recommendations in Log Analytics workspace** (Investigar recomendaciones en el área de trabajo de Log Analytics).

1. Para ver los detalles de un dispositivo concreto, seleccione **Show Operation system (OS) baseline rules details** [Mostrar los detalles de las reglas de línea base del sistema operativo (SO)] en la página de vista rápida **Recommendation details** (Detalles de la recomendación).

   :::image type="content" source="media/how-to-investigate-cis-benchmark/recommendation-details.png" alt-text="Vea los detalles de un dispositivo específico."::: 

Para consultar los eventos de seguridad de IoT en el área de trabajo de Log Analytics directamente:

1. Vaya a la página **Registros**.

    :::image type="content" source="media/how-to-investigate-cis-benchmark/logs.png" alt-text="Seleccione los registros en el panel izquierdo.":::

1. Seleccione **Investigate the alerts** (Investigar las alertas) o bien, seleccione la opción **Investigate the alerts in Log Analytics** (Investigar las alertas en Log Analytics) de cualquier recomendación de seguridad o alerta.   

## <a name="useful-queries-to-investigate-the-os-baseline-resources"></a>Consultas útiles para investigar los recursos de línea base del sistema operativo: 

> [!Note]
> Asegúrese de reemplazar `<device-id>` por los nombres que asignó al dispositivo en cada una de las siguientes consultas. 


### <a name="retrieve-the-latest-information"></a>Recuperación de la información más reciente

- **Error de flota de dispositivos**: ejecute la siguiente consulta para recuperar la información más reciente sobre las comprobaciones que produjeron error en la flota de dispositivos: 

    ```azurecli
    let lastDates = SecurityIoTRawEvent | 
    
    where RawEventName == "OSBaseline" | 
    
    summarize TimeStamp=max(TimeStamp) by DeviceId; 
    
    lastDates | join kind=inner (SecurityIoTRawEvent) on TimeStamp, DeviceId  | 
    
    extend event = parse_json(EventDetails) | 
    
    where event.Result == "FAIL" | 
    
    project DeviceId, event.CceId, event.Description 
    ```
 
- **Error de dispositivo específico**: ejecute la siguiente consulta para recuperar la información más reciente sobre las comprobaciones que generaron error en un dispositivo concreto:  

    ```azurecli
    let LastEvents = SecurityIoTRawEvent | 
    
    where RawEventName == "OSBaseline" | 
    
    where DeviceId == "<device-id>" | 
    
    top 1 by TimeStamp desc | 
    
    project IoTRawEventId; 
    
    LastEvents | join kind=leftouter SecurityIoTRawEvent on IoTRawEventId | 
    
    extend event = parse_json(EventDetails) | 
    
    where event.Result == "FAIL" | 
    
    project DeviceId, event.CceId, event.Description 
    ```

- **Error de dispositivo específico**: ejecute la siguiente consulta para recuperar la información más reciente sobre las comprobaciones que generaron error en un dispositivo concreto: 

    ```azurecli
    let LastEvents = SecurityIoTRawEvent | 
    
    where RawEventName == "OSBaseline" | 
    
    where DeviceId == "<device-id>" | 
    
    top 1 by TimeStamp desc | 
    
    project IoTRawEventId; 
    
    LastEvents | join kind=leftouter SecurityIoTRawEvent on IoTRawEventId | 
    
    extend event = parse_json(EventDetails) | 
    
    where event.Result == "ERROR" | 
    
    project DeviceId, event.CceId, event.Description 
    ```
 
- **Actualizar la lista de dispositivos de la flota de dispositivos que no superó una comprobación concreta**: ejecute esta consulta para recuperar la lista actualizada de dispositivos (en la flota del dispositivo) que no pudieron realizar una comprobación específica:  
 
    ```azurecli
    let lastDates = SecurityIoTRawEvent | 
    
    where RawEventName == "OSBaseline" | 
    
    summarize TimeStamp=max(TimeStamp) by DeviceId; 
    
    lastDates | join kind=inner (SecurityIoTRawEvent) on TimeStamp, DeviceId  | 
    
    extend event = parse_json(EventDetails) | 
    
    where event.Result == "FAIL" | 
    
    where event.CceId contains "6.2.8" | 
    
    project DeviceId; 
    ```
 
## <a name="next-steps"></a>Pasos siguientes

[Investigación de recomendaciones de seguridad](quickstart-investigate-security-recommendations.md).
 