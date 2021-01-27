---
title: Información general e implementación de máquinas virtuales con GPU en el dispositivo Azure Stack Edge Pro
description: Describe cómo crear y administrar máquinas virtuales con GPU en un dispositivo Azure Stack Edge Pro mediante plantillas.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: 7534052412c2bee0f31e352fc577d376c11215c3
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98804928"
---
# <a name="gpu-vms-for-your-azure-stack-edge-pro-device"></a>Máquinas virtuales con GPU para el dispositivo Azure Stack Edge Pro

En este artículo se proporciona información general sobre las máquinas virtuales con GPU en el dispositivo Azure Stack Edge Pro. En el artículo se describe cómo crear una máquina virtual con GPU e instalar la extensión del controlador de GPU para instalar los controladores de Nvidia adecuados. Use las plantillas de Azure Resource Manager para crear la máquina virtual con GPU e instalar la extensión del controlador de GPU. 

Este artículo es aplicable a los dispositivos Azure Stack Edge Pro con GPU y Azure Stack Edge Pro R.

## <a name="about-gpu-vms"></a>Acerca de las máquinas virtuales con GPU

Los dispositivos Azure Stack Edge Pro están equipados con 1 o 2 GPU Tesla T4 de Nvidia. Para implementar cargas de trabajo de máquinas virtuales con aceleración de GPU en estos dispositivos, use tamaños de máquina virtual optimizados para GPU. Por ejemplo, se debe usar la serie NC T4 v3 para implementar cargas de trabajo de inferencia que incluyen las GPU T4. 

Para más información, consulte [Serie NCasT4_v3](../virtual-machines/nct4-v3-series.md).

## <a name="supported-os-and-gpu-drivers"></a>Sistema operativo y controladores de GPU admitidos 

Para aprovechar las funcionalidades de GPU de las máquinas virtuales de la serie N de Azure, se deben instalar los controladores de GPU de Nvidia. 

La extensión del controlador de GPU de Nvidia instala los controladores CUDA o GRID de Nvidia adecuados. Puede instalar o administrar la extensión mediante las plantillas de Azure Resource Manager.

### <a name="supported-os-for-gpu-extension-for-windows"></a>Sistema operativo admitido para la extensión de GPU para Windows

Esta extensión admite los siguientes sistemas operativos. Otras versiones pueden funcionar, pero no se han probado internamente en máquinas virtuales con GPU que se ejecutan en dispositivos Azure Stack Edge Pro.

| Distribución | Versión |
|---|---|
| Windows Server 2019 | Core |
| Windows Server 2016 | Core |

### <a name="supported-os-for-gpu-extension-for-linux"></a>Sistema operativo admitido para la extensión de GPU para Linux

Esta extensión admite las siguientes distribuciones del sistema operativo, en función de la compatibilidad del controlador para la versión específica del sistema operativo. Otras versiones pueden funcionar, pero no se han probado internamente en máquinas virtuales con GPU que se ejecutan en dispositivos Azure Stack Edge Pro.


| Distribución | Versión |
|---|---|
| Ubuntu | 18.04 LTS |
| Red Hat Enterprise Linux | 7.4 |


## <a name="gpu-vms-and-kubernetes"></a>Máquinas virtuales con GPU y Kubernetes

Antes de implementar máquinas virtuales con GPU en el dispositivo, revise las siguientes consideraciones si Kubernetes está configurado en el dispositivo.

#### <a name="for-1-gpu-device"></a>Para un dispositivo con 1 GPU: 

- **Cree una máquina virtual con GPU seguida de la configuración de Kubernetes en el dispositivo**: en este escenario, la creación de la máquina virtual con GPU y la configuración de Kubernetes se realizarán correctamente. En este caso, Kubernetes no tendrá acceso a la GPU.

- **Configure Kubernetes en el dispositivo y, después, cree una máquina virtual con GPU**: en este escenario, Kubernetes reclamará la GPU del dispositivo y se producirá un error en la creación de la máquina virtual, ya que no hay ningún recurso de GPU disponible.

#### <a name="for-2-gpu-device"></a>Para un dispositivo con 2 GPU:

