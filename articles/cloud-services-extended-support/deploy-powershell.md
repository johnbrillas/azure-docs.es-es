---
title: 'Implementación de un servicio en la nube (soporte extendido): PowerShell'
description: Implementación de un servicio en la nube (soporte extendido) mediante PowerShell
ms.topic: tutorial
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 8bfa7c164f5b974a8cf8974b3ff346f3401dd218
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880226"
---
# <a name="deploy-a-cloud-service-extended-support-using-azure-powershell"></a>Implementación de un servicio en la nube (soporte extendido) mediante Azure PowerShell

En este artículo se muestra cómo usar el módulo `Az.CloudService` de PowerShell para implementar Cloud Services (soporte extendido) en Azure, con varios roles (WebRole y WorkerRole) y una extensión de escritorio remoto. 

> [!IMPORTANT]
> Cloud Services (soporte extendido) se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Antes de comenzar

Consulte los [requisitos previos de implementación](deploy-prerequisite.md) de Cloud Services (soporte extendido) y cree los recursos asociados. 

## <a name="deploy-a-cloud-services-extended-support"></a>Implementación de Cloud Services (soporte extendido)
1. Instale el módulo Az.CloudService de PowerShell.  

    ```powershell
    Install-Module -Name Az.CloudService 
    ```

2. Crear un grupo de recursos. Este paso es opcional si se usa un grupo de recursos existente.   

    ```powershell
    New-AzResourceGroup -ResourceGroupName “ContosOrg” -Location “East US” 
    ```

