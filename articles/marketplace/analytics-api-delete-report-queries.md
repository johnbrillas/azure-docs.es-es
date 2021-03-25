---
title: Eliminación de API de consultas de informe
description: Use esta API para eliminar consultas definidas por el usuario para el análisis del marketplace comercial.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 4fc3479f1e35970a97684396a7a2e0c0c2582128
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583656"
---
# <a name="delete-report-queries-api"></a>Eliminación de API de consultas de informe

Esta API elimina consultas definidas por el usuario.

**Sintaxis de la solicitud**

| **Método** | **URI de solicitud** |
| --- | --- |
| Delete | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries/{queryId}` |

**Encabezado de solicitud**

| **Header** | **Tipo** | **Descripción** |
| --- | --- | --- |
| Authorization | string | Necesario. Token de acceso de Azure Active Directory (Azure AD) con el formato `Bearer <token>` |
| Content-Type | string | `Application/JSON` |

**Parámetro de ruta de acceso**

| **Nombre del parámetro** | **Tipo** | **Descripción** |
| --- | --- | --- |
| `queryId` | cadena | Filtrar para obtener los detalles de solo las consultas con el id. dado en este argumento |

**Parámetro de consulta**

Ninguno

**Carga útil de solicitud**

Ninguno

**Glosario**

Ninguno

**Respuesta**

La carga de respuesta tiene la estructura siguiente en formato JSON.

Código de respuesta: 200, 400, 401, 403, 404, 500

Carga de respuesta:

```json
{
  "Value": [
    {
      "QueryId": "string",
      "Name": "string",
      "Description": "string",
      "Query": "string",
      "Type": "string",
      "User": "string",
      "CreatedTime": "string",
      "ModifiedTime": "string"
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
| `QueryId` | UUID único de la consulta eliminada. |
| `Name` | Nombre de la consulta eliminada. |
| `Description` | Descripción de la consulta eliminada. |
| `Query` | Cadena de consulta de informe de la consulta eliminada. |
| `Type` | userDefined. |
| `User` | Id. de usuario que creó la consulta. |
| `CreatedTime` | Hora en que se creó la consulta. |
| `ModifiedTime` | Null |
| `TotalCount` | Número de conjuntos de datos en la matriz de valores. |
| `StatusCode` | Código de resultado. Los valores posibles son 200, 400, 401, 403 y 500. |
