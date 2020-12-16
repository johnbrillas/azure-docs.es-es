---
title: Información de exámenes sobre los datos de Azure Purview
description: En esta guía paso a paso se describe cómo ver y usar los informes de información de exámenes de Purview sobre los datos.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: 00f72e1de230cdc68f86010b7b25d86debaa5eb5
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96575795"
---
# <a name="scan-insights-on-your-data-in-azure-purview"></a>Información de exámenes sobre los datos de Azure Purview

En esta guía paso a paso se describe cómo obtener acceso, ver y filtrar los informes de información detallada de exámenes de Azure Purview para los datos.

En esta guía paso a paso, aprenderá a:

> [!div class="checklist"]
> * Ver información desde la cuenta de Purview.
> * Obtener una visión general de los exámenes.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a usar la información detallada de Purview, asegúrese de que ha completado los pasos siguientes:

* Ha configurado los recursos de Azure y ha rellenado la cuenta con datos.
* Ha configurado y realizado un examen en el origen de datos.

Para más información, consulte [Administración de orígenes de datos en Azure Purview (versión preliminar)](manage-data-sources.md).

## <a name="use-purview-scan-insights"></a>Usar la información de exámenes de Purview

En Azure Purview, puede registrar y examinar los tipos de origen. Puede ver el estado de los exámenes a lo largo del tiempo en Información de exámenes. La información indica el número de exámenes erróneos, correctos o cancelados dentro de un período de tiempo determinado.

### <a name="view-scan-insights"></a>Visualización de información de exámenes

1. Vaya a la pantalla de instancia **Azure Purview** de Azure Portal y seleccione su cuenta de Purview.

1. En la página **Información general**, en la sección **Inicio**, seleccione el icono **Open Purview Studio** (Abrir Purview Studio).

   :::image type="content" source="./media/scan-insights/portal-access.png" alt-text="Inicio de Purview desde Azure Portal":::

1. En la página **Home** (Inicio) de Purview, seleccione el icono **View insights** (Ver información) para acceder al área **Insights** :::image type="icon" source="media/scan-insights/ico-insights.png" border="false"::: (Información detallada).

   :::image type="content" source="./media/scan-insights/view-insights.png" alt-text="Visualización de información detallada en Azure Portal":::

1. En el área **Insights** (Información detallada) :::image type="icon" source="media/scan-insights/ico-insights.png" border="false":::, seleccione **Scans** (Exámenes) para mostrar el informe **Scan insights** (Información detallada de exámenes).

### <a name="view-high-level-kpis-to-show-count-of-scans-by-status"></a>Ver KPI de alto nivel para mostrar el recuento de exámenes por estado
 
Los KPI de alto nivel muestran los exámenes totales que se ejecutan dentro de un período. El período se toma como valor predeterminado en los últimos 30 días. Sin embargo, también puede seleccionar los últimos siete días. Según el filtro de tiempo, los valores de KPI reflejan el número de exámenes correctamente.


Según el valor del filtro de tiempo seleccionado, puede ver la distribución de los exámenes correctos, con errores y cancelados por semana o por día en el gráfico.

   :::image type="content" source="./media/scan-insights/scan-insights.png" alt-text="Visualización de información de exámenes":::

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los informes de información detallada de Azure Purview con [Información detallada de recursos](./asset-insights.md)
