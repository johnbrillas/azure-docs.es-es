---
title: Consulta entre recursos en Azure Data Explorer mediante Azure Monitor
description: Use Azure Monitor para realizar consultas entre productos entre Azure Data Explorer, áreas de trabajo de Log Analytics y aplicaciones clásicas de Application Insights en Azure Monitor.
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 12/02/2020
ms.openlocfilehash: 5cb2f7b3b07c20e09d61e97412bc35f03b15cb3b
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96572157"
---
# <a name="cross-resource-query-azure-data-explorer-using-azure-monitor"></a>Consulta entre recursos en Azure Data Explorer mediante Azure Monitor
Azure Monitor admite las consultas entre los servicios Azure Data Explorer, [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview) y [Log Analytics (LA)](/azure/azure-monitor/platform/data-platform-logs). Asimismo, puede consultar el clúster de Azure Data Explorer mediante Log Analytics o las herramientas de Application Insights y hacer referencia al mismo en una consulta entre servicios. En el artículo se muestra cómo realizar una consulta entre servicios.

El flujo entre los servicios de Azure Monitor: :::image type="content" source="media\azure-data-explorer-monitor-proxy\azure-monitor-data-explorer-flow.png" alt-text="Flujo entre los servicios Azure Monitor y Azure Data Explorer.":::

>[!NOTE]
>* La consulta entre servicios de Azure Monitor está en versión preliminar privada. Se requiere AllowListing.
>* Póngase en contacto con el [equipo de servicio](mailto:ADXProxy@microsoft.com) si tiene alguna duda.
## <a name="cross-query-your-log-analytics-or-application-insights-resources-and-azure-data-explorer"></a>Consulta cruzada de las instancias de Log Analytics o de recursos de Application Insights y Azure Data Explorer

Puede ejecutar las consultas entre recursos usando herramientas de cliente que admitan consultas de Kusto, como: Interfaz de usuario web de Log Analytics, Workbooks, PowerShell, API de REST y mucho más.

* Escriba el identificador de un clúster de Azure Data Explorer en una consulta dentro del patrón 'adx' seguido del nombre y la tabla de la base de datos.

```kusto
adx('https://help.kusto.windows.net/Samples').StormEvents
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-cross-service-query-example.png" alt-text="Ejemplo de consulta entre servicios.":::

> [!NOTE]
>* Los nombres de las bases de datos distinguen mayúsculas de minúsculas.
>* No se admite la consulta entre recursos como una alerta.
## <a name="combining-azure-data-explorer-cluster-tables-using-union-and-join-with-la-workspace"></a>Combinación de tablas de clúster de Azure Data Explorer (mediante unión y combinación) con el área de trabajo de LA.

```kusto
union customEvents, adx('https://help.kusto.windows.net/Samples').StormEvents
| take 10
```
```kusto
let CL1 = adx('https://help.kusto.windows.net/Samples').StormEvents;
union customEvents, CL1 | take 10
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-union-cross-query.png" alt-text="Ejemplo de consulta entre servicios con unión.":::

>[!Tip]
>* Se permite el formato abreviado- ClusterName/InitialCatalog. Por ejemplo, adx ('help/Samples') se traduce a adx ('help.kusto.windows.net/Samples')
## <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Combinación de datos desde un clúster de Azure Data Explorer de un inquilino con un recurso de Azure Monitor de otro inquilino

No se admiten las consultas entre inquilinos en los servicios. Ha iniciado sesión en un solo inquilino para ejecutar la consulta que abarca ambos recursos.

Si el recurso de Azure Data Explorer está en el inquilino "A" y el área de trabajo de Log Analytics está en el inquilino "B", use uno de los dos métodos siguientes:

*  Azure Data Explorer permite agregar roles para entidades de seguridad de distintos inquilinos. Agregue su identificador de usuario en el inquilino "B" como usuario autorizado en el clúster de Azure Data Explorer. Compruebe que la propiedad *["TrustedExternalTenant"](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* del clúster de Azure Data Explorer contiene al inquilino "B". Ejecute la consulta cruzada completamente en el inquilino "B".
*  Use [Lighthouse](https://docs.microsoft.com/azure/lighthouse/) para proyectar el recurso de Azure Monitor en el inquilino "A".
## <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Conexión a clústeres de Azure Data Explorer desde otros inquilinos

Kusto Explorer inicia sesión automáticamente en el inquilino al que pertenece originalmente la cuenta de usuario. Para acceder a los recursos de otros inquilinos con la misma cuenta de usuario, se tiene que especificar explícitamente el valor de `tenantId` en la cadena de conexión: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=`**TenantId**

## <a name="next-steps"></a>Pasos siguientes
* [Escritura de consultas](https://docs.microsoft.com/azure/data-explorer/write-queries)
* [Consulta de datos en Azure Monitor mediante Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/query-monitor-data)
* [Consulta de registros entre recursos en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/cross-workspace-query)