- **Cree una máquina virtual con GPU seguida de la configuración de Kubernetes en el dispositivo**: en este escenario, la máquina virtual con GPU que cree reclamará una GPU del dispositivo y la configuración de Kubernetes también se realizará correctamente y reclamará la GPU restante. 

- **Cree dos máquinas virtuales con GPU seguida de la configuración de Kubernetes en el dispositivo**: en este escenario, las dos máquinas virtuales con GPU reclamarán las dos GPU del dispositivo y Kubernetes se configurará correctamente sin GPU. 

- **Configure Kubernetes en el dispositivo y, después, cree una máquina virtual con GPU**: en este escenario, Kubernetes reclamará ambas GPU del dispositivo y se producirá un error en la creación de la máquina virtual, ya que no hay ningún recurso de GPU disponible.

Si tiene máquinas virtuales con GPU que se ejecutan en el dispositivo y Kubernetes también está configurado, cada vez que se cancele la asignación de la máquina virtual (cuando detenga o elimine una máquina virtual con Stop-AzureRmVM o Remove-AzureRmVM), existe el riesgo de que el clúster de Kubernetes reclame todas las GPU disponibles del dispositivo. En tal caso, no podrá reiniciar las máquinas virtuales con GPU implementadas en el dispositivo ni crear máquinas virtuales con GPU.


## <a name="create-gpu-vms"></a>Creación de máquinas virtuales con GPU

Siga estos pasos al implementar máquinas virtuales con GPU en el dispositivo:

1. Identifique si el dispositivo también va a ejecutar Kubernetes. Si el dispositivo va a ejecutar Kubernetes, primero deberá crear la máquina virtual con GPU y luego configurar Kubernetes. Si Kubernetes se configura en primer lugar, reclamará todos los recursos de GPU disponibles y se producirá un error en la creación de la máquina virtual con GPU.

