---
title: 'Tutorial: Exploración de conjuntos de recursos, detalles, esquemas y clasificaciones en Azure Purview (versión preliminar)'
description: En este tutorial se describe cómo utilizar conjuntos de recursos, detalles de recursos, esquemas y clasificaciones.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: tutorial
ms.date: 12/01/2020
ms.openlocfilehash: c39ed746630c646b0ce305a9535ae6d610f5161c
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550016"
---
# <a name="tutorial-explore-resource-sets-details-schemas-and-classifications-in-azure-purview-preview"></a>Tutorial: Exploración de conjuntos de recursos, detalles, esquemas y clasificaciones en Azure Purview (versión preliminar)

> [!IMPORTANT]
> Azure Purview se encuentra actualmente en VERSIÓN PRELIMINAR. Los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

En este tutorial, explorará los principales componentes del catálogo: conjuntos de recursos, detalles de recursos, esquemas y clasificaciones.

Este tutorial constituye la *parte 4 de una serie de tutoriales de cinco partes* en la que aprenderá los aspectos básicos de cómo administrar la gobernanza de datos en un patrimonio de datos mediante Azure Purview. Este tutorial se basa en el trabajo completado en la [parte 3: Examen de recursos en Azure Purview (versión preliminar) y visualización de su linaje](tutorial-browse-and-view-lineage.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Ver conjuntos de recursos.
> * Ver detalles de recursos.
> * Editar el esquema y agregar clasificaciones.

## <a name="prerequisites"></a>Prerrequisitos

* Complete el [Tutorial:  Examen de recursos en Azure Purview (versión preliminar) y visualización de su linaje](tutorial-browse-and-view-lineage.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="view-resource-sets"></a>Visualización de conjuntos de recursos

Un conjunto de recursos es un objeto único del catálogo que representa muchos objetos físicos de un almacenamiento. Normalmente, los objetos comparten un esquema común y, en la mayoría de los casos, una convención de nomenclatura o una estructura de carpetas. Por ejemplo, el formato de fecha es *aaaa/mm/dd*. Para más información sobre los conjuntos de recursos, consulte [Descripción de conjuntos de recursos](concept-resource-sets.md).

1. Vaya al recurso de Azure Purview en Azure Portal y seleccione **Abrir Purview Studio**. Se le dirigirá automáticamente a la página principal de Purview Studio.

2. Escriba **contoso_staging_positivecashflow_ n** en el cuadro **Buscar recursos** y, a continuación, seleccione **Contoso_Staging_PositiveCashFlow_{N}.csv** en los resultados de la búsqueda.

## <a name="view-asset-details"></a>Visualización de detalles de recursos

1. En la pestaña **Información general** se muestran **Descripción**, **Términos del glosario** y **Propiedades**, como **qualifiedName**.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/overview-tab.png" alt-text="Captura de pantalla que muestra la pestaña Información general de una página de recursos de un conjunto de recursos.":::

1. En **Propiedades**, tenga en cuenta los dos campos siguientes:

   * **partitionCount**: indica el número de archivos físicos que están asociados a este conjunto de recursos.
   * **schemaCount**: indica el número de variaciones de esquema que se han encontrado en este conjunto de recursos.

   Azure Purview rellena estas propiedades en un plazo de 24 horas después de completar el examen de la [parte 1 de esta serie de tutoriales](tutorial-scan-data.md).

1. Seleccione la pestaña **Contactos** para revisar los valores **Expertos** y **Propietarios**.

## <a name="edit-the-schema-and-add-classifications"></a>Edición del esquema e incorporación de clasificaciones

1. Seleccione la pestaña **Esquema**. Observe los nombres de columna y las clasificaciones que están asociados a ellos. Observe que el examen ha rellenado las propiedades automáticamente.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/schema-tab.png" alt-text="Captura de pantalla que muestra la pestaña Esquema.":::

1. Para editar el recurso, seleccione el botón **Editar** en la parte superior izquierda. A continuación, seleccione la pestaña **Esquema**.

1. Agregue una **Descripción** para una columna o cambie el nombre de la columna por un nombre más descriptivo.

1. Agregue una clasificación en el nivel de recurso seleccionando la lista desplegable **Column level classification** (Clasificación de nivel de columna) para un nombre de columna que no tenga una clasificación de conjunto.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/edit-schema.png" alt-text="Página de edición del esquema":::

1. Cuando haya terminado con los cambios, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
>
> * Ver conjuntos de recursos.
> * Ver detalles de recursos.
> * Editar el esquema y agregar clasificaciones.

Prosiga en el siguiente tutorial para ver información sobre el glosario y cómo definir e importar nuevos términos para recursos.

> [!div class="nextstepaction"]
> [Creación e importación de términos del glosario](tutorial-import-create-glossary-terms.md)