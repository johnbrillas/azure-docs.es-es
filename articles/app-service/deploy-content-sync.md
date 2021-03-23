---
title: Sincronización del contenido de una carpeta en la nube
description: Aprenda a implementar una aplicación en Azure App Service mediante una sincronización de contenido desde una carpeta de la nube, lo que incluye OneDrive o Dropbox.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 02/25/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: bfee320c7a8b4cbe8439c376350d1234b393bfb5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051246"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Sincronización de contenido de una carpeta de nube a Azure App Service
En este artículo se explica cómo sincronizar el contenido en [Azure App Service](./overview.md) desde Dropbox y OneDrive. 

Con el enfoque de la sincronización de contenido, se trabaja con el código y el contenido de la aplicación en una carpeta en la nube designada para asegurarse de que se encuentra en un estado listo para implementar y, después, se sincroniza con App Service con un clic de botón. 

Debido a diferencias subyacentes en las API, **OneDrive para la empresa** no se admite en este momento.

> [!NOTE]
> La página **Centro de desarrollo (clásico)** en Azure Portal, que es la experiencia de implementación anterior, quedará en desuso en marzo de 2021. Este cambio no afectará a ninguna configuración de implementación existente en la aplicación y puede continuar con la administración de la implementación de aplicaciones en la página **Centro de implementación**.

## <a name="enable-content-sync-deployment"></a>Habilitación de la implementación de la sincronización de contenido

1. En [Azure Portal](https://portal.azure.com), vaya a la página de administración de la aplicación App Service.

1. En el menú de la izquierda, haga clic en **Centro de implementación** > **Configuración**. 

1. En **Origen**, seleccione **OneDrive** o **Dropbox**.

1. Haga clic en **Autorizar** y siga las indicaciones de autorización. 

    ![Muestra cómo autorizar OneDrive o Dropbox en el centro de implementación en Azure Portal.](media/app-service-deploy-content-sync/choose-source.png)

    Solo debe autorizarse con OneDrive o Dropbox una vez para la cuenta de Azure. Para autorizar una cuenta diferente de OneDrive o Dropbox para una aplicación, haga clic en **Cambiar cuenta**.

1. En **Carpeta**, seleccione la carpeta que desea sincronizar. Esta carpeta se crea en la siguiente ruta de acceso de contenido designada en OneDrive o Dropbox. 
   
    * **OneDrive**: `Apps\Azure Web Apps`
    * **Dropbox**: `Apps\Azure`
    
1. Haga clic en **Save**(Guardar).

## <a name="synchronize-content"></a>Sincronización de contenido

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. En [Azure Portal](https://portal.azure.com), vaya a la página de administración de la aplicación App Service.

1. En el menú de la izquierda, haga clic en **Centro de implementación** > **Redeploy/Sync** (Reimplementar/sincronizar). 

    ![Muestra cómo sincronizar su carpeta en la nube con App Service.](media/app-service-deploy-content-sync/synchronize.png)
   
1. Haga clic en **Aceptar** para confirmar la sincronización.

# <a name="azure-cli"></a>[CLI de Azure](#tab/cli)

Para iniciar una sincronización, ejecute el siguiente comando y reemplace \<group-name> y \<app-name>:

```azurecli-interactive
az webapp deployment source sync –-resource-group <group-name> –-name <app-name>
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Para iniciar una sincronización, ejecute el siguiente comando y reemplace \<group-name> y \<app-name>:

```azurepowershell-interactive
Invoke-AzureRmResourceAction -ResourceGroupName <group-name> -ResourceType Microsoft.Web/sites -ResourceName <app-name> -Action sync -ApiVersion 2019-08-01 -Force –Verbose
```

-----

## <a name="disable-content-sync-deployment"></a>Deshabilitación de la implementación de la sincronización de contenido

1. En [Azure Portal](https://portal.azure.com), vaya a la página de administración de la aplicación App Service.

1. En el menú de la izquierda, haga clic en **Centro de implementación** > **Configuración** > **Desconectar**. 

    ![Muestra cómo desconectar la sincronización de carpetas en la nube con la aplicación de App Service en Azure Portal.](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación desde el repositorio GIT local](deploy-local-git.md)