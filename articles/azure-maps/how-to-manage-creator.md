---
title: Administración de Creator de Microsoft Azure Maps (versión preliminar)
description: En este artículo, aprenderá a administrar Creator de Microsoft Azure Maps (versión preliminar).
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 1ed3a9033f9be39774e1c52982f63259cc477d29
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96906104"
---
# <a name="manage-azure-maps-creator-preview"></a>Administración de Creator de Azure Maps (versión preliminar) 

> [!IMPORTANT]
> Los servicios de Creator de Azure Maps se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Maps Creator le permite crear datos de planos interiores privados. Mediante el uso de la API de Azure Maps y el módulo Indoor Maps, puede desarrollar aplicaciones web interactivas y dinámicas de planos interiores. Actualmente, Creator solo está disponible en Estados Unidos con el plan de tarifa S1.

En este artículo le guiaremos por los pasos necesarios para crear y eliminar un recurso de Creator en una cuenta de Azure Maps.

## <a name="create-creator-preview-resource"></a>Creación de un recurso de Creator (versión preliminar)

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

2. Seleccione la cuenta de Azure Maps. Si no puede ver su cuenta de Azure Maps en los **recursos recientes**, vaya al menú de Azure Portal. Seleccione **Todos los recursos**. Busque y seleccione la cuenta de Azure Maps.

    ![Página principal del portal de Azure Maps](./media/how-to-manage-creator/select-maps-account.png)

3. Una vez en la página de la cuenta de Azure Maps, vaya a la opción **Información general** en **Creator**. Haga clic en **Crear** para crear un recurso de Azure Maps Creator.

    ![Creación de una página de Azure Maps Creator](./media/how-to-manage-creator/creator-blade-settings.png)

4. Escriba el nombre y la ubicación del recurso de Creator. Actualmente, Creator solo se admite el Estados Unidos. Haga clic en **Revisar + crear**.

   ![Escriba la página de información de cuenta de Creator](./media/how-to-manage-creator/creator-creation-dialog.png)

5. Revise la configuración y haga clic en **Crear**.

    ![Confirmación de la página de configuración de la cuenta de Creator](./media/how-to-manage-creator/creator-create-dialog.png)

6. Una vez finalizada la implementación, verá una página con un mensaje de error o de operación correcta.

   ![Página de estado de la implementación de recursos](./media/how-to-manage-creator/creator-resource-created.png)

7. Haga clic en **Ir al recurso**. En la página con la vista de recursos de Creator se muestra el estado del recurso de Creator y la región demográfica elegida.

    ![Página de estado de Creator](./media/how-to-manage-creator/creator-resource-view.png)

   >[!NOTE]
   >En la página de recursos de Creator, puede volver a la cuenta de Azure Maps a la que pertenece haciendo clic en la cuenta de Azure Maps.

## <a name="delete-creator-preview-resource"></a>Eliminación de un recurso de Creator (versión preliminar)

Para eliminar el recurso de Creator, vaya a la cuenta de Azure Maps. En **Creator**, haga clic en **Información general**. Haga clic en el botón **Eliminar** .

>[!WARNING]
>Al eliminar el recurso de Creator de la cuenta de Azure Maps, también se eliminarán los conjuntos de datos, los conjuntos de mosaicos y los conjuntos de estados de características creados con los servicios de Creator.

![Página de Creator con el botón eliminar](./media/how-to-manage-creator/creator-delete.png)

Haga clic en el botón de **eliminar** y escriba su nombre de Creator para confirmar la eliminación. Una vez eliminado el recurso, verá una página de confirmación, tal como se muestra en la imagen siguiente:

![Página de Creator con la confirmación de la eliminación](./media/how-to-manage-creator/creator-confirmdelete.png)

## <a name="authentication"></a>Authentication

Creator (versión preliminar) hereda la configuración de Access Control (IAM) de Azure Maps. Todas las llamadas de API para el acceso a los datos se deben enviar con reglas de autenticación y autorización.

Los datos de uso de Creator se incorporan en los gráficos de uso de Azure Maps y en el registro de actividad.  Para obtener más información, consulte [Administrar la autenticación en Azure Maps](./how-to-manage-authentication.md).

## <a name="access-to-creator-services"></a>Acceso a los servicios de Creator

Solo se puede acceder a los servicios de Creator desde la ubicación seleccionada durante la creación. Si se realizan llamadas a los servicios de Creator fuera de la ubicación seleccionada, se devolverá un mensaje de error de usuario. Para realizar llamadas fuera de la ubicación seleccionada, la dirección URL del servicio debe incluir el prefijo geográfico de las ubicaciones seleccionadas. Por ejemplo, si se crea Creator en Estados Unidos, todas las llamadas al servicio de conversión se deben enviar a `us.atlas.microsoft.com/conversion/convert`.

Asimismo, todos los datos importados en Creator deben cargarse en la misma ubicación geográfica que el recurso de Creator. Por ejemplo, si se aprovisiona Creator en Estados Unidos, se deben cargar todos los datos sin procesar a través de `us.atlas.microsoft.com/mapData/upload`.

## <a name="next-steps"></a>Pasos siguientes

Introducción a los servicios de Creator (versión preliminar) para mapas de interiores:

> [!div class="nextstepaction"]
> [Data Upload](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Conversión de datos](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Dataset](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Conjunto de mosaicos](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Conjunto de estados de características](creator-indoor-maps.md#feature-statesets)

Aprenda a usar los servicios de Creator (versión preliminar) para representar mapas de interiores en la aplicación:

> [!div class="nextstepaction"]
> [Tutorial de Azure Maps Creator](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Estilo dinámico de planos interiores](indoor-map-dynamic-styling.md)

> [!div class="nextstepaction"]
> [Uso del módulo de planos interiores](how-to-use-indoor-module.md)