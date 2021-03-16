---
title: Obtención de API de informe
description: Use esta API para obtener informes de análisis programados en el Centro de partners.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 3383af447f40ea984bce9cbc956f22ee6c5af200
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102583593"
---
# <a name="get-report-api"></a>Obtención de API de informe

Esta API obtiene todos los informes que se han programado.

**Sintaxis de la solicitud**

| **Método** | **URI de solicitud** |
| --- | --- |
| GET | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport?reportId={Report ID}&reportName={Report Name}&queryId={Query ID} ` |

**Encabezado de solicitud**

| **Header** | **Tipo** | **Descripción** |
| --- | --- | --- |
| Authorization | string | Necesario. El token de acceso de Azure Active Directory (Azure AD) con el formato `Bearer <token>`. |
| Content-Type | string | `Application/JSON` |

**Parámetro de ruta de acceso**

Ninguno

**Parámetro de consulta**

| **Nombre de parámetro** | **Obligatorio** | **Tipo** | **Descripción** |
| --- | --- | --- | --- |
| `reportId` | No | string | Filtrar para obtener los detalles de solo los informes con el elemento `reportId` especificado en este argumento |
| `reportName` | No | string | Filtrar para obtener los detalles de solo los informes con el elemento `reportName` especificado en este argumento |
| `queryId` | No | boolean | Incluir consultas predefinidas por el sistema en la respuesta |

**Glosario**

Ninguno

**Respuesta**

La carga de respuesta tiene la estructura en formato JSON siguiente:

Código de respuesta: 200, 400, 401, 403, 404, 500

Carga de respuesta:

```json
{
  "Value": [
    {
      "ReportId": "string",
      "ReportName": "string",
      "Description": "string",
      "QueryId": "string",
      "Query": "string",
      "User": "string",
      "CreatedTime": "string",
      "ModifiedTime": "string",
      "StartTime": "string",
      "ReportStatus": "string",
      "RecurrenceInterval": 0,
      " RecurrenceCount": 0,
      "CallbackUrl": "string",
      "Format": "string"
    }
  ],
  "TotalCount": 0,
  "Message": "string",
  "StatusCode": 0
}
```

**Glosario**

En esta tabla se indican las definiciones clave de los elementos de la respuesta.

| **Parámetro** | **Descripción** |
| --- | --- |
| `ReportId` | UUID único del informe que se creó. |
| `ReportName` | Nombre dado al informe en la carga útil de la solicitud. |
| `Description` | Descripción dada cuando se creó el informe. |
| `QueryId` | Id. de consulta pasado en el momento en que se creó el informe. |
| `Query` | Texto de la consulta que se ejecutará para este informe. |
| `User` | Id. de usuario usado para crear el informe. |
| `CreatedTime` | Hora de creación del informe. El formato de la hora es aaaa-MM-ddTHH:mm:ssZ. |
| `ModifiedTime` | Hora en que se modificó el informe por última vez. El formato de la hora es aaaa-MM-ddTHH:mm:ssZ. |
| `StartTime` | Se iniciará la ejecución de la hora. El formato de la hora es aaaa-MM-ddTHH:mm:ssZ. |
| `ReportStatus` | Estado de la ejecución del informe. Los valores posibles son en pausa, activo e inactivo. |
| `RecurrenceInterval` | Intervalo de periodicidad proporcionado durante la creación del informe. |
| `RecurrenceCount` | Recuento de periodicidad proporcionado durante la creación del informe. |
| `CallbackUrl` | Dirección URL de devolución de llamada proporcionada en la solicitud. |
| `Format` | Formato de los archivos de informe. |
