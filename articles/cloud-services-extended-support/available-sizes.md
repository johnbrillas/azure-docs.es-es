---
title: Tamaños disponibles para Azure Cloud Services (soporte extendido)
description: Tamaños disponibles para las implementaciones de Azure Cloud Services (soporte extendido)
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: c3515b559ef647b9a043a04282b4edfc6e7fa9be
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744030"
---
# <a name="available-sizes-for-azure-cloud-services-extended-support"></a>Tamaños disponibles para Azure Cloud Services (soporte extendido)

En este artículo se describen los tamaños de máquina virtual disponibles para las instancias de Cloud Services (soporte extendido).   

| Familia de SKU |  ACU/núcleo | 
|---|---|
| [A5-7](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series)| 100 |
|[A8-A11](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series---compute-intensive-instances) | 225* |
|[Av2](https://docs.microsoft.com/azure/virtual-machines/av2-series) | 100 | 
|[D](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#d-series) | 160 | 
|[Dv2](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series) | 160 - 190* |
|[Dv3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series) | 160 - 190* |
|[Ev3](https://docs.microsoft.com/azure/virtual-machines/ev3-esv3-series) | 160 - 190*
|[G](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#g-series) | 180-240* |
|[H](https://docs.microsoft.com/azure/virtual-machines/h-series) | 290 - 300* | 

>[!NOTE]
> Las ACU marcadas con un asterisco * usan la tecnología Intel® Turbo para incrementar la frecuencia de CPU y brindar una mejora del rendimiento. El volumen de la mejora puede variar según el tamaño de la máquina virtual, la carga de trabajo y las otras cargas de trabajo que se ejecutan en el mismo host.

## <a name="configure-sizes-for-cloud-services-extended-support"></a>Configuración de tamaños para Cloud Services (soporte extendido)

Puede especificar el tamaño de la máquina virtual de una instancia de rol como parte del modelo de servicio descrito por el archivo de definición de servicio. El tamaño del rol determina la cantidad de núcleos de CPU, la capacidad de memoria y el tamaño del sistema de archivos local.

Por ejemplo, establecer el tamaño de la instancia de rol web en `Standard_D2`: 

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2"> 
</WorkerRole> 
```

## <a name="change-the-size-of-an-existing-role"></a>Cambio de tamaño de un rol existente

Para cambiar el tamaño de un rol existente, cambie el tamaño de la máquina virtual en el archivo de definición de servicio (csdef), vuelva a empaquetar el servicio en la nube y vuelva a implementarlo. 

## <a name="get-a-list-of-available-sizes"></a>Obtención de una lista de los tamaños disponibles 

Para recuperar una lista de los tamaños disponibles, consulte [Resource Skus - List](https://docs.microsoft.com/rest/api/compute/resourceskus/list) y aplique los siguientes filtros:


`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


## <a name="next-steps"></a>Pasos siguientes 
- Revise los [requisitos previos de implementación](deploy-prerequisite.md) para Cloud Services (soporte extendido).
- Consulte las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implemente una instancia de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
