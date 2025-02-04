---
title: Personalización de un modelo de marcas con el sitio web de Video Indexer
titleSuffix: Azure Media Services
description: Obtenga información sobre cómo personalizar un modelo de marcas con el sitio web de Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/15/2019
ms.author: kumud
ms.openlocfilehash: a2de9dbb479f43d6b646cd9f6cf604d6a08c8b6a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97586120"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Personalización de un modelo de marcas con el sitio web de Video Indexer

Video Indexer permite la detección de marcas por voz y texto visual durante la indexación y la reindexación de contenido de audio y vídeo. La característica de detección de marcas identifica menciones de productos, servicios y compañías sugeridas por la base de datos de marcas de Bing. Por ejemplo, si Microsoft se menciona en un contenido de audio o vídeo, o si se muestra en texto visual en un vídeo, Video Indexer lo detectará como una marca en el contenido.

Un modelo de marcas personalizado permite hacer lo siguiente:

- Seleccionar si quiere que Video Indexer detecte marcas en la base de datos de marcas de Bing
- Seleccionar si quiere que Video Indexer excluya ciertas marcas para que no se detecten (básicamente, crear una lista de denegación de marcas)
- Seleccionar si quiere que Video Indexer incluya marcas que formen parte del modelo y que podrían no estar en la base de datos de marcas de Bing (básicamente, crear una lista de aceptación de marcas).

Para obtener información general detallada, vea esta [Introducción](customize-brands-model-overview.md).

Puede utilizar el sitio web de Video Indexer para crear, usar y editar modelos de marcas personalizados detectados en un vídeo, como se describe en este tema. También puede usar la API, como se describe en [Personalización del modelo de marcas mediante las API](customize-brands-model-with-api.md).

> [!NOTE]
> Si el vídeo se ha indexado antes de agregar una marca, debe reindexarlo. Encontrará el elemento **Volver a indexar** en el menú desplegable asociado al vídeo. Seleccione **Opciones avanzadas** -> **Categorías de marca** y marque **Todas las marcas**.

## <a name="edit-brands-model-settings"></a>Edición de la configuración del modelo de marcas

Tiene la opción de establecer si quiere que las marcas de la base de datos de marcas de Bing sean o no detectadas. Para establecer esta opción, debe editar la configuración del modelo de marcas. Siga estos pasos:

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
1. Para personalizar un modelo en su cuenta, seleccione el botón **Content model customization** (Personalización del modelo de contenido) a la izquierda de la página.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/content-model-customization/content-model-customization.png" alt-text="Personalización de un modelo de contenido en Video Indexer":::
1. Para editar las marcas, seleccione la pestaña **Brands** (Marcas).

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/customize-brand-model/customize-brand-model.png" alt-text="Captura de pantalla que muestra la pestaña de marcas del cuadro de diálogo de personalización del modelo de contenido":::
1. Marque la opción **Show brands suggested by Bing** (Mostrar marcas sugeridas por Bing) si quiere que Video Indexer incluya las marcas sugeridas por Bing. En caso contrario, déjela sin marcar.

## <a name="include-brands-in-the-model"></a>Inclusión de marcas en el modelo

La sección **Include brands** (Incluir marcas) representa las marcas personalizadas que quiere que Video Indexer detecte aunque no las haya sugerido Bing.  

### <a name="add-a-brand-to-include-list"></a>Adición de una marca a la lista de inclusión

1. Seleccione **+ Create new brand** (Crear nueva marca).

    Proporcione un nombre (obligatorio), la categoría (opcional), la descripción (opcional) y la dirección URL de referencia (opcional).
    El campo de categoría tiene como fin ayudarle a etiquetar sus marcas. Este campo se muestra como las *etiquetas* de la marca al usar las API de Video Indexer. Por ejemplo, la marca "Azure" se puede etiquetar o clasificar como "Nube".

    El campo de dirección URL de referencia puede ser cualquier sitio web de referencia para la marca, como un vínculo a su página de Wikipedia.

2. Seleccione **Save** (Guardar) y verá que la marca se ha agregado a la lista **Include brands** (Incluir marcas).

### <a name="edit-a-brand-on-the-include-list"></a>Edición de una marca en la lista de inclusión

1. Seleccione el icono de lápiz junto a la marca que quiera editar.

    Puede actualizar la categoría, la descripción o la dirección URL de referencia de una marca. No puede cambiar el nombre de una marca porque los nombres de marcas son únicos. Si necesita cambiar el nombre de la marca, elimine la marca entera (consulte la sección siguiente) y cree una marca con el nuevo nombre.

2. Seleccione el botón **Update** (Actualizar) para actualizar la marca con la nueva información.

### <a name="delete-a-brand-on-the-include-list"></a>Eliminación de una marca en la lista de inclusión

1. Seleccione el icono de papelera junto a la marca que quiera eliminar.
2. Seleccione **Delete** (Eliminar) y la marca ya no aparecerá en la lista *Include brands* (Incluir marcas).

## <a name="exclude-brands-from-the-model"></a>Exclusión de marcas del modelo

La sección **Exclude brands** (Excluir marcas) representa las marcas que no quiera que Video Indexer detecte.

### <a name="add-a-brand-to-exclude-list"></a>Adición de una marca a la lista de exclusión

1. Seleccione **+ Create new brand** (Crear nueva marca).

    Proporcione un nombre (obligatorio) y una categoría (opcional).

2. Seleccione **Save** (Guadrar) y verá que la marca se ha agregado a la lista *Exclude brands* (Excluir marcas).

### <a name="edit-a-brand-on-the-exclude-list"></a>Edición de una marca en la lista de exclusión

1. Seleccione el icono de lápiz junto a la marca que quiera editar.

    Solo se puede actualizar la categoría de una marca. No puede cambiar el nombre de una marca porque los nombres de marcas son únicos. Si necesita cambiar el nombre de la marca, elimine la marca entera (consulte la sección siguiente) y cree una marca con el nuevo nombre.

2. Seleccione el botón **Update** (Actualizar) para actualizar la marca con la nueva información.

### <a name="delete-a-brand-on-the-exclude-list"></a>Eliminación de una marca en la lista de exclusión

1. Seleccione el icono de papelera junto a la marca que quiera eliminar.
2. Seleccione **Delete** (Eliminar) y la marca ya no aparecerá en la lista *Exclude brands* (Excluir marcas).

## <a name="next-steps"></a>Pasos siguientes

[Personalización del modelo de marcas mediante las API](customize-brands-model-with-api.md)
