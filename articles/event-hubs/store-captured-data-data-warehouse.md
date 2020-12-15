---
title: 'Tutorial: Migración de datos de eventos a Azure Synapse Analytics: Azure Event Hubs'
description: Describe cómo usar Azure Event Grid y Azure Functions para migrar datos capturados de Event Hubs a una instancia de Azure Synapse Analytics.
services: event-hubs
ms.date: 12/07/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: 77a2b35dea57c71e9e6f07056bd106df2ac109b8
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96854428"
---
# <a name="tutorial-migrate-captured-event-hubs-data-to-azure-synapse-analytics-using-event-grid-and-azure-functions"></a>Tutorial: Migración de datos capturados de Event Hubs a Azure Synapse Analytics mediante Event Grid y Azure Functions
Azure Event Hubs [Capture](./event-hubs-capture-overview.md) permite capturar automáticamente los datos de transmisión de Event Hubs en Azure Blob Storage o Azure Data Lake Store. En este tutorial se muestra cómo migrar datos capturados de Event Hubs de Azure Storage a Azure Synapse Analytics mediante una función de Azure desencadenada por [Event Grid](../event-grid/overview.md).

[!INCLUDE [event-grid-event-hubs-functions-synapse-analytics.md](../../includes/event-grid-event-hubs-functions-synapse-analytics.md)]

## <a name="next-steps"></a>Pasos siguientes 
Puede usar las eficaces herramientas de visualización de datos con Data Warehouse para conseguir detalles accionables.

En este artículo se muestra cómo usar [Power BI con Azure Synapse Analytics](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect).