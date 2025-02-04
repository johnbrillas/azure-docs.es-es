---
title: 'Series Dv4 y Dsv4: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de las series Dv4 y Dsv4.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: vm-sizes-general
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: cc6ce30855d17f45636e0df04978fed88dcecff7
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102549148"
---
# <a name="dv4-and-dsv4-series"></a>Series Dv4 y Dsv4

Las máquinas virtuales de las series Dv4 y Dsv4 se ejecutan en los procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) en una configuración con tecnología Hyper-Threading, lo que proporciona una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general. Cuenta con una velocidad de reloj en todos los núcleos de 3,4 GHz. 

> [!NOTE]
> Para consultar las preguntas más frecuentes, consulte [Tamaños de máquina virtual de Azure sin disco temporal local](azure-vms-no-temp-disk.md).
## <a name="dv4-series"></a>Serie Dv4

Los tamaños de la serie Dv4 se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Los tamaños de la serie Dv4 ofrecen una combinación de vCPU, memoria y almacenamiento remoto adecuados para la mayoría de las cargas de trabajo de producción. Las máquinas virtuales de la serie Dv4 cuentan con la [tecnología Hyper-Threading de Intel&reg;](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html).

El almacenamiento en disco de datos remotos se factura de forma independiente a las máquinas virtuales. Para usar discos de Premium Storage, utilice los tamaños de la serie Dsv4. Los precios y los medidores de facturación de los tamaños de la serie Dsv4 son los mismos que para la serie Dv4.

[ACU](acu.md): 195-210<br>
[Premium Storage](premium-storage-performance.md): No compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): No compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible (*se requieren 4 vCPU como mínimo*)<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Nº máx. NIC|Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|
| Standard_D2_v4 | 2 | 8 | Solo almacenamiento remoto | 4 | 2|1000 |
| Standard_D4_v4 | 4 | 16  | Solo almacenamiento remoto | 8 | 2|2000 |
| Standard_D8_v4 | 8 | 32 | Solo almacenamiento remoto | 16 | 4|4000 |
| Standard_D16_v4 | 16 | 64 | Solo almacenamiento remoto | 32 | 8|8000 |
| Standard_D32_v4 | 32 | 128 | Solo almacenamiento remoto | 32 | 8|16000 |
| Standard_D48_v4 | 48 | 192 | Solo almacenamiento remoto | 32 | 8|24000 |
| Standard_D64_v4 | 64 | 256 | Solo almacenamiento remoto | 32 | 8|30000 |

## <a name="dsv4-series"></a>Serie Dsv4

Los tamaños de la serie Dsv4 se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Los tamaños de la serie Dv4 ofrecen una combinación de vCPU, memoria y almacenamiento remoto adecuados para la mayoría de las cargas de trabajo de producción. Las máquinas virtuales de la serie Dsv4 cuentan con la [tecnología Hyper-Threading de Intel&reg;](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html). El almacenamiento en disco de datos remotos se factura de forma independiente a las máquinas virtuales.

[ACU](acu.md): 195-210<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible (*se requieren 4 vCPU como mínimo*)<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. NIC|Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_D2s_v4 | 2 | 8  | Solo almacenamiento remoto | 4 | 3200/48 | 2|1000 |
| Standard_D4s_v4 | 4 | 16 | Solo almacenamiento remoto | 8 | 6400/96 | 2|2000 |
| Standard_D8s_v4 | 8 | 32 | Solo almacenamiento remoto | 16 | 12800/192 | 4|4000 |
| Standard_D16s_v4 | 16 | 64  | Solo almacenamiento remoto | 32 | 25600/384 | 8|8000 |
| Standard_D32s_v4 | 32 | 128 | Solo almacenamiento remoto | 32 | 51200/768 | 8|16000 |
| Standard_D48s_v4 | 48 | 192 | Solo almacenamiento remoto | 32 | 76800/1152 | 8|24000 |
| Standard_D64s_v4 | 64 | 256 | Solo almacenamiento remoto | 32 | 80000/1200 | 8|30000 |
