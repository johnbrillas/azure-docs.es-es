---
title: 'Administración de una aplicación de QnA Maker: QnA Maker'
description: QnA Maker permite que varias personas colaboren en una base de conocimiento. QnA Maker ofrece la capacidad de mejorar la calidad de la base de conocimiento con el aprendizaje activo. Es posible revisar, aceptar o rechazar y agregar sin quitar ni cambiar las preguntas existentes.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 49bfca118e53bbe3e4287b2ce25e5baffa717175
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102217336"
---
# <a name="manage-qna-maker-app"></a>Administración de una aplicación de QnA Maker

QnA Maker permite colaborar con distintos autores y editores de contenido, ya que ofrece una funcionalidad para restringir el acceso de los colaboradores según el rol del colaborador.
Obtenga más información sobre los [conceptos de autenticación de colaboradores en QnA Maker](../Concepts/role-based-access-control.md).

## <a name="add-azure-role-based-access-control-azure-rbac"></a>Adición del control de acceso basado en roles de Azure (Azure RBAC)

QnA Maker permite que varias personas colaboren en todas las bases de conocimiento del mismo recurso de QnA Maker. Esta característica se proporciona con el [control de acceso basado en roles de Azure (Azure RBAC)](../../../role-based-access-control/role-assignments-portal.md).

## <a name="access-at-the-qna-maker-resource-level"></a>Acceso en el nivel de recursos de QnA Maker

No puede compartir una base de conocimiento particular en un servicio QnA Maker. Si desea un control de acceso más pormenorizado, considere la distribución de las bases de conocimiento entre diferentes recursos de QnA Maker y, a continuación, agregue los roles a cada recurso.

## <a name="add-a-role-to-a-resource"></a>Adición de un rol a un recurso

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Agregar una cuenta de usuario al recurso de QnA Maker

En los pasos siguientes se usa el rol de colaborador, pero cualquiera de los roles se puede agregar mediante estos pasos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya al recurso de QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. Vaya a la pestaña **Access Control (IAM)** .

    ![QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. Seleccione **Agregar**.

    ![Agregar QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Seleccione un rol en la lista siguiente:

    |Role|
    |--|
    |Propietario|
    |Colaborador|
    |Lector de QnA Maker de Cognitive Services|
    |Editor de QnA Maker de Cognitive Services|
    |Usuario de Cognitive Services|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="Adición de rol de QnA Maker (IAM).":::

1. Escriba la dirección de correo electrónico del usuario y presione **Guardar**.

    ![Agregar correo electrónico de QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Visualización de knowledge bases de QnA Maker

Cuando la persona con quien comparte el servicio QnA Maker inicia sesión en el [portal de QnA Maker](https://qnamaker.ai), puede ver todas las bases de conocimiento de ese servicio según el rol.

Cuando esa persona selecciona una knowledge base, su rol actual en ese recurso de QnA Maker es visible junto al nombre de la knowledge base.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Captura de pantalla de knowledge base en modo de edición con el nombre de rol entre paréntesis junto al nombre de la knowledge base en la esquina superior izquierda de la página web.":::

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una base de conocimientos](./manage-knowledge-bases.md)
