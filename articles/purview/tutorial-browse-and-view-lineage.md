---
title: 'Tutorial: Examen de recursos en Azure Purview y visualización de su linaje'
description: En este tutorial se describe cómo buscar recursos en el catálogo y ver el linaje de los datos.
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: tutorial
ms.date: 12/01/2020
ms.openlocfilehash: 7ffbe2ded44ded4f580655f6ae9e98391490f94a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97696103"
---
# <a name="tutorial-browse-assets-in-azure-purview-preview-and-view-their-lineage"></a>Tutorial: Examen de recursos en Azure Purview (versión preliminar) y visualización de su linaje

> [!IMPORTANT]
> Azure Purview se encuentra actualmente en VERSIÓN PRELIMINAR. Los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

En este tutorial se muestra cómo examinar recursos en Azure Purview y visualizar detalles importantes, como el linaje.

Este tutorial constituye la *parte 3 de una serie de tutoriales de cinco partes* en la que aprenderá los aspectos básicos de cómo administrar la gobernanza de datos en un patrimonio de datos mediante Azure Purview. Este tutorial se basa en el trabajo completado en la [parte 2: Navegación por la página principal de Azure Purview (versión preliminar) y búsqueda de un recurso](tutorial-asset-search.md).

En este tutorial va a:

> [!div class="checklist"]
>
> * Buscar recursos en el catálogo.
> * Ver el linaje de recursos.

## <a name="prerequisites"></a>Prerrequisitos

* Complete el [Tutorial: Navegación por la página principal de Azure Purview (versión preliminar) y búsqueda de un recurso](tutorial-asset-search.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="browse-for-assets-in-the-catalog"></a>Búsqueda de recursos en el catálogo

En el tutorial anterior, ha aprendido cómo puede la búsqueda ayudarle a descubrir recursos en el catálogo de Azure Purview. Otra manera de descubrir recursos en el catálogo es utilizar la experiencia de exploración del catálogo.

En esta sección se muestra cómo examinar el catálogo de Azure Purview.

1. Vaya al recurso de Azure Purview en Azure Portal y seleccione **Abrir Purview Studio**. Se le dirigirá automáticamente a la página principal de Purview Studio.

1. En la página **Inicio**, seleccione **Examinar recursos**.

   Aparece la página **Examinar recursos**, que muestra un resumen de todos los tipos de recursos del catálogo.

1. Para explorar los distintos recursos de tipo Azure Data Lake Gen2 disponibles en el catálogo, seleccione el icono **Azure Data Lake Gen2**.

   :::image type="content" source="./media/tutorial-browse-and-view-lineage/browse-by-asset-type.png" alt-text="Captura de pantalla que muestra la página Examinar por tipo de recurso con Azure Data Lake Gen2 seleccionado.":::

1. Si hay varios recursos, puede seleccionar el encabezado de columna **Nombre** para ordenar los recursos alfabéticamente. En este tutorial, solo hay un recurso de Azure Data Lake Storage Gen2.

1. Seleccione el nombre del recurso.

1. Seleccione el conjunto de recursos **Contoso_GrossProfit_{N}.ssv**. Si no existe este recurso en el catálogo, elija otro.

   :::image type="content" source="media/tutorial-browse-and-view-lineage/view-adls-assets.png" alt-text="Lista de recursos de Azure Data Lake Storage Gen2":::

## <a name="view-the-lineage-of-assets"></a>Visualización del linaje de los recursos

En la página de detalles del recurso, explore el origen de los datos.

1. Seleccione la pestaña **Linaje** del conjunto de recursos **Contoso_GrossProfit_{N}.ssv**.

   Se muestra el recurso seleccionado. La información del linaje que ve muestra que este conjunto de recursos se ha copiado de la cuenta de Azure Blob Storage en Azure Data Lake Storage Gen2.

   :::image type="content" source="./media/tutorial-browse-and-view-lineage/view-lineage.png" alt-text="Captura de pantalla que muestra la vista del linaje del recurso.":::

1. Seleccione el recurso de blob en esta página y, después, el vínculo **Switch to asset** (Cambiar al recurso).

   Observe que la ventana ha cambiado al conjunto de recursos de Azure Blob, que era el origen de la instancia de Azure Data Lake Storage Gen2 original.

1. Seleccione la pestaña **Información general** para investigar el recurso y confirmar sus detalles.

Para más información sobre cómo crear una actividad de flujo de datos de Azure Data Factory entre un conjunto de recursos de Azure Blob y de Azure Data Lake Storage Gen2, y observar el linaje, consulte el artículo sobre la [actividad de flujo de datos de Azure Data Factory](../data-factory/concepts-data-flow-overview.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
>
> * Buscar recursos en el catálogo.
> * Ver el linaje de recursos.

Prosiga con el siguiente tutorial para ver información sobre los conjuntos de recursos, detalles de recursos, esquemas y clasificaciones.

> [!div class="nextstepaction"]
> [Conjuntos de recursos, detalles de recursos, esquemas y clasificaciones](tutorial-schemas-and-classifications.md)
