---
title: Restauración de aplicaciones eliminadas
description: Aprenda a restaurar una aplicación eliminada en Azure App Service. Evite los problemas que supone eliminar una aplicación accidentalmente.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: 71f762ac0effc9ad14510c02679109362163f66d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97008544"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Restauración de aplicaciones de App Service eliminadas con PowerShell

Si por accidente ha eliminado su aplicación de Azure App Service, puede restaurarla mediante los comandos del [módulo de PowerShell Az](/powershell/azure/).

> [!NOTE]
> - Las aplicaciones eliminadas se purgan del sistema 30 días después de la eliminación inicial. Después de purgar una aplicación, no se puede recuperar.
> - La funcionalidad de deshacer eliminación no es compatible con el plan de consumo.
> - Las aplicaciones de App Services que se ejecutan en una instancia de App Service Environment no admiten instantáneas. Por lo tanto, la funcionalidad de deshacer eliminación y la funcionalidad de clonación no se admiten para las aplicaciones de App Service que se ejecutan en una instancia de App Service Environment.
>

## <a name="re-register-app-service-resource-provider"></a>Volver a registrar el proveedor de recursos de App Service

Algunos clientes pueden encontrarse con una incidencia por la que no se recupera la lista de aplicaciones eliminadas. Para resolver el problema, ejecute el siguiente comando:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Mostrar una lista de aplicaciones eliminadas

Para obtener la colección de aplicaciones eliminadas, puede usar `Get-AzDeletedWebApp`.

Para más información sobre una aplicación eliminada específica, puede usar:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

La información detallada incluye:

- **DeletedSiteId**: identificador único de la aplicación, que se usa con escenarios en los que se han eliminado varias aplicaciones con el mismo nombre.
- **SubscriptionID**: suscripción que contiene el recurso eliminado.
- **Ubicación**: ubicación de la aplicación original.
- **ResourceGroupName**: nombre del grupo de recursos original.
- **Name**: nombre de la aplicación original.
- **Ranura**: el nombre de la ranura.
- **Hora de eliminación**: cuándo se eliminó la aplicación.  

## <a name="restore-deleted-app"></a>Restauración de la aplicación eliminada

>[!NOTE]
> `Restore-AzDeletedWebApp` no se admite para aplicaciones de funciones.

Una vez identificada la aplicación que quiere restaurar, puede hacerlo mediante `Restore-AzDeletedWebApp`.

```powershell
Restore-AzDeletedWebApp -TargetResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Las ranuras de implementación no se restauran como parte de la aplicación. Si necesita restaurar un espacio de ensayo use la marca `-Slot <slot-name>`.
>

Las entradas del comando son:

- **Grupo de recursos de destino**: grupo de recursos de destino donde se restaurará la aplicación.
- **Name**: nombre de la aplicación, debe ser globalmente único.
- **TargetAppServicePlanName**: plan de App Service vinculado a la aplicación.

De forma predeterminada `Restore-AzDeletedWebApp` restaurará también la configuración de la aplicación y todo el contenido. Si solo quiere restaurar el contenido, use la marca `-RestoreContentOnly` con este cmdlet.

> [!NOTE]
> Si la aplicación se hospedaba en una instancia de App Service Environment y después se eliminó de dicha instancia, solo se puede restaurar si la instancia de App Service Environment correspondiente sigue existiendo.
>

Aquí puede encontrar la referencia de commandlet completa: [Restore-AzDeletedWebApp](/powershell/module/az.websites/restore-azdeletedwebapp).
