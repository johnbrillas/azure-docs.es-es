---
title: Agregación de eventos (versión preliminar)
description: Los agentes de seguridad de Defender para IoT recopilan datos y eventos del sistema de un dispositivo local, y envían los datos a la nube de Azure para su procesamiento y análisis.
ms.date: 1/20/2021
ms.topic: conceptual
ms.openlocfilehash: c0280e97549009a1e4911c072a7a8ec052684b4e
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104779331"
---
# <a name="event-aggregation-preview"></a>Agregación de eventos (versión preliminar)

Los agentes de seguridad de Defender para IoT recopilan datos y eventos del sistema de un dispositivo local, y envían los datos a la nube de Azure para su procesamiento y análisis. El microagente de Defender para IoT recopila muchos tipos de eventos de dispositivo, incluidos los nuevos procesos y todos los nuevos eventos de conexión. Tanto el nuevo proceso como los nuevos eventos de conexión pueden producirse con frecuencia en un dispositivo en un segundo. Esta capacidad es importante para proporcionar una seguridad completa; sin embargo, el número de mensajes que envían los agentes de seguridad puede alcanzar rápidamente, o incluso superar, los límites de cuota y costo de IoT Hub. No obstante, estos eventos contienen información de seguridad muy valiosa que es crucial para proteger el dispositivo. 

Para reducir tanto la cuota como los costos adicionales y, al mismo tiempo, mantener los dispositivos protegidos, los agentes de Defender para IoT agregan estos tipos de eventos: 

- ProcessCreate (solo Linux) 

- ConnectionCreate (solo Azure RTOS) 

## <a name="how-does-event-aggregation-work"></a>¿Cómo funciona la agregación de eventos? 

Los agentes de Defender para IoT agregan eventos para el intervalo o el período de tiempo. Una vez transcurrido el período de intervalo, el agente envía los eventos agregados a la nube de Azure para su posterior análisis. Los eventos agregados se almacenan en memoria hasta que se envían a la nube de Azure. 

Cuando el agente recopila un evento idéntico a otro que ya está guardado en la memoria, el agente aumenta el número de llamadas de este evento específico para reducir la superficie de memoria del agente. Cuando ha transcurrido el periodo de agregación, el agente envía el número de llamadas de cada tipo de evento que se ha producido. La agregación de eventos es simplemente la agregación de los números de llamadas de cada tipo de evento recopilado. 

## <a name="process-events"></a>Eventos de proceso 

Actualmente, los eventos de proceso solo se admiten en los sistemas operativos Linux. 

Los eventos de proceso se consideran idénticos cuando la *línea de comandos*  y el  *identificador de usuario*  son idénticos. 

El búfer predeterminado para los eventos de proceso es de 32 procesos, tras los que funcionará de forma cíclica y los eventos de proceso más antiguos se descartarán para dejar espacio a los nuevos eventos de proceso.  

## <a name="network-connection-events"></a>Eventos de conexión de red 

Actualmente, los eventos de conexión de red solo se admiten en Azure RTOS. 

Los eventos de conexión de red se consideran idénticos cuando el *puerto local*, el  *puerto remoto*, el  *protocolo de transporte*, la  *dirección local* y la  *dirección remota* son idénticos. 

El búfer predeterminado para los eventos de conexión de red es 64. No se almacenará en la caché ningún evento de red nuevo hasta el siguiente ciclo de recopilación. Se registrará una advertencia para aumentar el tamaño de la caché.

## <a name="next-steps"></a>Pasos siguientes

Consulte sus [alertas de seguridad de Defender para IoT](concept-security-alerts.md).
