---
title: Configuración de un origen de Azure Front Door Estándar/Prémium (versión preliminar)
description: En este artículo, se muestra cómo configurar un origen con Endpoint Manager.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ebc71ea2d354caf0c8f31b1231ecc1487237dd29
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101741895"
---
# <a name="set-up-an-azure-front-door-standardpremium-preview-origin"></a>Configuración de un origen de Azure Front Door Estándar/Prémium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

En este artículo, se muestra cómo crear un origen de Azure Front Door Estándar/Prémium en un grupo de orígenes existente. 

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Antes de poder crear un origen de Azure Front Door Estándar/Prémium, debe haber creado al menos un grupo de orígenes.

## <a name="create-a-new-azure-front-door-standardpremium-origin"></a>Creación de un origen de Azure Front Door Estándar/Prémium

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a su perfil de Azure Front Door Estándar/Prémium.

1. Seleccione **Origin Group** (Grupo de orígenes). Luego, seleccione **+ Agregar** para crear un grupo de orígenes nuevo.
   
    :::image type="content" source="../media/how-to-create-origin/select-add-origin.png" alt-text="Captura de pantalla de la página de aterrizaje del grupo de orígenes.":::

1. En la página **Add an origin group** (Agregar un grupo de orígenes), escriba un **Nombre** único para el grupo de orígenes nuevo.

1. Luego, seleccione **+ Add an Origin** (+ Agregar un origen) para agregar un origen nuevo a este grupo de orígenes. 

    :::image type="content" source="../media/how-to-create-origin/add-origin-view.png" alt-text="Captura de pantalla de la página para agregar un origen.":::
  
    | Configuración | Value |
    | --- | --- |
    | Nombre | Escriba un nombre único para el origen de Azure Front Door nuevo. |   
    | Tipo de origen | El tipo de recurso que desea agregar. Azure Front Door Estándar/Prémium admite la detección automática del origen de la aplicación del servicio de aplicación, el servicio en la nube o el almacenamiento. Si desea usar un recurso diferente en Azure o un back-end que no es de Azure, seleccione **Host personalizado**. |
    | Nombre de host  | Si no seleccionó **Host personalizado** en el tipo de host de origen, para seleccionar el back-end, debe elegir el nombre de host de origen en el menú desplegable. |
    | Encabezado host del origen | Escriba el valor del encabezado host que se envía al back-end para cada solicitud. Para más información, consulte la sección sobre el [encabezado host del origen](concept-origin.md#hostheader). |
    | Puerto HTTP | Escriba el valor del puerto que el origen admite para el protocolo HTTP. |
    | Puerto HTTPS | Escriba el valor del puerto que el origen admite para el protocolo HTTPS. |
    | Priority | Asigne prioridades a un origen distinto cuando quiera usar un origen de servicio principal para todo el tráfico. Además, proporcione las copias de seguridad si el origen principal o el de copia de seguridad no están disponibles. Para más información, consulte [Prioridad](concept-origin.md#priority). |
    | Peso | Asigne pesos a sus distintos orígenes para distribuir el tráfico entre un conjunto de orígenes, ya sea de manera uniforme o según los coeficientes de peso. Para más información, vea [Pesos](concept-origin.md#weighted). |
    | Status | Seleccione esta opción para habilitar el origen. |
    | Regla | Seleccione los conjuntos de reglas que se aplicarán a esta ruta. Para más información sobre cómo configurar las reglas, consulte el artículo sobre [configuración de un conjunto de reglas para Azure Front Door](how-to-configure-rule-set.md). | 

    > [!IMPORTANT]
    > Durante la configuración, las API no validan si el origen no es accesible desde los entornos de Front Door. Asegúrese de que Front Door puede llegar a su origen.

1. Seleccione **Agregar** para crear el origen nuevo. El origen creado debe aparecer en la lista de orígenes con el grupo.
  
    :::image type="content" source="../media/how-to-create-origin/origin-list-view.png" alt-text="Captura de pantalla de la vista de origen en la lista.":::

1. Seleccione **Agregar** para agregar el grupo de orígenes al punto de conexión actual. El grupo de orígenes debe aparecer dentro del panel del grupo de orígenes.

## <a name="clean-up-resources"></a>Limpieza de recursos
Para eliminar un grupo de orígenes cuando ya no lo necesite, haga clic en **…** y, luego, seleccione **Eliminar** en el menú desplegable.

:::image type="content" source="../media/how-to-create-origin/delete-origin-group.png" alt-text="Captura de pantalla de cómo eliminar un grupo de orígenes.":::

Para eliminar un origen cuando ya no lo necesite, haga clic en **…** y, luego, seleccione **Eliminar** en el menú desplegable. 

:::image type="content" source="../media/how-to-create-origin/delete-origin-view.png" alt-text="Captura de pantalla de cómo eliminar un origen.":::

## <a name="next-steps"></a>Pasos siguientes

Para información sobre los dominios personalizados, consulte cómo [agregar un dominio personalizado](how-to-add-custom-domain.md) al punto de conexión de Azure Front Door Estándar/Prémium.
