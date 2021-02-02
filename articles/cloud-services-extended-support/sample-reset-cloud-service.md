---
title: 'Ejemplos de Azure PowerShell: restablecimiento de las Azure Cloud Services (soporte extendido)'
description: Scripts de ejemplo para restablecer una implementación de un servicio en la nube de Azure (soporte extendido)
ms.topic: sample
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 40b44fd277eac14a5bf2c15f58fccfd9d5b156c4
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881491"
---
# <a name="reset-an-azure-cloud-service-extended-support"></a>Restablecimiento de un servicio en la nube de Azure (soporte extendido) 
Los siguientes ejemplos abordan diversas formas de restablecer una implementación existente de un servicio en la nube de Azure (soporte extendido).

## <a name="reimage-role-instances-of-cloud-service"></a>Restablecimiento de la imagen inicial de las instancias de un servicio en la nube
```powershell
$roleInstances = @("ContosoFrontEnd_IN_0", "ContosoBackEnd_IN_1")
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance $roleInstances -Reimage
```
Este comando restablece la imagen inicial de dos instancias de rol **ContosoFrontEnd\_IN\_0** y **ContosoBackEnd\_IN\_1** del servicio en la nube denominado ContosoCS que pertenece al grupo de recursos ContosOrg.

## <a name="reimage-all-roles-of-cloud-service"></a>Restablecimiento de la imagen inicial de todos los roles de un servicio en la nube
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Reimage
```

## <a name="reimage-a-single-role-instance-of-a-cloud-service"></a>Restablecimiento de la imagen inicial de una única instancia de rol de un servicio en la nube
```powershell
Reset-AzCloudServiceRoleInstance -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "ContosoFrontEnd_IN_0" -Reimage
```

## <a name="restart-a-single-role-instance-of-a-cloud-service"></a>Reinicio de una única instancia de rol de un servicio en la nube
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Restart
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre Azure Cloud Services (soporte extendido), consulte [Introducción a Azure Cloud Services (soporte extendido)](overview.md).
- Visite el [repositorio de ejemplos de Cloud Services (soporte extendido)](https://github.com/Azure-Samples/cloud-services-extended-support)