3. Cree una cuenta de almacenamiento y un contenedor que se usará para almacenar el paquete de servicios en la nube (.cspkg) y los archivos de configuración de servicio (.cscfg). Se debe usar un nombre único para el nombre de la cuenta de almacenamiento. 

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName “ContosOrg” -Name “contosostorageaccount” -Location “East US” -SkuName “Standard_RAGRS” -Kind “StorageV2” 
    $container = New-AzStorageContainer -Name “ContosoContainer” -Context $storageAccount.Context -Permission Blob 
    ```

4. Cargue el paquete de servicios en la nube (cspkg) en la cuenta de almacenamiento.

    ```powershell
    $tokenStartTime = Get-Date 
    $tokenEndTime = $tokenStartTime.AddYears(1) 
    $cspkgBlob = Set-AzStorageBlobContent -File “./ContosoApp/ContosoApp.cspkg” -Container “ContosoContainer” -Blob “ContosoApp.cspkg” -Context $storageAccount.Context 
    $cspkgToken = New-AzStorageBlobSASToken -Container “ContosoContainer” -Blob $cspkgBlob.Name -Permission rwd -StartTime $tokenStartTime -ExpiryTime $tokenEndTime -Context $storageAccount.Context 
    $cspkgUrl = $cspkgBlob.ICloudBlob.Uri.AbsoluteUri + $cspkgToken 
    ```
 

5.  Cargue la configuración de servicios en la nube (cscfg) en la cuenta de almacenamiento. 

    ```powershell
    $cscfgBlob = Set-AzStorageBlobContent -File “./ContosoApp/ContosoApp.cscfg” -Container ContosoContainer -Blob “ContosoApp.cscfg” -Context $storageAccount.Context 
    $cscfgToken = New-AzStorageBlobSASToken -Container “ContosoContainer” -Blob $cscfgBlob.Name -Permission rwd -StartTime $tokenStartTime -ExpiryTime $tokenEndTime -Context $storageAccount.Context 
    $cscfgUrl = $cscfgBlob.ICloudBlob.Uri.AbsoluteUri + $cscfgToken 
    ```

6. Cree una red virtual y una subred. Este paso es opcional si se usan una red y una subred existentes. En este ejemplo se usa una sola red virtual y una subred para ambos roles de servicios en la nube (WebRole y WorkerRole). 

    ```powershell
    $subnet = New-AzVirtualNetworkSubnetConfig -Name "ContosoWebTier1" -AddressPrefix "10.0.0.0/24" -WarningAction SilentlyContinue 
    $virtualNetwork = New-AzVirtualNetwork -Name “ContosoVNet” -Location “East US” -ResourceGroupName “ContosOrg” -AddressPrefix "10.0.0.0/24" -Subnet $subnet 
    ```
 
7. Cree una dirección IP pública y, si lo desea, establezca la propiedad de la etiqueta DNS de la dirección IP pública. Si usa una dirección IP estática, el archivo de configuración de servicio tiene que hacer referencia a ella como IP reservada.  

    ```powershell
    $publicIp = New-AzPublicIpAddress -Name “ContosIp” -ResourceGroupName “ContosOrg” -Location “East US” -AllocationMethod Dynamic -IpAddressVersion IPv4 -DomainNameLabel “contosoappdns” -Sku Basic 
    ```

8. Cree el objeto de perfil de red y asocie la dirección IP pública al front-end del equilibrador de carga creado por la plataforma.  

    ```powershell
    $publicIP = Get-AzPublicIpAddress -ResourceGroupName ContosOrg -Name ContosIp  
    $feIpConfig = New-AzCloudServiceLoadBalancerFrontendIPConfigurationObject -Name 'ContosoFe' -PublicIPAddressId $publicIP.Id 
    $loadBalancerConfig = New-AzCloudServiceLoadBalancerConfigurationObject -Name 'ContosoLB' -FrontendIPConfiguration $feIpConfig 
    $networkProfile = @{loadBalancerConfiguration = $loadBalancerConfig} 
    ```
 
9. Crear un almacén de claves. Este almacén de claves se usará para almacenar los certificados asociados a los roles del servicio en la nube (soporte extendido). El almacén de claves debe estar vinculado a la misma región y suscripción que el servicio en la nube y tener un nombre único. Para más información, consulte [Uso de certificados con Azure Cloud Services (soporte extendido)](certificates-and-key-vault.md).

    ```powershell
    New-AzKeyVault -Name "ContosKeyVault” -ResourceGroupName “ContosoOrg” -Location “East US” 
    ```

10. Actualice la directiva de acceso de Key Vault y conceda permisos de certificados a la cuenta de usuario. 

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosoOrg' -UserPrincipalName 'user@domain.com' -PermissionsToCertificates create,get,list,delete 
    ```

    Como alternativa, establezca la directiva de acceso mediante ObjectId, que puede obtener al ejecutar `Get-AzADUser`. 
    
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosOrg' -ObjectId 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' -PermissionsToCertificates create,get,list,delete 
    ```
 

11. En este ejemplo, agregaremos un certificado autofirmado a un almacén de claves. La huella digital del certificado se debe agregar en el archivo de configuración de servicio en la nube (. cscfg) para implementarse en roles de servicio en la nube. 

    ```powershell
    $Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal 
    Add-AzKeyVaultCertificate -VaultName "ContosKeyVault" -Name "ContosCert" -CertificatePolicy $Policy 
    ```
 
12. Cree un objeto en memoria de perfil de SO. El perfil de SO especifica los certificados que están asociados a roles de servicio en la nube. Se trata del mismo certificado creado en el paso anterior. 

    ```powershell
    $keyVault = Get-AzKeyVault -ResourceGroupName ContosOrg -VaultName ContosKeyVault 
    $certificate = Get-AzKeyVaultCertificate -VaultName ContosKeyVault -Name ContosCert 
    $secretGroup = New-AzCloudServiceVaultSecretGroupObject -Id $keyVault.ResourceId -CertificateUrl $certificate.SecretId 
    $osProfile = @{secret = @($secretGroup)} 
    ```

13. Cree un objeto en memoria de perfil de rol. El perfil de rol define propiedades específicas de SKU de roles, como el nombre, la capacidad y el nivel. En este ejemplo, se han definido dos roles: frontendRole y backendRole. La información del perfil de rol debe coincidir con la configuración de rol definida en el archivo de configuración (cscfg) y el archivo de definición de servicio (csdef). 

    ```powershell
    $frontendRole = New-AzCloudServiceRoleProfilePropertiesObject -Name 'ContosoFrontend' -SkuName 'Standard_D1_v2' -SkuTier 'Standard' -SkuCapacity 2 
    $backendRole = New-AzCloudServiceRoleProfilePropertiesObject -Name 'ContosoBackend' -SkuName 'Standard_D1_v2' -SkuTier 'Standard' -SkuCapacity 2 
    $roleProfile = @{role = @($frontendRole, $backendRole)} 
    ```

14. Si lo desea, cree un objeto en memoria de perfil de extensión que desee agregar al servicio en la nube. En este ejemplo, se agregará la extensión RDP. 

    ```powershell
    $credential = Get-Credential 
    $expiration = (Get-Date).AddYears(1) 
    $extension = New-AzCloudServiceRemoteDesktopExtensionObject -Name 'RDPExtension' -Credential $credential -Expiration $expiration -TypeHandlerVersion '1.2.1' 

    $wadExtension = New-AzCloudServiceDiagnosticsExtension -Name "WADExtension" -ResourceGroupName "ContosOrg" -CloudServiceName "ContosCS" -StorageAccountName "ContosSA" -StorageAccountKey $storageAccountKey[0].Value -DiagnosticsConfigurationPath $configFile -TypeHandlerVersion "1.5" -AutoUpgradeMinorVersion $true 
    $extensionProfile = @{extension = @($rdpExtension, $wadExtension)} 
    ```
15. Si lo desea, defina etiquetas, como en una tabla hash de PowerShell, que desee agregar al servicio en la nube. 

    ```powershell
    $tag=@{"Owner" = "Contoso"} 
    ```

17. Cree una implementación de servicio en la nube mediante objetos de perfil y direcciones URL de SAS.

    ```powershell
    $cloudService = New-AzCloudService ` 
    -Name “ContosoCS” ` 
    -ResourceGroupName “ContosOrg” ` 
    -Location “East US” ` 
    -PackageUrl $cspkgUrl ` 
    -ConfigurationUrl $cscfgUrl ` 
    -UpgradeMode 'Auto' ` 
    -RoleProfile $roleProfile ` 
    -NetworkProfile $networkProfile  ` 
    -ExtensionProfile $extensionProfile ` 
    -OSProfile $osProfile `
    -Tag $tag 
    ```

## <a name="next-steps"></a>Pasos siguientes 
- Consulte las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implemente un servicio en la nube (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
- Visite el [repositorio de ejemplos de Cloud Services (soporte extendido)](https://github.com/Azure-Samples/cloud-services-extended-support)