1. [Descargue las plantillas de máquina virtual y los archivos de parámetros](https://aka.ms/ase-vm-templates) en la máquina cliente. Descomprímalos en un directorio que usará como directorio de trabajo.

1. Para crear máquinas virtuales con GPU, siga todos los pasos descritos en [Implementación de máquinas virtuales en el dispositivo Azure Stack Edge Pro con GPU mediante plantillas](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) excepto en las siguientes diferencias: 

    1. Durante la configuración de la red de proceso, habilite el puerto que está conectado a Internet para proceso. Esto le permite descargar los controladores de GPU necesarios para las extensiones de GPU para las máquinas virtuales con GPU.

        Este es un ejemplo en el que el puerto 2 estaba conectado a Internet y se usaba para habilitar la red de proceso. Si ha identificado que Kubernetes no es necesario en el paso anterior, puede omitir la dirección IP del nodo de Kubernetes y la asignación de direcciones IP del servicio externo.

        ![Habilitación de la configuración de proceso en el puerto conectado a Internet](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/enable-compute-network-1.png)

            
    1. Cree una máquina virtual con las plantillas. Al especificar los tamaños de la máquina virtual con GPU, asegúrese de usar la serie NCasT4-V3 en el archivo `CreateVM.parameters.json`, ya que se admite para las máquinas virtuales con GPU. Para más información, consulte [Tamaños y tipos de máquina virtual para el dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-virtual-machine-sizes.md#ncast4_v3-series-preview).

        ```json
            "vmSize": {
          "value": "Standard_NC4as_T4_v3"
        },
        ```

    1. Una vez que la máquina virtual con GPU se crea correctamente, puede ver esta máquina virtual en la lista de máquinas virtuales del recurso de Azure Stack Edge en Azure Portal.

        ![Máquina virtual con GPU en la lista de máquinas virtuales de Azure Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/list-virtual-machine-1.png)

1. Seleccione la máquina virtual y explore en profundidad los detalles. Copie la dirección IP asignada a la máquina virtual.

    ![Dirección IP asignada a la máquina virtual con GPU en Azure Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/get-ip-gpu-virtual-machine-1.png)

1. Una vez creada la máquina virtual, implemente la extensión de GPU mediante la plantilla de extensiones. En el caso de las máquinas virtuales Linux, consulte [Extensión de GPU para Linux](#gpu-extension-for-linux) y, para máquinas virtuales Windows, consulte [Extensión de GPU para Windows](#gpu-extension-for-windows).

1. Para comprobar la instalación de la extensión de GPU, conéctese a la máquina virtual con GPU:
    1. Si usa una máquina virtual Windows, siga los pasos descritos en [Conexión a una máquina virtual Windows](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-windows-vm). [Compruebe la instalación](#verify-windows-driver-installation).
    1. Si usa una máquina virtual Linux, siga los pasos descritos en [Conexión a una máquina virtual Linux](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-linux-vm). [Compruebe la instalación](#verify-linux-driver-installation).

1. Si es necesario, puede volver a cambiar la red de proceso. 


> [!NOTE]
> Al actualizar la versión del software del dispositivo de la versión 2012 a una posterior, tendrá que detener manualmente las máquinas virtuales con GPU.


## <a name="install-gpu-extension"></a>Instalación de la extensión de GPU

En función del sistema operativo de la máquina virtual, podría instalar la extensión de GPU para Windows o para Linux.

> [!NOTE]
> Antes de instalar la extensión de GPU, asegúrese de que el puerto habilitado para la red de proceso en el dispositivo está conectado a Internet y tiene acceso. Los controladores de GPU se descargan mediante el acceso a Internet.

### <a name="gpu-extension-for-windows"></a>Extensión de GPU para Windows

Para implementar los controladores de GPU de Nvidia para una máquina virtual existente, edite el archivo de parámetros `addGPUExtWindowsVM.parameters.json` y, a continuación, implemente la plantilla `addGPUextensiontoVM.json`.

#### <a name="edit-parameters-file"></a>Edición del archivo de parámetros

El archivo `addGPUExtWindowsVM.parameters.json` toma los parámetros siguientes:

```json
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: windowsGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverWindows" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DriverURL" : "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
    "DriverCertificateUrl" : "https://go.microsoft.com/fwlink/?linkid=871664",
    "DriverType":"CUDA"
    }
    }
    }
```

Este es un archivo de parámetros de ejemplo que se usó en este artículo:

```powershell
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\CreateVM\CreateVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\CreateVM\CreateVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment2"

DeploymentName          : deployment2
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:02:56 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          adminUsername    String                     Administrator
                          password         String                     Password1
                          imageName        String                     myasewindowsimg
                          vmSize           String                     Standard_NC4as_T4_v3
                          vnetName         String                     ASEVNET
                          vnetRG           String                     aserg
                          subnetName       String                     ASEVNETsubNet
                          nicName          String                     nic6
                          ipConfigName     String                     ipconfig6
                          privateIPAddress  String

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```
#### <a name="deploy-template"></a>Implementar plantilla

Implemente la plantilla `addGPUextensiontoVM.json`. Esta plantilla implementa la extensión en una máquina virtual existente. Ejecute el siguiente comando:

```powershell
$templateFile = "<Path to addGPUextensiontoVM.json>" 
$templateParameterFile = "<Path to addGPUExtWindowsVM.parameters.json>" 
$RGName = "<Name of your resource group>"
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
```
> [!NOTE]
> La implementación de la extensión es un trabajo de larga duración y tarda unos 10 minutos en completarse.

Este es una salida de ejemplo:

```powershell
PS C:\WINDOWS\system32> "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtWindowsVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment3"

DeploymentName          : deployment3
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:18:50 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          extensionName    String                     windowsgpuext
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverWindows
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
                          }

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```
#### <a name="track-deployment"></a>Seguimiento de la implementación

Para comprobar el estado de implementación de las extensiones de una máquina virtual determinada, ejecute el comando siguiente: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName <Name of resource group> -VMName <Name of VM> -Name <Name of the extension>
```
Este es una salida de ejemplo:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM2 -Name windowsgpuext

ResourceGroupName       : myasegpuvm1
VMName                  : VM2
Name                    : windowsgpuext
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverWindows
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/myasegpuvm1/providers/Microsoft.Compute/virtualMachines/VM2/extensions/windowsgpuext
PublicSettings          : {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

La salida de la ejecución de las extensiones se registra en el archivo siguiente: Consulte el archivo `C:\Packages\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverWindows\1.3.0.0\Status` para realizar un seguimiento del estado de la instalación. 


Una instalación correcta se indica mediante el elemento `message` establecido como `Enable Extension` y el elemento `status` establecido como `success`.

```powershell
"status":  {
                       "formattedMessage":  {
                                                "message":  "Enable Extension",
                                                "lang":  "en"
                                            },
                       "name":  "NvidiaGpuDriverWindows",
                       "status":  "success",
```

#### <a name="verify-windows-driver-installation"></a>Comprobación de la instalación del controlador de Windows

Inicie sesión en la máquina virtual y ejecute la utilidad de la línea de comandos nvidia-smi instalada con el controlador. El archivo `nvidia-smi.exe` se encuentra en `C:\Program Files\NVIDIA Corporation\NVSMI\nvidia-smi.exe`. Si no ve el archivo, es posible que la instalación del controlador siga ejecutándose en segundo plano. Espere 10 minutos y vuelva a comprobarlo.

Si el controlador está instalado, verá una salida similar a la del ejemplo siguiente: 

```powershell
PS C:\Users\Administrator> cd "C:\Program Files\NVIDIA Corporation\NVSMI"
PS C:\Program Files\NVIDIA Corporation\NVSMI> ls

    Directory: C:\Program Files\NVIDIA Corporation\NVSMI

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/26/2020  12:00 PM         849640 MCU.exe
-a----        2/26/2020  12:00 PM         443104 nvdebugdump.exe
-a----        2/25/2020   2:06 AM          81823 nvidia-smi.1.pdf
-a----        2/26/2020  12:01 PM         566880 nvidia-smi.exe
-a----        2/26/2020  12:01 PM         991344 nvml.dll

PS C:\Program Files\NVIDIA Corporation\NVSMI> .\nvidia-smi.exe
Wed Dec 16 00:35:51 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 442.50       Driver Version: 442.50       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla T4            TCC  | 0000503C:00:00.0 Off |                    0 |
| N/A   35C    P8    11W /  70W |      8MiB / 15205MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
PS C:\Program Files\NVIDIA Corporation\NVSMI>
```

Para más información, consulte [Extensión del controlador de GPU de NVIDIA para Windows](../virtual-machines/extensions/hpccompute-gpu-windows.md).

### <a name="gpu-extension-for-linux"></a>Extensión de GPU para Linux

Para implementar los controladores de GPU de Nvidia para una máquina virtual existente, edite el archivo de parámetros y, a continuación, implemente la plantilla `addGPUextensiontoVM.json`. Hay determinados archivos de parámetros para Ubuntu y Red Hat Enterprise Linux (RHEL), como se describe en las secciones siguientes.

#### <a name="edit-parameters-file"></a>Edición del archivo de parámetros

Si usa Ubuntu, el archivo `addGPUExtLinuxVM.parameters.json` toma los parámetros siguientes:

```powershell
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: linuxGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverLinux" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
    "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
    "CUDA_ver": "10.0.130",
    "InstallCUDA": "true"
    }
    }
    }
```
Si usa Red Hat Enterprise Linux (RHEL), el archivo `addGPUExtensionRHELVM.parameters.json` toma los parámetros siguientes:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<name of the VM>" 
        },
        "extensionName": {
            "value": "<name for the extension. Example: linuxGpu>" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
                    "isCustomInstall":true,
                    "DRIVER_URL":"https://go.microsoft.com/fwlink/?linkid=874273",
                    "CUDA_ver":"10.0.130",
                    "PUBKEY_URL":"http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                    "DKMS_URL":"https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm",
                    "LIS_URL":"https://aka.ms/lis",
                    "LIS_RHEL_ver":"3.10.0-1062.9.1.el7"
            }
        }
    }
}
```


Este es un archivo de parámetros de Ubuntu de ejemplo que se usó en este artículo:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "VM1" 
        },
        "extensionName": {
            "value": "gpuLinux" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
            "CUDA_ver": "10.0.130",
            "InstallCUDA": "true"
            }
        }
    }
}
```


#### <a name="deploy-template"></a>Implementar plantilla

Implemente la plantilla `addGPUextensiontoVM.json`. Esta plantilla implementa la extensión en una máquina virtual existente. Ejecute el siguiente comando:

```powershell
$templateFile = "Path to addGPUextensiontoVM.json" 
$templateParameterFile = "Path to addGPUExtLinuxVM.parameters.json" 
$RGName = "<Name of your resource group>" 
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
``` 

> [!NOTE]
> La implementación de la extensión es un trabajo de larga duración y tarda unos 10 minutos en completarse.

Este es una salida de ejemplo:

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtLinuxVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "rg2"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "delpoyment7"

DeploymentName          : delpoyment7
ResourceGroupName       : rg2
ProvisioningState       : Succeeded
Timestamp               : 12/10/2020 10:43:23 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM1
                          extensionName    String                     gpuLinux
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverLinux
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL":
                          "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
                          }

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```

