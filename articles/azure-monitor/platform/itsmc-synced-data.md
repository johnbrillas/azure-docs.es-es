---
title: Datos sincronizados desde el producto de ITSM con el área de trabajo de LA
description: En este artículo se proporciona información general sobre los datos sincronizados desde el producto de ITSM con el área de trabajo de LA.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: fd570950190ceabac413aca2d68368e5e722a3da
ms.sourcegitcommit: 31d242b611a2887e0af1fc501a7d808c933a6bf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811973"
---
# <a name="data-synced-from-your-itsm-product"></a>Datos sincronizados desde el producto de ITSM

Los incidentes y las solicitudes de cambio se sincronizan desde la herramienta de ITSM con el área de trabajo de Log Analytics según la configuración de la conexión (mediante el campo "Sincronización de datos"):
* [ServiceNow](./itsmc-connections-servicenow.md)
* [System Center Service Manager](./itsmc-connections-scsm.md)
* [Cherwell](./itsmc-connections-cherwell.md)
* [Provance](./itsmc-connections-provance.md)

## <a name="synced-data"></a>Datos sincronizados

En esta sección se muestran algunos ejemplos de datos recopilados por ITSMC.

Los campos de **ServiceDesk_CL** varían según el tipo de elemento de trabajo que importe en Log Analytics. Esta es una lista de campos para dos tipos de elementos de trabajo:

**Elemento de trabajo:** **Incidentes**  
ServiceDeskWorkItemType_s="Incidente"

**Fields**

- ServiceDeskConnectionName
- Service Desk ID
- State
- Urgencia
- Impacto
- Priority
- Escalado
- Creado por
- Resuelto por
- Cerrado por
- Source
- Asignado a
- Category
- Título
- Descripción
- Fecha de creación
- Fecha de cierre
- Fecha de resolución
- Fecha de última modificación
- Computer

**Elemento de trabajo:** **Solicitudes de cambio**

ServiceDeskWorkItemType_s="ChangeRequest"

**Fields**
- ServiceDeskConnectionName
- Service Desk ID
- Creado por
- Cerrado por
- Source
- Asignado a
- Título
- Tipo
- Category
- State
- Escalado
- Estado del conflicto
- Urgencia
- Priority
- Riesgo
- Impacto
- Asignado a
- Fecha de creación
- Fecha de cierre
- Fecha de última modificación
- Fecha de solicitud
- Fecha de inicio planeada
- Fecha de finalización planeada
- Fecha de inicio del trabajo
- Fecha de finalización del trabajo
- Descripción
- Computer

## <a name="servicenow-example"></a>Ejemplo de ServiceNow 
### <a name="output-data-for-a-servicenow-incident"></a>Datos de salida para un incidente de ServiceNow

| Campo de Log Analytics | Campo de ServiceNow |
|:--- |:--- |
| ServiceDeskId_s| Number |
| IncidentState_s | State |
| Urgency_s |Urgencia |
| Impact_s |Impacto|
| Priority_s | Priority |
| CreatedBy_s | Abierto por |
| ResolvedBy_s | Resuelto por|
| ClosedBy_s  | Cerrado por |
| Source_s| Tipo de contacto |
| AssignedTo_s | Asignado a  |
| Category_s | Category |
| Title_s|  Descripción breve |
| Description_s|  Notas |
| CreatedDate_t|  Abierto |
| ClosedDate_t| closed|
| ResolvedDate_t|Resuelto|
| Computer  | Elemento de configuración |

### <a name="output-data-for-a-servicenow-change-request"></a>Datos de salida para una solicitud de cambio de ServiceNow

| Log Analytics | Campo de ServiceNow |
|:--- |:--- |
| ServiceDeskId_s| Number |
| CreatedBy_s | Solicitado por |
| ClosedBy_s | Cerrado por |
| AssignedTo_s | Asignado a  |
| Title_s|  Descripción breve |
| Type_s|  Tipo |
| Category_s|  Category |
| CRState_s|  State|
| Urgency_s|  Urgencia |
| Priority_s| Priority|
| Risk_s| Riesgo|
| Impact_s| Impacto|
| RequestedDate_t  | Solicitado por fecha |
| ClosedDate_t | Fecha de cierre |
| PlannedStartDate_t  | Fecha de inicio planeada |
| PlannedEndDate_t  | Fecha de finalización planeada |
| WorkStartDate_t  | Fecha de inicio real |
| WorkEndDate_t | Fecha de finalización real|
| Description_s | Descripción |
| Computer  | Elemento de configuración |

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)
