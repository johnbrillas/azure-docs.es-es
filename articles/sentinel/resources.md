---
title: Recursos útiles al trabajar con Azure Sentinel | Microsoft Docs
description: En este documento encontrará una lista de recursos de gran utilidad al trabajar con Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 9b4c8e38-c986-4223-aa24-a71b01cb15ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2021
ms.author: yelevin
ms.openlocfilehash: 11c5c77623fcc693210d5a42bf94c968e884fec3
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98920027"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Recursos útiles al trabajar con Azure Sentinel



En este artículo se enumeran los recursos que sirven para obtener más información sobre cómo trabajar con Azure Sentinel.

- **Conectores de Azure Logic Apps**: <https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Informes y auditoría
Los registros de auditoría de Azure Sentinel se mantienen en los [registros de actividades de Azure](../azure-monitor/platform/platform-logs-overview.md).

Se pueden auditar las siguientes operaciones admitidas.

|Nombre de la operación|    Tipo de recurso|
|----|----|
|Crear o actualizar libro  |Microsoft.Insights/workbooks|
|Eliminar libro    |Microsoft.Insights/workbooks|
|Establecer flujo de trabajo   |Microsoft.Logic/workflows|
|Eliminar flujo de trabajo    |Microsoft.Logic/workflows|
|Crear búsqueda guardada    |Microsoft.OperationalInsights/workspaces/savedSearches|
|Eliminar búsqueda guardada    |Microsoft.OperationalInsights/workspaces/savedSearches|
|Actualizar reglas de alerta |Microsoft.SecurityInsights/alertRules|
|Eliminar reglas de alerta |Microsoft.SecurityInsights/alertRules|
|Actualizar acciones de respuesta de la regla de alerta |Microsoft.SecurityInsights/alertRules/actions|
|Eliminar acciones de respuesta de la regla de alerta |Microsoft.SecurityInsights/alertRules/actions|
|Actualizar marcadores   |Microsoft.SecurityInsights/bookmarks|
|Eliminar marcadores   |Microsoft.SecurityInsights/bookmarks|
|Actualizar casos   |Microsoft.SecurityInsights/Cases|
|Actualizar la investigación de casos  |Microsoft.SecurityInsights/Cases/investigations|
|Crear comentarios de los casos   |Microsoft.SecurityInsights/Cases/comments|
|Actualizar los conectores de datos |Microsoft.SecurityInsights/dataConnectors|
|Eliminar los conectores de datos |Microsoft.SecurityInsights/dataConnectors|
|Actualizar la configuración    |Microsoft.SecurityInsights/settings|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Ver los datos de auditoría e informes en Azure Sentinel

Puede ver estos datos transmitiéndolos desde el registro de actividades de Azure a Azure Sentinel, donde podrá realizar investigaciones con ellos y analizarlos.

1. Conecte el origen de datos de las [actividades de Azure](connect-azure-activity.md). Después de hacerlo, los eventos de auditoría se transmiten a una nueva tabla en la pantalla de **registros** denominada AzureActivity.

1. A continuación, consulte los datos mediante KQL, tal como lo haría con cualquier otra tabla.

    Por ejemplo, para averiguar quién fue el último usuario en editar una regla de análisis determinada, use la consulta siguiente (reemplazando `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` por el identificador de la regla que desea comprobar):

    ```kusto
    AzureActivity
    | where OperationNameValue startswith "MICROSOFT.SECURITYINSIGHTS/ALERTRULES/WRITE"
    | where Properties contains "alertRules/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    | project Caller , TimeGenerated , Properties
    ```



## <a name="vendor-documentation"></a>Documentación del proveedor

| **Proveedor**  | **Uso de incidentes en Azure Sentinel** | **Vínculo**|
|----|----|----|
| GitHub| Utilizado para acceder a la página de la comunidad| <https://github.com/Azure/Azure-Sentinel> |
| PaloAlto| Configuración de CEF| <https://www.paloaltonetworks.com/documentation/misc/cef.html>|
| PluralSight | Curso del lenguaje de consulta Kusto| [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)|

## <a name="blogs-and-forums"></a>Blogs y foros

¡Nos encanta tener escuchar a nuestros usuarios!

- **Publique sus preguntas** en el [espacio de TechCommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) para Azure Sentinel. 

- **Envíe sugerencias para obtener mejoras** a través de nuestro programa [Voz del usuario](https://feedback.azure.com/forums/920458-azure-sentinel).

- **Vea y comente** nuestras entradas de blog de Azure Sentinel:

    - [TechCommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) 
    - [Microsoft Azure](https://azure.microsoft.com/blog/tag/azure-sentinel/)

Para información adicional sobre la seguridad y el cumplimiento de Azure, consulte el [blog sobre seguridad y cumplimiento de Microsoft Azure](https://techcommunity.microsoft.com/t5/microsoft-security-and/bg-p/MicrosoftSecurityandCompliance).


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener la certificación](/learn/paths/security-ops-sentinel/)

> [!div class="nextstepaction"]
> [Leer historias de casos de uso de clientes](https://customers.microsoft.com/en-us/search?sq=%22Azure%20Sentinel%20%22&ff=&p=0&so=story_publish_date%20desc)

