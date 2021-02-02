---
title: Aplicación de la extensión de Windows Azure Diagnostics en Cloud Services (soporte extendido)
description: Aplicación de la extensión de Windows Azure Diagnostics en Cloud Services (soporte extendido)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 4511ad979312d58e0a1b9cce9b1280e9ca059007
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744104"
---
# <a name="apply-the-windows-azure-diagnostics-extension-in-cloud-services-extended-support"></a>Aplicación de la extensión de Windows Azure Diagnostics en Cloud Services (soporte extendido) 
Puede supervisar las métricas de rendimiento claves de cualquier servicio en la nube. Cada rol de servicio en la nube recopila datos mínimos: uso de CPU, uso de red y uso de disco. Si el servicio en la nube tiene la extensión Microsoft.Azure.Diagnostics aplicada a un rol, este último puede recopilar puntos de datos adicionales. Para obtener más información, consulte [Introducción a las extensiones](extensions.md).

La extensión de Windows Azure Diagnostics se puede habilitar para Cloud Services (soporte extendido) a través de [PowerShell](deploy-powershell.md) o la [plantilla de ARM](deploy-template.md)

## <a name="apply-windows-azure-diagnostics-extension-using-powershell"></a>Aplicación de la extensión de Windows Azure Diagnostics mediante PowerShell

```powershell
# Create WAD extension object
$wadExtension = New-AzCloudServiceDiagnosticsExtension -Name "WADExtension" -ResourceGroupName "ContosOrg" -CloudServiceName "ContosCS" -StorageAccountName "ContosSA" -StorageAccountKey $storageAccountKey[0].Value -DiagnosticsConfigurationPath $configFile -TypeHandlerVersion "1.5" -AutoUpgradeMinorVersion $true 
$extensionProfile = @{extension = @($rdpExtension, $wadExtension)} 

# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"

# Add WAD extension to existing Cloud Service extension object
$cloudService.ExtensionProfileExtension = $cloudService.ExtensionProfileExtension + $wadExtension

# Update Cloud Service
$cloudService | Update-AzCloudService
```

## <a name="apply-windows-azure-diagnostics-extension-using-arm-template"></a>Aplicación de la extensión de Windows Azure Diagnostics mediante la plantilla de ARM
```json
"extensionProfile": { 
          "extensions": [ 
            { 
              "name": "Microsoft.Insights.VMDiagnosticsSettings_WebRole1", 
              "properties": { 
                "autoUpgradeMinorVersion": true, 
                "publisher": "Microsoft.Azure.Diagnostics", 
                "type": "PaaSDiagnostics", 
                "typeHandlerVersion": "1.5", 
                "settings": "Include PublicConfig XML as a raw string", 
                "protectedSettings": "Include PrivateConfig XML as a raw string”", 
                "rolesAppliedTo": [ 
                  "WebRole1" 
                ] 
              } 
            }
          ]
        },

```

## <a name="next-steps"></a>Pasos siguientes 
- Revise los [requisitos previos de implementación](deploy-prerequisite.md) para Cloud Services (soporte extendido).
- Consulte las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implemente una instancia de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).