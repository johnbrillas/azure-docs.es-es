---
title: Requisitos previos para la implementación de Azure Cloud Services (soporte extendido)
description: Requisitos previos para la implementación de Azure Cloud Services (soporte extendido)
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 1473305d7da57d1216ef05c0b88a0f69d586784b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101728117"
---
# <a name="prerequisites-for-deploying-azure-cloud-services-extended-support"></a>Requisitos previos para la implementación de Azure Cloud Services (soporte extendido)

> [!IMPORTANT]
> Cloud Services (soporte extendido) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para garantizar una implementación correcta de Cloud Services (soporte extendido), revise los pasos siguientes y complete cada punto antes de intentar realizar la implementación. 

## <a name="register-the-cloudservices-feature"></a>Registro de la característica CloudServices
Registre la característica de la suscripción. El registro puede tardar varios minutos en terminar. 

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```

Compruebe el estado del registro mediante lo siguiente:  
```powershell
Get-AzProviderFeature 

#Sample output
FeatureName               ProviderName      RegistrationState
CloudServices           Microsoft.Compute    Registered
```

## <a name="required-service-configuration-cscfg-file-updates"></a>Actualizaciones necesarias del archivo de configuración de servicio (.cscfg)

### <a name="1-virtual-network"></a>1) Red virtual
Las implementaciones del servicio en la nube (soporte extendido) deben estar en una red virtual. La red virtual se puede crear por medio de [Azure Portal](../virtual-network/quick-create-portal.md), [PowerShell](../virtual-network/quick-create-powershell.md), la [CLI de Azure](../virtual-network/quick-create-cli.md) o una [plantilla de ARM](../virtual-network/quick-create-template.md). Además, debe haber una referencia a la red virtual y las subredes en el archivo de configuración de servicio (.cscfg), en la sección [NetworkConfiguration](schema-cscfg-networkconfiguration.md). 

En el caso de las redes virtuales que pertenecen al mismo grupo de recursos que el servicio en la nube, basta con una referencia al nombre de la red virtual en el archivo de configuración de servicio (.cscfg). Si la red virtual y el servicio en la nube se encuentran en dos grupos de recursos diferentes, es necesario especificar el id. de Azure Resource Manager completo de la red virtual en el archivo de configuración de servicio (.cscfg).
 
#### <a name="virtual-network-located-in-same-resource-group"></a>Red virtual ubicada en el mismo grupo de recursos
```xml
<VirtualNetworkSite name="<vnet-name>"/> 
  <AddressAssignments> 
    <InstanceAddress roleName="<role-name>"> 
     <Subnets> 
       <Subnet name="<subnet-name>"/> 
     </Subnets> 
    </InstanceAddress> 
```

#### <a name="virtual-network-located-in-different-resource-group"></a>Red virtual ubicada en otro grupo de recursos
```xml
<VirtualNetworkSite name="/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>"/> 
   <AddressAssignments> 
     <InstanceAddress roleName="<role-name>"> 
       <Subnets> 
        <Subnet name="<subnet-name>"/> 
       </Subnets> 
     </InstanceAddress> 
```
### <a name="2-remove-the-old-plugins"></a>2) Eliminación de complementos antiguos

Quite la configuración antigua de Escritorio remoto del archivo de configuración de servicio (.cscfg).  

```xml
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="gachandw" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="XXXX" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="2021-12-17T23:59:59.0000000+05:30" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" /> 
```

## <a name="required-service-definition-file-csdef-updates"></a>Actualizaciones necesarias del archivo de definición de servicio (.csdef)

### <a name="1-virtual-machine-sizes"></a>1) Tamaños de máquina virtual
Los tamaños siguientes están en desuso en Azure Resource Manager. Pero si quiere seguir usándolos, actualice el nombre `vmsize` con la convención de nomenclatura de Azure Resource Manager asociada.  

| Nombre de tamaño anterior | Nombre de tamaño actualizado | 
|---|---|
| ExtraSmall | Standard_A0 | 
| Pequeño | Standard_A1 |
| Media | Standard_A2 | 
| Grande | Standard_A3 | 
| ExtraLarge | Standard_A4 | 
| A5 | Standard_A5 | 
| A6 | Standard_A6 | 
| A7 | Standard_A7 |  
| A8 | Standard_A8 | 
| A9 | Standard_A9 |
| A10 | Standard_A10 | 
| A11 | Standard_A11 | 
| MSODSG5 | Standard_MSODSG5 | 

 Por ejemplo, `<WorkerRole name="WorkerRole1" vmsize="Medium"` se convertiría en `<WorkerRole name="WorkerRole1" vmsize="Standard_A2"`.
 
> [!NOTE]
> Para recuperar una lista de los tamaños disponibles, vea la [lista de SKU de recursos](/rest/api/compute/resourceskus/list) y aplique los siguientes filtros: <br>
`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


### <a name="2-remove-old-remote-desktop-plugins"></a>2) Eliminación de los complementos antiguos de Escritorio remoto
Las implementaciones que usan los complementos antiguos de Escritorio remoto deben eliminar los módulos del archivo de definición de servicio (.csdef) y cualquier certificado asociado. 

```xml
<Imports> 
<Import moduleName="RemoteAccess" /> 
<Import moduleName="RemoteForwarder" /> 
</Imports> 
```

## <a name="key-vault-creation"></a>Creación de Key Vault 

Key Vault se usa para almacenar certificados asociados a Cloud Services (soporte extendido). Agregue los certificados a Key Vault y haga referencia a las huellas digitales del certificado en el archivo de configuración de servicio. También debe habilitar Key Vault para los permisos adecuados, de modo que el recurso de Cloud Services (soporte extendido) pueda recuperar el certificado almacenado como secretos de Key Vault. Key Vault se puede crear por medio de [Azure Portal](../key-vault/general/quick-create-portal.md) y [PowerShell](../key-vault/general/quick-create-powershell.md). Key Vault se debe crear en la misma región y suscripción que el servicio en la nube. Para obtener más información, vea [Uso de certificados con Azure Cloud Services (soporte extendido)](certificates-and-key-vault.md).

## <a name="next-steps"></a>Pasos siguientes 
- Revise los [requisitos previos de implementación](deploy-prerequisite.md) de Cloud Services (soporte extendido).
- Implemente un servicio de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
- Vea las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Visite el [repositorio de ejemplos de Cloud Services (soporte extendido)](https://github.com/Azure-Samples/cloud-services-extended-support)