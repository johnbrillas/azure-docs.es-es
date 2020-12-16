---
title: Conexión de Data Factory a Azure Purview
description: Información sobre la conexión de Data Factory a Azure Purview
services: data-factory
ms.author: lle
author: lrtoyou1223
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom:
- seo-lt-2019
- references_regions
ms.date: 12/3/2020
ms.openlocfilehash: 94b2ed8a25ca5cc837f6677dea1c0bbb54225fcd
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603001"
---
# <a name="connect-data-factory-to-azure-purview-preview"></a>Conexión de Data Factory a Azure Purview (versión preliminar)
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se explica cómo conectar la factoría de datos a Azure Purview y cómo notificar el linaje de datos de las actividades de ADF Copiar datos, Flujo de datos y Ejecutar paquete SSIS.

## <a name="connect-data-factory-to-azure-purview"></a>Conexión de la factoría de datos a Azure Purview
Azure Purview es un nuevo servicio en la nube para los usuarios de datos que administran de forma centralizada la gobernanza de datos en su patrimonio de datos distribuido en entornos locales y de nube. Puede conectar su factoría de datos a Azure Purview. La conexión le permite aprovechar Azure Purview para capturar datos de linaje de Copiar, Flujo de datos y Ejecutar paquete SSIS. Para obtener información sobre cómo registrar Data Factory en Azure ámbito, consulte [Cómo conectar Azure Data Factory y Azure Purview](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory). 

## <a name="report-lineage-data-to-azure-purview"></a>Notificación de datos de linaje a Azure Purview
Cuando los clientes ejecutan las actividades Copiar, Flujo de datos o Ejecutar paquetes SSIS en Azure Data Factory, pueden obtener la relación de dependencia y tener una introducción de alto nivel del proceso de flujo de trabajo completo entre los orígenes de datos y el destino.
Para obtener información sobre cómo recopilar el linaje de Azure Data Factory, consulte el artículo sobre el [linaje de Data Factory](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory#supported-azure-data-factory-activities).

## <a name="next-steps"></a>Pasos siguientes
[Guía de usuario del linaje de Data Catalog](https://docs.microsoft.com/azure/purview/catalog-lineage-user-guide)

[Tutorial: Inserción de datos de linaje de Data Factory en Azure Purview](turorial-push-lineage-to-purview.md)