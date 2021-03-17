---
title: Creación de un contenedor de servicio en la nube (clásico) con PowerShell | Microsoft Docs
description: En este artículo se explica cómo crear un contenedor de servicios en la nube con PowerShell. El contenedor hospeda roles web y roles de trabajo.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 39fe439e37b1af4e833396ef83205729af8c7ad3
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102610423"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-classic-container"></a>Uso de un comando de Azure PowerShell para crear un contenedor vacío de servicio en la nube (clásico)

> [!IMPORTANT]
> [Azure Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md) es un nuevo modelo de implementación basado en Azure Resource Manager para el producto Azure Cloud Services. Con este cambio, se ha modificado el nombre del modelo de implementación basado en Azure Cloud Services para Azure Service Manager a Cloud Services (clásico), y todas las implementaciones nuevas deben usar [Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md).

En este artículo se explica cómo crear rápidamente un contenedor de Cloud Services mediante cmdlets de Azure PowerShell. Siga estos pasos:

1. Instale el cmdlet de Microsoft Azure PowerShell desde la página de [descargas de Azure PowerShell](https://aka.ms/webpi-azps) .
2. Abra un símbolo del sistema de PowerShell.
3. Use [Add-AzureAccount](/powershell/module/servicemanagement/azure.service/add-azureaccount) para iniciar sesión.

   > [!NOTE]
   > Para más instrucciones acerca de cómo instalar el cmdlet de Azure PowerShell y conectarse a la suscripción de Azure, consulte [Cómo instalar y configurar Azure PowerShell](/powershell/azure/).
   >
   >
4. Use el cmdlet **New-AzureService** para crear un contenedor de servicios en la nube de Azure vacío.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Para invocar el cmdlet, siga este ejemplo:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Para obtener más información sobre cómo crear el servicio en la nube de Azure, ejecute:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Pasos siguientes

* Para administrar la implementación de servicios en la nube, consulte los comandos [Get-AzureService](/powershell/module/servicemanagement/azure.service/Get-AzureService), [Remove-AzureService](/powershell/module/servicemanagement/azure.service/Remove-AzureService) y [Set-AzureService](/powershell/module/servicemanagement/azure.service/set-azureservice). Para más información, también puede consultar [Configuración de servicios en la nube](cloud-services-how-to-configure-portal.md) .
* Para publicar el proyecto de servicio en la nube en Azure, consulte el código de ejemplo de **PublishCloudService.ps1** del [repositorio de servicios en la nube archivado](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).