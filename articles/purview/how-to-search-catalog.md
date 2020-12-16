---
title: Cómo realizar búsquedas en Data Catalog
description: En este artículo se proporciona información general sobre cómo realizar búsquedas en un catálogo de datos.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/15/2020
ms.openlocfilehash: 87ca3c49d6a96f083cc50f01f5b9a4a739d688a1
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96551137"
---
# <a name="search-the-azure-purview-data-catalog"></a>Búsqueda en Azure Purview Data Catalog

En este artículo se describe cómo usar las diversas características de búsqueda en Azure Purview Data Catalog.

## <a name="search-the-catalog-for-assets"></a>Búsqueda de recursos en el catálogo

Los pasos para realizar una búsqueda de recursos son los siguientes:

1. [Para abrir el diálogo de búsqueda de recursos](#open-the-asset-search-dialog), seleccione **Search catalog** (Buscar en el catálogo).
1. [Escriba los términos de búsqueda](#enter-search-terms) para buscar los recursos que tengan las características que coincidan con los términos de la búsqueda.
1. [Establezca filtros rápidos](#set-quick-filters) para limitar el ámbito de la búsqueda.
1. [Inicie la búsqueda](#start-the-search) y vaya a los resultados.

No importa si establece filtros rápidos antes o después de escribir los términos de búsqueda.

Si no hay ningún término de búsqueda ni filtros, los resultados de la búsqueda incluirán todos los recursos.

### <a name="open-the-asset-search-dialog"></a>Apertura del cuadro de diálogo de búsqueda de recursos

Para abrir el diálogo de búsqueda de recursos, seleccione **Search catalog** (Buscar en el catálogo).

:::image type="content" source="./media/how-to-search-catalog/search-catalog.png" alt-text="Debajo de la opción &quot;Search catalog&quot; (Buscar en el catálogo) hay un panel izquierdo con filtros de búsqueda y un panel derecho con las búsquedas recientes." border="true":::

En el cuadro de diálogo de búsqueda se muestran los filtros rápidos, el historial de búsqueda y una lista de los recursos a los que se ha obtenido acceso recientemente.

:::image type="content" source="./media/how-to-search-catalog/asset-search-dialog.png" alt-text="La lista de búsqueda se encuentra en el panel derecho, debajo de &quot;Search catalog&quot; (Buscar en el catálogo)&quot;." border="true":::

### <a name="enter-search-terms"></a>Escritura del texto de búsqueda

Escriba uno o varios términos de búsqueda en la opción **Search catalog** (Buscar en el catálogo). A medida que escribe, los términos de búsqueda que coincidan con las búsquedas recientes se enumeran en la opción **Búsquedas recientes**; igualmente, los términos de búsqueda coincidentes sugeridos se muestran en las **Sugerencias de búsqueda**, y los recursos de datos coincidentes se enumeran en **Asset suggestions** (Sugerencias de recursos).

:::image type="content" source="./media/how-to-search-catalog/enter-search-terms.png" alt-text="Captura de pantalla que muestra los resultados de una búsqueda que se está escribiendo en el cuadro de la opción Search catalog (Buscar en el catálogo)":::

Los resultados de la búsqueda incluyen solo aquellos recursos que cuenten con una o más características que coincidan con los términos de la búsqueda. Estas características incluyen el nombre del recurso, el tipo de recurso, las clasificaciones y los contactos.

#### <a name="types-of-search-criteria"></a>Tipos de criterios de búsqueda

Azure Purview admite los siguientes tipos de criterios de búsqueda.

> [!Note]
> Especifique siempre los operadores booleanos de texto (**AND**, **OR**, **NOT**) en mayúsculas. De lo contrario, el caso no es importante, ni tampoco los espacios adicionales.

- **subárbol**: búsqueda de documentos que contengan **subárbol**.
- **base de datos de subárbol**: búsqueda de documentos que contengan exactamente **base de datos de subárbol**.
- **subárbol OR base de datos**: búsqueda de documentos que contengan **subárbol**, **base de datos** o ambos.
- **subárbol AND base de datos**, **subárbol && base de datos**: búsqueda de documentos que contengan tanto **subárbol** como **base de datos**.
- **subárbol AND (base de datos OR almacén)** : búsqueda de documentos que contengan **subárbol** y **base de datos** o **almacén**, o ambos.
- **subárbol NOT base de datos**: búsqueda de documentos que contengan **subárbol**, pero no **base de datos**.
- **sub**: búsqueda de documentos que contengan una palabra que comience por **sub**. Por ejemplo, **sub**, **subárbol**, **subar** (* es un carácter comodín que coincide con cualquier número de caracteres).

### <a name="set-quick-filters"></a>Establecer filtros rápidos

La lista de resultados de la búsqueda se basa en los términos de búsqueda que especifique en la opción **Search catalog** (Buscar en el catálogo) y en los valores que seleccione para los filtros rápidos.

Un filtro rápido limita la lista de resultados de búsqueda a los recursos que tengan un valor seleccionado de una característica. El filtro tiene una lista desplegable y un cuadro de texto. La lista desplegable muestra los valores de la característica que se encuentra en los resultados de la búsqueda *actuales*. Junto a cada valor de la lista hay un recuento del número de recursos que hay en los resultados de las búsqueda actuales que tienen ese valor. Si selecciona un valor de la lista, los resultados de la búsqueda se limitarán a los recursos que tengan ese valor. Recuerde que solo puede seleccionar un valor.

Los resultados de la búsqueda actual que se usan para formar la lista desplegable se determinan de la siguiente manera:

- Los términos de búsqueda que se especifican en la opción **Search catalog** (Buscar en el catálogo). 
- Los valores que se hayan seleccionado en los filtros rápidos.

Aquí tiene un ejemplo del filtro rápido "Tipo de recurso".

:::image type="content" source="./media/how-to-search-catalog/asset-type-quick-filter.png" alt-text="Ejemplo del filtro rápido de tipo de recurso." border="true":::

Para limitar los valores de la lista desplegable a los valores que coincidan total o parcialmente con el texto, puede escribir texto en el cuadro. Si quiere obtener ejemplos de cómo usar el cuadro de texto, consulte [Filtro rápido de búsqueda: filtrar por tipo de recurso](#search-quick-filter-filter-by-asset-type) y [Filtro rápido de búsqueda: filtrar por clasificación](#search-quick-filter-filter-by-classification).

#### <a name="search-quick-filter-filter-by-asset-type"></a>Filtro rápido de búsqueda: filtrar por tipo de recurso

Para filtrar en función del tipo de recurso, use el filtro rápido **Tipo de recurso**. La lista desplegable mostrará los tipos de recursos que se encuentran en los resultados de la búsqueda actual, según lo establecido en los términos de búsqueda y los filtros rápidos. Asimismo, se muestra el número de recursos de cada tipo.

:::image type="content" source="./media/how-to-search-catalog/asset-type-quick-filter.png" alt-text="Los filtros rápidos de tipo de recurso están resaltados. Aquí se muestran los tipos de recursos y un recuento de cada uno." border="true":::

Seleccione un tipo de recurso para limitar los resultados de la búsqueda a los recursos de ese tipo. Recuerde que solo puede seleccionar un tipo.

Para mostrar solo los tipos de recursos cuyos nombres coinciden con una cadena, escriba la cadena en el cuadro de texto. Por ejemplo, para mostrar solo los tipos de recursos que tengan **sql** en el nombre, escriba **sql**.

:::image type="content" source="./media/how-to-search-catalog/filter-asset-types.png" alt-text="El panel filtros rápidos tiene sql como &quot;Tipo de recurso&quot;. Así pues, la lista de recursos que contienen sql muestra tres entradas." border="true":::

Seleccione un tipo de recurso para limitar los resultados de la búsqueda a los recursos de ese tipo. Recuerde que solo puede seleccionar un tipo.

#### <a name="search-quick-filter-filter-by-classification"></a>Filtro rápido de búsqueda: filtrar por clasificación

Para filtrar por clasificación de recursos, use el filtro rápido **Clasificación**. La lista desplegable mostrará los tipos de clasificaciones que se hayan asignado a uno o más recursos en los resultados de la búsqueda actual, según lo establecido en los términos de búsqueda y los filtros rápidos. Asimismo, se muestra al número de recursos asignados a cada clasificación.

:::image type="content" source="./media/how-to-search-catalog/classification-quick-filter.png" alt-text="Aquí se resaltan los filtros rápidos de clasificación." border="true":::

Seleccione una clasificación para limitar los resultados de la búsqueda a los recursos asignados a esa clasificación. Recuerde que solo puede seleccionar una clasificación.

Para mostrar solo los tipos de clasificaciones cuyos nombres coinciden con una cadena, escriba la cadena en el cuadro de texto. Por ejemplo, para mostrar solo las clasificaciones con el valor **number** en sus nombres, escriba **number**.

:::image type="content" source="./media/how-to-search-catalog/filter-classifications.png" alt-text="En el panel de filtros rápidos, la clasificación es &quot;bank&quot; y las clasificaciones que se muestran contienen ese valor." border="true":::

Seleccione una clasificación para limitar los resultados de la búsqueda a los recursos que se hayan asignado a esa clasificación. Recuerde que no puede seleccionar más de una clasificación.

#### <a name="search-quick-filter-filter-by-contacts"></a>Filtro rápido de búsqueda: filtrar por contactos

Un *contacto* es una persona que está asignada a un recurso como propietario o experto. Al ver los detalles de los recursos, los contactos se muestran en la pestaña **Contactos**.

Existen dos maneras de buscar los recursos que tengan asignado un contacto determinado.

- Escriba todo el nombre del contacto (o parte del mismo) en la opción **Search catalog** (Buscar en el catálogo) y realice una búsqueda. Los resultados de la búsqueda incluirán aquellos recursos que tengan contactos cuyos nombres coincidan con los términos de la búsqueda.
- Seleccione el contacto que le interese en el filtro rápido **Contacto** y realice una búsqueda.

:::image type="content" source="./media/how-to-search-catalog/contact-quick-filter.png" alt-text="El valor de tipo Persona en el panel de filtros rápidos es &quot;Darren&quot;. Existen tres sugerencias en el panel de sugerencias." border="true":::

## <a name="search-example"></a>Ejemplo de búsqueda

Veamos un ejemplo hipotético para ver el modo en que los términos de búsqueda y los filtros rápidos interactúan para determinar los resultados de la búsqueda. En concreto, supervisaremos el recuento del tipo de recurso **Azure Blob Storage**.

- Así pues, hacemos la primera búsqueda sin ningún término de búsqueda especificado y sin valores seleccionados en los filtros rápidos. Hecho esto, la búsqueda incluirá todos los recursos del catálogo. La lista de resultados de la búsqueda y el filtro rápido **Tipo de recurso** revelan lo siguiente:

    - La lista de resultados de la búsqueda contiene 164.230 recursos; esto es, todos los recursos del catálogo.
    - La lista desplegable **Tipo de recurso** tiene 43 entradas. Estos son todos los tipos de recursos del catálogo. Puesto que cada recurso es de un solo tipo, la suma de los recuentos de los 43 tipos de recursos es 164.230.
    - El tipo de recurso **Azure Blob Storage** es la primera entrada de la lista desplegable del filtro rápido **Tipo de recurso**. Los valores se ordenan por número y el más grande se muestra primero; así pues, podemos ver que **Azure Blob Storage** es el tipo de recurso más común. El recuento es 118.174.

- Ahora escriba **parquet** en la opción **Search catalog** (Buscar en el catálogo) y realice otra búsqueda. Los resultados de la búsqueda solo incluirán aquellos recursos que cuenten con una o más características que coincidan con el término **parquet**. Esto reduce todos el recuento, tal como se indica a continuación:

    - La lista de resultados de la búsqueda contiene 493 recursos. Solo 493 de los 164.230 recursos del catálogo tienen características que coinciden con "parquet".
    - La lista desplegable **Tipo de recurso** tiene 15 entradas. Cada uno de los 493  recursos es de uno de estos 15 tipos, y la suma de todos ellos es 493.
    - Existen 456 recursos de tipo **Azure Blob Storage**. Los otros 37 recursos (493 menos 456) son de uno de los otros 14 tipos.

- Ahora veremos la lista desplegable de un filtro rápido diferente denominado **Clasificación**:

    - Hay 12 clasificaciones para los 493 recursos de la lista de resultados de la búsqueda. El recuento de los 493 recursos no suma 493, ya que se puede asignar cualquier número de clasificación a un recurso.
    - La clasificación **Nombre de persona** se asigna a 36 recursos, más que cualquier otra clasificación.

- A continuación, seleccionamos la clasificación **Nombre de persona**. La lista de resultados de la búsqueda desciende a 36 recursos según lo esperado, ya que el número de clasificaciones de tipo **Nombre de la persona** era 36. Ninguno de estos resultados son del tipo **Azure Blob Storage**.

Por lo tanto, podemos concluir que no hay ningún recurso cuyo tipo sea **Azure Blob Storage** que coincida con **parquet** y que tenga una clasificación de tipo **Nombre de persona**.

## <a name="start-the-search"></a>Inicio de la búsqueda

Al realizar la búsqueda, los términos de búsqueda especificados en la opción **Search catalog** (Buscar en el catálogo) se comparan con las características del recurso. Estas características incluyen el nombre del recurso, el tipo de recurso, las clasificaciones y los contactos. Los recursos con características coincidentes aparecen en la lista de resultados de la búsqueda a menos que se excluyan mediante un filtro rápido.

Una vez que haya escrito los términos de búsqueda y establecido los filtros rápidos, comience la búsqueda de una de estas maneras:

- Para buscar en función de los términos especificados, seleccione el icono de búsqueda (:::image type="icon" source="./media/how-to-search-catalog/search-icon.png":::) y presione **Entrar** o **Ver resultados de la búsqueda**.
- Para realizar búsquedas con los términos de una búsqueda anterior, selecciónelos en **Búsquedas recientes**.
- Para realizar una búsqueda con términos sugeridos, selecciónelos en **Sugerencias de búsqueda**.

Seleccione un recurso de la opción **Asset suggestions** (Sugerencias de recursos) para ir directamente a la página de detalles del recurso. Tenga en cuenta que no se realiza ninguna búsqueda.

La lista de resultados de las sugerencias y las búsquedas de usuario puede diferir ligeramente. Los resultados de la lista de sugerencias se basan en coincidencias aproximadas, mientras que los resultados de las búsquedas iniciadas por el usuario se basan en coincidencias exactas.

Al realizar la búsqueda, aparece la página **Resultados de la búsqueda** y se muestran los recursos encontrados en la búsqueda.

:::image type="content" source="./media/how-to-search-catalog/search-results.png" alt-text="Captura de pantalla que muestra los resultados de la búsqueda de un valor de búsqueda de contoso.":::

Para ver los detalles del recurso, seleccione un nombre de recurso.

Use los controles de la parte inferior de la página de resultados de la búsqueda para navegar a otras páginas de resultados de la búsqueda.

:::image type="content" source="./media/how-to-search-catalog/page-navigation.png" alt-text="Captura de pantalla que muestra cómo navegar a través de las páginas de resultados de la búsqueda.":::

### <a name="sort-search-results"></a>Ordenación de los resultados de la búsqueda

Use **Ordenar por** para ordenar los resultados de la búsqueda por **Relevancia** o **Nombre**.

:::image type="content" source="./media/how-to-search-catalog/sort-by.png" alt-text="Captura de pantalla que muestra cómo ordenar los resultados de la búsqueda. En este ejemplo, la lista desplegable &quot;Ordenar por&quot; está establecida según la Relevancia.":::

### <a name="search-results-dynamic-filters"></a>Filtros dinámicos de resultados de la búsqueda

En el panel **Filtro** de la página de **Resultados de la búsqueda** tiene filtros que proporcionan la función de filtrado dinámico de los recursos de la lista de resultados de la búsqueda. El filtrado es dinámico, ya que pueden aparecer filtros adicionales según las selecciones de filtro.

Los filtros dinámicos tienen una casilla para cada valor de la lista desplegable. Use estas casillas de verificación para filtrar tantos valores como quiera.

#### <a name="search-results-dynamic-filter-filter-by-asset-type"></a>Filtro dinámico de resultados de la búsqueda: filtrar por tipo de recurso

Si selecciona un tipo de recurso en la lista desplegable **Tipo de recurso**, aparecen filtros dinámicos que proporcionan otras maneras de limitar los resultados de la búsqueda. Tenga en cuenta que los filtros varían según el tipo de recurso seleccionado. Por ejemplo, si selecciona **Azure SQL Database**, aparecerán filtros dinámicos para el servidor, la base de datos y el esquema. Los valores de estos filtros provienen de los activos de los resultados de la búsqueda del tipo seleccionado.

:::image type="content" source="./media/how-to-search-catalog/asset-type-dynamic-filter.png" alt-text="El elemento de filtro Azure SQL Database es el único elemento de &quot;Tipo de recurso&quot; que está seleccionado. También se resalta el resultado de la búsqueda de ese tipo de recurso." border="true":::

#### <a name="search-results-dynamic-filter-filter-by-classification"></a>Filtro dinámico de resultados de la búsqueda: filtrar por clasificación

Cada clasificación de la lista **Clasificación** se aplica al menos a un elemento de la lista de resultados de la búsqueda. Seleccione una o más clasificaciones para limitar los resultados de la búsqueda a los recursos de las clasificaciones seleccionadas.

:::image type="content" source="./media/how-to-search-catalog/classification-dynamic-filter.png" alt-text="Se resalta el filtro de clasificación de &quot;Resultados de la búsqueda&quot;." border="true":::

#### <a name="edit-and-delete-search-results-filters"></a>Edición y eliminación de los filtros de resultados de la búsqueda

Para quitar un filtro, desactive la casilla situada junto al nombre del mismo.

### <a name="recently-accessed-assets"></a>Recursos a los que se ha obtenido acceso recientemente

La sección de **Usados recientemente** del cuadro de búsqueda expandido muestra los recursos a los que se ha obtenido acceso recientemente, si los hubiera.

- Seleccione **Ver todos** en la sección **Usados recientemente** para ver la lista completa de recursos a los que ha obtenido acceso recientemente.

   :::image type="content" source="./media/how-to-search-catalog/get-to-recent-view.png" alt-text="Captura de pantalla que muestra la sección &quot;Usados recientemente&quot; del cuadro de búsqueda expandido.":::

   Aparece una lista de recursos a los que se ha obtenido acceso recientemente.

   :::image type="content" source="./media/how-to-search-catalog/recent.png" alt-text="Captura de pantalla que muestra una lista de recursos a los que se ha obtenido acceso recientemente.":::

- Para filtrar por nombre, escriba una cadena de búsqueda en **Filtrar por nombre**.

- Para quitar los elementos de la lista, selecciónelos mediante sus casillas y, a continuación, seleccione **Quitar**.

   :::image type="content" source="./media/how-to-search-catalog/remove-from-recent-view.png" alt-text="Captura de pantalla que muestra cómo quitar elementos de una lista de recursos a los que se ha obtenido acceso recientemente.":::

- Para borrar la lista completa, seleccione **Borrar**.

   :::image type="content" source="./media/how-to-search-catalog/clear-recent-view-selections.png" alt-text="Captura de pantalla que muestra cómo borrar una lista de recursos a los que se ha obtenido acceso recientemente.":::

### <a name="search-assets"></a>Búsqueda de recursos

Muchas páginas a parte de la página de **Inicio** tienen un cuadro de **búsqueda de recursos**. Por ejemplo, aquí se muestra una página de detalles de recursos, con la opción **Buscar recursos** resaltada.

:::image type="content" source="./media/how-to-search-catalog/search-assets.png" alt-text="Captura de pantalla que muestra una página de detalles de recursos con la opción de búsqueda de recursos resaltada.":::

Seleccione **Buscar recursos** para visualizar un cuadro de búsqueda como el de la opción **Search catalog** (Buscar en el catálogo) de la página **Inicio**, y que cuenta con las mismas funcionalidades.

:::image type="content" source="./media/how-to-search-catalog/search-assets-dialog.png" alt-text="Captura de pantalla que muestra un cuadro de búsqueda de recursos expandido.":::

## <a name="next-steps"></a>Pasos siguientes

- [Cómo crear, importar y exportar términos del glosario](how-to-create-import-export-glossary.md)
- [Cómo administrar plantillas de términos para el glosario empresarial](how-to-manage-term-templates.md)
