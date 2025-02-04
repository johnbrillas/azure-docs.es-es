---
title: 'Publicar aplicaciones integradas en Windows Virtual Desktop: Azure'
description: Publicación de aplicaciones integradas en Windows Virtual Desktop
author: Heidilohr
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 32441a7da0e079a2de2fbd6906184c518d60e4a5
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102562867"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Publicar aplicaciones integradas en Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop con objetos de Windows Virtual Desktop de Azure Resource Manager. Si usa Windows Virtual Desktop (clásico) sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/publish-apps-2019.md).

En este artículo se explica cómo publicar aplicaciones en el entorno de Windows Virtual Desktop.

## <a name="publish-built-in-apps"></a>Publicación de aplicaciones integradas

Para publicar una aplicación integrada:

1. Conéctese a una de las máquinas virtuales del grupo de hosts.
2. Para obtener el elemento **PackageFamilyName** de la aplicación que quiere publicar, siga las instrucciones de [este artículo](/powershell/module/appx/get-appxpackage).
3. Por último, ejecute el siguiente cmdlet con `<PackageFamilyName>` reemplazado por el elemento **PackageFamilyName** que encontró en el paso anterior:

   ```powershell
   New-AzWvdApplication -Name <applicationname> -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -FilePath "shell:appsFolder\<PackageFamilyName>!App" -CommandLineSetting <Allow|Require|DoNotAllow> -IconIndex 0 -IconPath <iconpath> -ShowInPortal:$true
   ```

>[!NOTE]
> Windows Virtual Desktop solo admite la publicación de aplicaciones con ubicaciones de instalación que comienzan por `C:\Program Files\WindowsApps`.

## <a name="update-app-icons"></a>Actualizar los iconos de aplicación

Después de publicar una aplicación, tendrá el icono de aplicación de Windows predeterminado, en lugar de su imagen de icono normal. Para cambiar el icono a su icono normal, coloque la imagen del icono que quiera en un recurso compartido de red. Los formatos de imagen admitidos son PNG, BMP, GIF, JPG, JPEG e ICO.

## <a name="publish-microsoft-edge"></a>Publicar Microsoft Edge

El proceso que se usa para publicar Microsoft Edge es un poco diferente del proceso de publicación de otras aplicaciones. Para publicar Microsoft Edge con la página principal predeterminada, ejecute este cmdlet:

```powershell
New-AzWvdApplication -Name -ResourceGroupName -ApplicationGroupName -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" -CommandLineSetting <Allow|Require|DoNotAllow> -iconPath "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\microsoftedge.exe" -iconIndex 0 -ShowInPortal:$true
```

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar fuentes para organizar cómo se muestran las aplicaciones para los usuarios en [Personalización de fuente para usuarios de Windows Virtual Desktop](customize-feed-for-virtual-desktop-users.md).
- Aprenda sobre la característica de asociación de aplicaciones de MSIX en [Configuración de la asociación de aplicaciones en formato .MSIX](app-attach.md).

