---
title: Máquinas virtuales de Azure de la serie Msv2 (versión preliminar)
description: Especificaciones de las máquinas virtuales de la serie Msv2.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: a7f4757467523837423d52998eb6b8204090e627
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102562578"
---
# <a name="msv2-and-mdsv2-series-medium-memory-preview"></a>Serie Msv2 y Mdsv2 de memoria media (versión preliminar)


> [!IMPORTANT]
> Para unirse a la versión preliminar, rellene el formulario en **https://aka.ms/Mv2MedMemoryPreview** .  

La serie de máquinas virtuales Msv2 y Mdsv2 de memoria media incluye el procesador Intel® Xeon® Platinum 8280 (Cascade Lake) con una frecuencia de base en todos los núcleos de 2,7 GHz y una frecuencia turbo en un único núcleo de 4,0 GHz. Con estas máquinas virtuales, los clientes consiguen una mayor flexibilidad con las opciones de disco local y sin disco. Los clientes también tienen acceso a un conjunto de nuevos tamaños de máquina virtual aislada con más CPU y memoria: hasta 192 vCPU con 4 TiB de memoria. 


Las máquinas virtuales de la serie Msv2 y Mdsv2 solo son de segunda generación y admiten un subconjunto de imágenes compatibles de segunda generación. A continuación, puede encontrar la lista completa de imágenes compatibles de la serie Msv2 y Mdsv2.  

- Windows Server 2019 o posterior
- SUSE Linux Enterprise Server 12 SP4 y las versiones posteriores o SUSE Linux Enterprise Server 15 SP1 y las versiones posteriores.
- Red Hat Enterprise Linux 7.6, 7.7, 8.1 o posterior 
- Oracle Enterprise Linux 7.7 o posterior

Para más información sobre las máquinas virtuales de segunda generación, consulte [Compatibilidad con máquinas virtuales de segunda generación en Azure](./generation-2.md).



[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 2<br>
[Acelerador de escritura](./how-to-enable-write-accelerator.md): Compatible<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>
 
## <a name="msv2-medium-memory-diskless"></a>Msv2 de memoria media sin disco 

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. NIC | Ancho de banda de red esperado (Mbps) | 
|---|---|---|---|---|---|---|---|
| Standard_M32ms_v2 | 32 | 875 | 0 | 32 |  20000/500 | 8 | 8000 | 
| Standard_M64s_v2 | 64 | 1024 | 0 | 64 | 40000/1000 | 8 | 16000 | 
| Standard_M64ms_v2 | 64 | 1792 | 0 | 64 | 40000/1000 | 8 | 16000 | 
| Standard_M128s_v2 | 128 | 2048 | 0 | 64 | 80000/2000 | 8 | 30000 | 
| Standard_M128ms_v2 | 128 | 3892 | 0 | 64 | 80000/2000 | 8 | 30000 | 
| Standard_M192is_v2 | 192 | 2048 | 0 | 64 | 80000/2000 | 8 | 30000 | 
| Standard_M192ims_v2 | 192 | 4096 | 0 | 64 | 80000/2000 | 8 | 30000 | 

## <a name="mdsv2-medium-memory-with-disk"></a>Mdsv2 de memoria media con disco  

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Disco de datos máximo | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. NIC | Ancho de banda de red esperado (Mbps) | 
|---|---|---|---|---|---|---|---|---|
| Standard_M32dms_v2 | 32 | 875 | 1024 | 32 | 40000/400 | 20000/500 | 8 | 8000 | 
| Standard_M64ds_v2 | 64 | 1024 | 2048 | 64 | 80000/800 | 40000/1000 | 8 | 16000 | 
| Standard_M64dms_v2 | 64 | 1792 | 2048 | 64 | 80000/800 | 40000/1000 | 8 | 16000 | 
| Standard_M128ds_v2 | 128 | 2048 | 4096 | 64 |160000/1600 | 80000/2000 | 8 | 30000 | 
| Standard_M128dms_v2 | 128 | 3892 | 4096 | 64 | 160000/1600 | 80000/2000 | 8 | 30000 | 
| Standard_M192ids_v2 | 192 | 2048 | 4096 | 64 | 160000/1600 | 80000/2000 | 8 | 30000 | 
| Standard_M192idms_v2 | 192 | 4096 | 4096 | 64 | 160000/1600 | 80000/2000 | 8 | 30000 | 


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Otros tamaños e información

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

Calculadora de precios: [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
