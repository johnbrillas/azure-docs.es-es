---
title: Análisis de Azure Batch
description: Los temas de análisis por lotes contienen información de referencia para los eventos y las alertas disponibles para los recursos del servicio de Batch.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: 0d55ecd7f10e267a9cb469dffcdf26c131c8ce41
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91849518"
---
# <a name="batch-analytics"></a>Análisis por lotes

Los temas de esta sección contienen información de referencia para los eventos y las alertas disponibles para los recursos del servicio de Batch.

Vea [Registro de diagnóstico de Azure Batch](./batch-diagnostics.md) para obtener más información sobre cómo habilitar y consumir los registros de diagnóstico por lotes.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

El servicio de Azure Batch genera los siguientes eventos de registro de diagnóstico mientras duren determinados recursos de Batch.

### <a name="service-log-events"></a>Eventos del registro del servicio

- [Creación del grupo](batch-pool-create-event.md)
- [Inicio de eliminación del grupo](batch-pool-delete-start-event.md)
- [Finalización de eliminación del grupo](batch-pool-delete-complete-event.md)
- [Inicio de cambio de tamaño del grupo](batch-pool-resize-start-event.md)
- [Finalización de cambio de tamaño del grupo](batch-pool-resize-complete-event.md)
- [Escalabilidad automática de grupo](batch-pool-autoscale-event.md)
- [Inicio de tarea](batch-task-start-event.md)
- [Finalización de tarea](batch-task-complete-event.md)
- [Error en la tarea](batch-task-fail-event.md)
- [Error en programación de tareas](batch-task-schedule-fail-event.md)
