---
title: 'Detalles de los documentos: Custom Translator'
titleSuffix: Azure Cognitive Services
description: En la página de la lista de documentos se muestran los 10 primeros documentos en el área de trabajo. Para cada uno de los documentos, se muestra el nombre, emparejamiento, tipo, idioma, marca de tiempo de carga y la dirección de correo electrónico del usuario que ha cargado el documento.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 56093204516e156d670097c22b4edab42db54bde
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98895618"
---
# <a name="view-document-details"></a>Ver detalles del documento

En la página de la lista de documentos se muestran los 10 primeros documentos en el área de trabajo. Para cada uno de los documentos, se muestra el nombre, emparejamiento, tipo, idioma, marca de tiempo de carga y la dirección de correo electrónico del usuario que ha cargado el documento.

Haga clic en un documento individual para ver la página de detalles del documento. La página de detalles del documento muestra la lista de las oraciones extraídas del documento.

- De forma predeterminada, la visualización de los idiomas de origen y destino "en paralelo" se selecciona en el campo desplegable, pero puede cambiar la selección para ver las oraciones en el idioma de origen o destino.
- Se muestran 20 oraciones por página de forma predeterminada. Puede usar el control de paginación para desplazarse entre las páginas.

![detalles del documento](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Eliminar un documento

El usuario debe ser propietario del área de trabajo para eliminar un documento. Además, si un modelo está usando el documento, ya sea durante el proceso de entrenamiento o durante el proceso de implementación, dicho documento no podrá eliminarse.

1. Vaya a la página del documento
2. Mantenga el mouse sobre cualquier registro de documento y haga clic en el icono de papelera.

    ![Eliminación de un documento](media/how-to/how-to-delete-document-1.png)

3. Confirme la eliminación.

    ![Confirmación de la eliminación](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [cómo entrenar un modelo](how-to-train-model.md).
