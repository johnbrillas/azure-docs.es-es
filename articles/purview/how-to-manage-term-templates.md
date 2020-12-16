---
title: Cómo administrar plantillas de términos para el glosario empresarial
description: Aprenda a administrar plantillas de términos para el glosario empresarial en un catálogo de datos de Azure Purview.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/04/2020
ms.openlocfilehash: b1b811d0817d5e23adc208da14719d64d53830dd
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96551336"
---
# <a name="how-to-manage-term-templates-for-business-glossary"></a>Cómo administrar plantillas de términos para el glosario empresarial

Azure Purview permite crear un glosario de términos que son importantes para enriquecer los datos. Cada nuevo término agregado a su glosario del catálogo de datos de Purview se basa en una plantilla de términos que determina los campos del término. En este artículo se describe cómo crear una plantilla de términos y atributos personalizados que se pueden asociar a los términos del glosario.

## <a name="manage-term-templates-and-custom-attributes"></a>Administración de plantillas de términos y atributos personalizados

Mediante el uso de plantillas de términos, puede crear atributos personalizados, agruparlos y aplicar una plantilla durante la creación de términos. Una vez creado un término, no se puede cambiar la plantilla asociada a él.

1. En la página **Términos del glosario**, seleccione **Manage term templates** (Administrar plantillas de términos).

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/manage-term-templates.png" alt-text="Captura de pantalla del botón Manage term templates (Administrar plantillas de términos) de Términos del glosario.":::

2. En la página se muestran tanto los atributos del sistema como los personalizados. Seleccione la pestaña **Custom** (Personalizado) para crear o editar plantillas de términos.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/manage-term-custom.png" alt-text="Captura de pantalla de la página Manage term templates (Administrar plantillas de términos) de Términos del glosario.":::

3. Seleccione **+ New term template** (Nueva plantilla de términos) y escriba un nombre y una descripción para la plantilla.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/new-term-template.png" alt-text="Captura de pantalla de la Términos del glosario > Manage term templates > New term template (Administrar plantillas de términos > Nueva plantilla de términos).":::

4. Seleccione **+ New attribute** (Nuevo atributo) para crear un nuevo atributo personalizado para la plantilla de términos. Escriba un nombre y una descripción para el atributo. El nombre del atributo personalizado debe ser único dentro de una plantilla de términos, pero se puede usar el mismo en varias plantillas.

   Seleccione el tipo de campo de la lista de opciones **Text** (Texto), **Single choice** (Opción única), **Multi choice** (Varias opciones) o **Date** (Fecha). También puede proporcionar una cadena de valor predeterminada para los tipos de campo de texto.  El atributo también se puede marcar como **obligatorio**.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/new-attribute.png" alt-text="Captura de pantalla de la página Términos del glosario > New attribute (Nuevo atributo).":::

5. Una vez que se hayan creado todos los atributos personalizados, seleccione **Preview** (Vista previa) para organizar la secuencia de atributos personalizados. Puede arrastrar y colocar atributos personalizados en la secuencia que desee.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/preview-term-template.png" alt-text="Captura de pantalla de la vista previa de la plantilla de términos en Términos del glosario.":::

6. Una vez definidos todos los atributos personalizados, seleccione **Create** (Crear) para crear una plantilla de términos con atributos personalizados.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/create-term-template.png" alt-text="Captura de pantalla del botón Create (Crear), en Términos del glosario > New term template (Nueva plantilla de términos).":::

7. Para definir los atributos personalizados existentes como expirados, seleccione **Mark as Expired** (Marcar como expirado). Una vez que ha expirado, el atributo no se puede reactivar. El atributo expirado se mostrará atenuado para los términos existentes. Los futuros términos creados con esta plantilla de términos ya no mostrarán el atributo que se ha marcado como expirado.

   :::image type="content" source="./media/how-to-manage-glossary-term-templates/expired-attribute.png" alt-text="Captura de pantalla que muestra cómo editar el atributo para marcarlo como expirado en Términos del glosario.":::

## <a name="next-steps"></a>Pasos siguientes

Lea el [Tutorial: Creación e importación de términos del glosario](tutorial-import-create-glossary-terms.md) para más información.
