---
title: Inicio de sesión en el portal de LUIS
description: Si es un usuario que inicia sesión por primera vez en el portal de LUIS, la experiencia de inicio de sesión será ligeramente diferente en función de su cuenta de usuario actual.
services: cognitive-services
ms.custom: ''
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 09/08/2020
ms.topic: how-to
ms.author: nitinme
author: nitinme
ms.openlocfilehash: 3235f6285edb99776b42014678cd2b6c60d17f62
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763860"
---
# <a name="sign-in-to-luis-portal"></a>Inicio de sesión en el portal de LUIS

Use este artículo para comenzar con el portal de LUIS y crear un recurso de creación. Después de completar los pasos de este artículo, podrá crear y publicar aplicaciones de LUIS.

## <a name="access-the-portal"></a>Acceso al portal


1. Para comenzar con LUIS, vaya al [portal de LUIS](https://www.luis.ai). Si aún no tiene una suscripción, se le pedirá que cree una [cuenta gratuita](https://azure.microsoft.com//free/cognitive-services/) y vuelva al portal.
2. Actualice la página para actualizarla con la suscripción recién creada.
3. Seleccione la suscripción en la lista desplegable.

    > [!div class="mx-imgBorder"]
    > ![selección de suscripciones](./media/migrate-authoring-key/select-subscription-sign-in-2.png)

4. Si la suscripción se encuentra en otro inquilino, no podrá cambiar los inquilinos de la ventana existente. Para cambiar de inquilinos, cierre esta ventana y seleccione el avatar situado en el extremo derecho que contiene sus iniciales en la barra superior. Haga clic en **Elegir un recurso de creación diferente** en la parte superior para volver a abrir la ventana.

    > [!div class="mx-imgBorder"]
    > ![cambio de directorios](./media/migrate-authoring-key/switch-directories.png)

5. Si ya tiene un recurso de creación de LUIS asociado a la suscripción, elíjalo en la lista desplegable. Puede ver todas las aplicaciones que se crean en este recurso de creación.
6. Si no es así, haga clic en **Crear un nuevo recurso de creación** en la parte inferior de este modal.
7.  Al crear un nuevo recurso de creación, proporcione la información siguiente:

    > [!div class="mx-imgBorder"]
    > ![Crear recurso](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

    * **Nombre de inquilino**: el inquilino al que está asociada la suscripción de Azure. No podrá cambiar los inquilinos de la ventana existente. Para cambiar de inquilinos, cierre esta ventana y seleccione el avatar en la esquina superior derecha de la pantalla, que contiene sus iniciales. Seleccione **Elegir un recurso de creación diferente** en la parte superior para volver a abrir la ventana.
    * **Grupo de recursos**: nombre del grupo de recursos personalizado que elige en la suscripción. Los grupos de recursos le permiten agrupar los recursos de Azure para el acceso y la administración. Si actualmente no tiene ningún grupo de recursos en la suscripción, no se le permitirá crear uno en el portal de LUIS. Vaya a [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) para crear uno y vuelva a LUIS para continuar con el proceso de inicio de sesión.
    * **Nombre del recurso**: el nombre personalizado que elija, que se usa como parte de la dirección URL para las transacciones de creación. Este nombre solo puede incluir caracteres alfanuméricos o el carácter `-`, y no puede terminar ni comenzar por `-`. Si se incluye algún otro símbolo en el nombre, se producirá un error al crear el recurso.
    * **Ubicación**: elija la opción de crear las aplicaciones en una de las [tres ubicaciones de creación](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions) que se admiten actualmente en LUIS, incluidas: Oeste de EE. UU., Oeste de Europa y Este de Australia.
    * **Plan de tarifa**: de manera predeterminada, se selecciona el plan de tarifa de creación F0, ya que es el recomendado. Cree una [clave administrada por el cliente](https://docs.microsoft.com/azure/cognitive-services/luis/luis-encryption-of-data-at-rest#customer-managed-keys-for-language-understanding) desde Azure Portal si está buscando una capa adicional de seguridad.
8. Ha cerrado sesión correctamente en LUIS. Ahora puede empezar a crear aplicaciones.

## <a name="troubleshooting"></a>Solución de problemas

* Al crear un nuevo recurso, asegúrese de que su nombre solo incluye caracteres alfanuméricos o el carácter '-', y que no comienza ni termina por '-'. De lo contrario, no se realizará correctamente.
* Asegúrese de que tiene los [permisos adecuados en su suscripción para crear un recurso de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles). Si no dispone de estos permisos, póngase en contacto con el administrador de la suscripción para que se los proporcione.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [iniciar una nueva aplicación](luis-how-to-start-new-app.md).
