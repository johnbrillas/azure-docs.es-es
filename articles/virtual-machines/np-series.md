---
title: 'Serie NP: Azure Virtual Machines'
description: Especificaciones de las VM de la serie NP.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.date: 02/09/2021
ms.author: vikancha
ms.openlocfilehash: 09adb19623ea866091e1b949e78263661eddbb52
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102551154"
---
# <a name="np-series-preview"></a>Serie NP (versión preliminar) 
Las máquinas virtuales de la serie NP cuentan con tecnología de FPGA [Xilinx U250 ](https://www.xilinx.com/products/boards-and-kits/alveo/u250.html) para acelerar las cargas de trabajo, como inferencia de aprendizaje automático, transcodificación de vídeo, y búsqueda y análisis de bases de datos. Las VM de la serie NP también cuentan con tecnología de CPU Intel Xeon 8171M (Skylake) con toda la velocidad de reloj de la turbo de 3,2 GHz.

Envíe una solicitud con el [formulario de versión preliminar](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR9x_QCQkJXxHl4qOI4jC9YtUOVI0VkgwVjhaTFFQMTVBTDFJVFpBMzJSSCQlQCN0PWcu) para que forme parte del programa de la versión preliminar de la serie NP.


[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
Compatibilidad con generación de VM: Generación 1<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | FPGA | Memoria de FPGA: GiB | Discos de datos máx. | N.º máx. de NIC/ancho de banda de red esperado (Mbps) | 
|---|---|---|---|---|---|---|---|
| Standard_NP10s | 10 | 168 | 736  | 1 | 64  | 8 | 1 / 7500 | 
| Standard_NP20s | 20 | 336 | 1474 | 2 | 128 | 16 | 2 / 15000 | 
| Standard_NP40s | 40 | 672 | 2948 | 4 | 256 | 32 | 4 / 30000 | 



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles
Visite las [notas de la versión de Xilinx Runtime (XRT)](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/ug1451-xrt-release-notes.pdf) para obtener la lista completa de los sistemas operativos compatibles.

Durante el programa de versión preliminar, los equipos de ingeniería de Microsoft Azure compartirán instrucciones específicas para la instalación de controladores.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