#### <a name="track-deployment-status"></a>Seguimiento del estado de la implementación    
    
La implementación de la plantilla es un trabajo de larga duración. Para comprobar el estado de implementación de las extensiones de una máquina virtual determinada, abra otra sesión de PowerShell (ejecutar como administrador). Ejecute el siguiente comando: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName <VM Name> -Name <Extension Name>
```
Este es una salida de ejemplo: 

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName rg2 -VMName VM1 -Name gpulinux

ResourceGroupName       : rg2
VMName                  : VM1
Name                    : gpuLinux
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverLinux
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/rg2/providers/Microsoft.Compute/virtualMachines/VM1/extensions/gpuLinux
PublicSettings          : {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

> [!NOTE]
> Una vez completada la implementación, `ProvisioningState` cambia a `Succeeded`.

La salida de la ejecución de las extensiones se registra en el archivo siguiente: `/var/log/azure/nvidia-vmext-status`.

#### <a name="verify-linux-driver-installation"></a>Comprobación de la instalación del controlador de Linux

Siga estos pasos para comprobar la instalación del controlador:

1. Conéctese a la máquina virtual con GPU. Siga las instrucciones que se indican en [Conexión a una máquina virtual Linux](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-linux-vm). 

    Este es una salida de ejemplo:

    ```powershell
    PS C:\WINDOWS\system32> ssh -l Administrator 10.57.50.60
    Administrator@10.57.50.60's password:
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
      System information as of Thu Dec 10 22:57:01 UTC 2020
    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
    
    249 packages can be updated.
    140 updates are security updates.
    
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage    
      System information as of Thu Dec 10 22:57:01 UTC 2020    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
        
    249 packages can be updated.
    140 updates are security updates.
    
    New release '20.04.1 LTS' available.
    Run 'do-release-upgrade' to upgrade to it.
        
    *** System restart required ***
    Last login: Thu Dec 10 21:49:29 2020 from 10.90.24.23
    To run a command as administrator (user "root"), use "sudo <command>".
    See "man sudo_root" for details.
    
    Administrator@VM1:~$
    ```
2. Ejecute la utilidad de la línea de comandos nvidia-smi instalada con el controlador. Si el controlador se ha instalado correctamente, podrá ejecutar la utilidad y ver la siguiente salida:

    ```powershell
    Administrator@VM1:~$ nvidia-smi
    Thu Dec 10 22:58:46 2020
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            Off  | 0000941F:00:00.0 Off |                    0 |
    | N/A   48C    P0    27W /  70W |      0MiB / 15109MiB |      5%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    Administrator@VM1:~$
    ```

Para más información, consulte [Extensión del controlador de GPU de NVIDIA para Linux](../virtual-machines/extensions/hpccompute-gpu-linux.md).

## <a name="remove-gpu-extension"></a>Eliminación de la extensión de GPU

Para eliminar la extensión de GPU, use el siguiente comando:

`Remove-AzureRmVMExtension -ResourceGroupName <Resource group name> -VMName <VM name> -Name <Extension name>`

Este es una salida de ejemplo:

```powershell
PS C:\azure-stack-edge-deploy-vms> Remove-AzureRmVMExtension -ResourceGroupName rgl -VMName WindowsVM -Name windowsgpuext
Virtual machine extension removal operation
This cmdlet will remove the specified virtual machine extension. Do you want to continue? [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): y
Requestld IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------    
          True                OK         OK
```




## <a name="next-steps"></a>Pasos siguientes

[Cmdlets de Azure Resource Manager](/powershell/module/azurerm.resources/?view=azurermps-6.13.0&preserve-view=true)