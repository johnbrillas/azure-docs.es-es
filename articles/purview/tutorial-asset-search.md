---
title: 'Tutorial: Navegación por la página principal de Azure Purview y búsqueda de un recurso'
description: En este tutorial se describe cómo utilizar las características de la página principal de Azure Purview y cómo buscar en el catálogo.
author: hophanms
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: tutorial
ms.date: 12/01/2020
ms.openlocfilehash: 46c2ee284c33cc8ffb7b0bd3bc536905cf4e2052
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97399883"
---
# <a name="tutorial-navigate-the-azure-purview-preview-home-page-and-search-for-an-asset"></a>Tutorial: Navegación por la página principal de Azure Purview (versión preliminar) y búsqueda de un recurso

> [!IMPORTANT]
> Azure Purview se encuentra actualmente en VERSIÓN PRELIMINAR. Los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

En este tutorial, se familiarizará con Azure Purview navegando por las características de la página principal y buscando un recurso en el catálogo.

Este artículo constituye la *parte 2 de una serie de tutoriales de cinco partes* en la que aprenderá los aspectos básicos de cómo administrar la gobernanza de datos en un patrimonio de datos mediante Azure Purview. Este tutorial se basa en el trabajo completado en [Parte 1: Examen de datos con Azure Purview](tutorial-scan-data.md)

En este tutorial va a:

> [!div class="checklist"]
>
> * Navegar por la página principal de Azure Purview.
> * Buscar un recurso.
> * Agregar un propietario de recursos.

## <a name="prerequisites"></a>Prerrequisitos

* Complete el [Tutorial: Examen de datos con Azure Purview](tutorial-scan-data.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="navigate-the-azure-purview-home-page"></a>Navegación por la página principal de Azure Purview

Los pasos siguientes le guiarán por la página principal de Azure Purview.

1. Vaya al recurso de Azure Purview en Azure Portal y seleccione **Abrir Purview Studio**. Se le dirigirá automáticamente a la página principal de Purview Studio.

   En la parte superior de la página principal se muestra el nombre del catálogo y un conjunto de análisis del catálogo. Se incluyen el número de usuarios, los recursos de datos y los términos del glosario. En el resumen, puede ver que hay más de 200 recursos en total y 113 términos de glosario. Este número se actualiza a medida que su organización examina y agrega más términos a Azure Purview.

   :::image type="content" source="./media/tutorial-asset-search/purview-home-page.png" alt-text="Captura de pantalla que muestra la página principal de Azure Purview.":::

1. Seleccione **Examinar recursos** para ver todos los recursos.

## <a name="search-for-an-asset"></a>Búsqueda de un recurso

Antes de empezar, presentamos un rápido recordatorio de algunos términos fundamentales:

* **Recurso**: un objeto único del catálogo que es conciso y que contiene la definición de cualquier entidad del patrimonio de datos. Entre los ejemplos de entidades se incluyen una tabla SQL, un informe de Power BI y la actividad de la factoría de datos.
  
* **Esquema**: también conocido como columna o atributo, un esquema representa la estructura de un recurso o un conjunto de recursos.

* **Conjunto de recursos**: un objeto único del catálogo que representa a muchos objetos físicos de un almacenamiento. Normalmente, estos objetos comparten un esquema común y, en la mayoría de los casos, una convención de nomenclatura o una estructura de carpetas. Por ejemplo, el formato de fecha es *aaaa/mm/dd*. Para más información, consulte [Descripción de conjuntos de recursos](concept-resource-sets.md).

* **Tipo de recurso**: agrupación de recursos y conjuntos de recursos bajo un nombre lógico, que se suele asignar al nombre de la plataforma de datos.

Siga estos pasos para buscar un recurso y marcarse a sí mismo como propietario:

1. En el cuadro **Search catalog** (Catálogo de búsqueda) de la página principal, escriba el nombre del grupo de recursos que contiene su patrimonio de datos (el grupo de recursos que creó en el tutorial anterior). Aparecerá una lista de sugerencias.

1. Seleccione **View search results** (Ver resultados de búsqueda). Dado que todos los recursos comienzan con el nombre del grupo de recursos, aparecen todos en los resultados de la búsqueda. Fuera de este tutorial, buscará nombres de recurso específicos, no grupos de recursos.

    :::image type="content" source="./media/tutorial-asset-search/search-suggestions.png" alt-text="Captura de pantalla que muestra la lista de sugerencias de búsqueda del catálogo.":::

1. Puede utilizar los filtros del panel de navegación izquierdo para filtrar por tipo de recurso, clasificación, contacto, etiqueta y término del glosario.

1. Para buscar un conjunto de recursos, empiece a escribir el nombre del conjunto. Aparecerá una lista de sugerencias de búsqueda con las palabras clave correctas. También puede buscar nombres absolutos poniéndolos entre comillas.

   :::image type="content" source="media/tutorial-asset-search/keyword-search.png" alt-text="Búsqueda de recursos de palabras clave de Azure Purview":::

## <a name="edit-an-asset"></a>Edición de un recurso

1. Seleccione uno de los recursos de los resultados de la búsqueda. Después, seleccione **Editar**.

1. En la pestaña **Información general**, puede agregar una descripción.

    :::image type="content" source="./media/tutorial-asset-search/overview-tab.png" alt-text="Captura de pantalla que muestra el campo Descripción en la pestaña Información general.":::

1. Seleccione la pestaña **Contacts** (Contactos). Junto a **Propietarios**, en **Seleccionar usuario o grupo**, empiece a escribir su dirección de correo electrónico corporativa.

    :::image type="content" source="./media/tutorial-asset-search/contacts-tab.png" alt-text="Captura de pantalla que muestra los campos rellenados.":::

    Se muestra automáticamente una sugerencia de nombre.

1. Junto a **Expertos**, en **Seleccionar usuario o grupo**, escriba un nombre (por ejemplo, su nombre de administrador) y, después, seleccione **Guardar**.

    Ahora los campos descripción, nombre del propietario y nombre del experto aparecen rellenados.

## <a name="next-steps"></a>Pasos siguientes

Antes de pasar al siguiente tutorial de esta serie, dedique más tiempo a explorar la página principal de Azure Purview. En este tutorial, ha aprendido a:

> [!div class="checklist"]
>
> * Navegar por la página principal de Azure Purview.
> * Buscar un recurso.
> * Agregar un propietario de recursos.

Prosiga con el siguiente tutorial para aprender a buscar recursos en Azure Purview y descubrir el linaje de los recursos.

> [!div class="nextstepaction"]
> [Examen de recursos y visualización de su linaje](tutorial-browse-and-view-lineage.md)
