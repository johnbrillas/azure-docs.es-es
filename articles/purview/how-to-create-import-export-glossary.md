---
title: Creación, importación y exportación de términos del glosario
description: Obtenga información acerca de cómo crear, importar y exportar términos del glosario en Azure Purview.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 12/02/2020
ms.openlocfilehash: a693761bcecab87e343014127ad37077c2569e21
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550626"
---
# <a name="how-to-create-import-and-export-glossary-terms"></a>Creación, importación y exportación de términos del glosario

En este artículo se describe cómo crear un término del glosario empresarial en el catálogo de datos de Azure Purview, y cómo importar y exportar términos de glosario mediante archivos .csv.

## <a name="create-a-new-term"></a>Creación de un nuevo término

Para crear un nuevo término de glosario, realice los pasos siguientes:

1. Seleccione el icono de glosario en el panel de navegación izquierdo de la página principal para ir a la página de lista de términos.

2. En la página **Términos del glosario**, seleccione **+ Nuevo término**. Se abrirá una página con la plantilla **Valores predeterminados del sistema** seleccionada. Elija la plantilla con la que desee crear el término de glosario y seleccione **Continuar**.

   :::image type="content" source="media/how-to-create-import-export-glossary/new-term-with-default-template.png" alt-text="Captura de pantalla de la creación del nuevo término." border="true":::

3. Asigne un nombre al nuevo término, que debe ser único en el catálogo. El nombre del término distingue mayúsculas de minúsculas, por lo que puede tener un término denominado **Ejemplo** y otro llamado **ejemplo** en el catálogo.

4. Agregue una **definición**.

5. Establezca el **estado** del término. Los nuevos términos tienen de forma predeterminada el estado **Borrador**.

   :::image type="content" source="media/how-to-create-import-export-glossary/new-term-options.png" alt-text="Captura de pantalla de las opciones de estado" border="true":::.

   Estos marcadores de estado son metadatos asociados al término. Actualmente, puede establecer los siguientes estados en cada término:

   - **Borrador**: este término aún no se ha implementado oficialmente.
   - **Aprobado**: este término es oficial, estándar o está aprobado.
   - **Expirada**: este término ya no debe usarse.
   - **Alerta**: el término requiere atención.

6. Agregue **recursos** y un **acrónimo**. Si el término forma parte de una jerarquía, puede agregar términos primarios en **Primario** en la pestaña de información general.

7. Agregue **sinónimos** y **términos relacionados** en la pestaña Related.

   :::image type="content" source="media/how-to-create-import-export-glossary/related-tab.png" alt-text="Captura de pantalla de Nuevo término > pestaña Related (Relacionados)." border="true":::

8. Opcionalmente, seleccione la pestaña **Contactos** para agregar expertos y administradores a su término.

9. Seleccione **Crear** para crear el término.

## <a name="import-terms-into-the-glossary"></a>Importación de términos al glosario

El catálogo de datos de Azure Purview proporciona un archivo .csv de plantilla para que pueda importar sus términos en el glosario.

Puede importar términos en el catálogo. Los términos duplicados en el archivo se sobrescribirán.

Tenga en cuenta que los nombres de los términos distinguen mayúsculas de minúsculas. Por ejemplo, `Sample` y `saMple` pueden existir en el mismo glosario.

### <a name="to-import-terms-follow-these-steps"></a>Pasos para la importación de términos

1. En la página **Términos del glosario**, seleccione **Import terms** (Importar términos).

2. Se abrirá la página de la plantilla de términos. Seleccione la plantilla de términos que se corresponda con el tipo de archivo .csv que desee importar.

   :::image type="content" source="media/how-to-create-import-export-glossary/select-term-template-for-import.png" alt-text="Captura de pantalla de la página Términos del glosario, botón Import terms (Importar términos).":::

3. Descargue la plantilla .csv y úsela para escribir los términos que desee agregar.

   > [!Important]
   > El sistema solo admite la importación de columnas que estén disponibles en la plantilla. La plantilla "Valores predeterminados del sistema" tendrá todos los atributos predeterminados.
   > Sin embargo, las plantillas de términos personalizadas tendrán atributos predefinidos y atributos personalizados adicionales definidos en la plantilla. Por lo tanto, el archivo .csv tendrá un número total de columnas y nombres de columna diferentes en función de la plantilla de términos seleccionada. También puede revisar el archivo para ver si hay problemas después de la carga.

   :::image type="content" source="media/how-to-create-import-export-glossary/select-file-for-import.png" alt-text="Captura de pantalla de la página Términos del glosario; seleccione el archivo que importar.":::

4. Una vez que haya terminado de rellenar el archivo. csv, seleccione el archivo que desea importar y, a continuación, seleccione **Aceptar**.

5. El sistema cargará el archivo y agregará todos los términos al catálogo.

## <a name="export-terms-from-glossary-with-custom-attributes"></a>Exportación de términos del glosario con atributos personalizados

Debe poder exportar términos del glosario siempre que los términos seleccionados pertenezcan a la misma plantilla de términos.

1. Cuando se encuentra en el glosario, el botón **Exportar** está deshabilitado de forma predeterminada. Una vez que seleccione los términos que desee exportar, se habilita el botón **Exportar** si los términos seleccionados pertenecen a la misma plantilla.

2. Seleccione **Exportar** para descargar los términos seleccionados.

## <a name="next-steps"></a>Pasos siguientes

Lea el [Tutorial: Creación e importación de términos del glosario](tutorial-import-create-glossary-terms.md) para más información.
