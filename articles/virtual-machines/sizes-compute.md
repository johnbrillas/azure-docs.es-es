---
title: 'Tamaños de las máquinas virtuales de Azure: optimizadas para proceso | Microsoft Docs'
description: Enumera los tamaños diferentes optimizados para proceso disponibles para las máquinas virtuales en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
author: mimckitt
ms.service: virtual-machines
ms.subservice: vm-sizes-compute
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: 01eba7bff571a8db25591b8bfa5c5e712511588f
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102557699"
---
# <a name="compute-optimized-virtual-machine-sizes"></a>Tamaños de máquina virtual optimizada para proceso

Los tamaños de las máquinas virtuales optimizadas para proceso tienen una elevada relación entre CPU y memoria. Estos tamaños son idóneos para servidores web de tráfico medio, aplicaciones de red, procesos por lotes y servidores de aplicaciones. En este artículo se proporciona información sobre el número de vGPU, discos de datos y NIC. También incluye información sobre el rendimiento del almacenamiento y el ancho de banda de red para cada tamaño de esta agrupación.

La [Serie Fsv2](fsv2-series.md) se ejecuta con los procesadores Intel® Xeon® Platino 8272CL (Cascade Lake) de segunda generación y los procesadores Intel® Xeon® Platino 8168 (Skylake). Presenta una velocidad de reloj turbo sostenida de todos los núcleos de hasta 3,4 GHz y una velocidad de reloj turbo de un solo núcleo máxima de 3,7 GHz. Las instrucciones de Intel® AVX-512 son nuevas en los procesadores escalables de Intel. Estas instrucciones pueden llegar a duplicar el rendimiento en las cargas de trabajo de procesamiento vectorial en las operaciones de número de punto flotante de precisión individual y doble. En otras palabras, son muy rápidas para cualquier carga de trabajo de cálculo.

Dado que su precio por hora es inferior, la serie Fsv2 tiene la mejor relación precio/rendimiento de la cartera de Azure según la unidad de proceso de Azure (ACU) por vCPU.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.

Para más información sobre cómo Azure asigna nombres a las máquinas virtuales, consulte [Convenciones de nomenclatura de los tamaños de las máquinas virtuales de Azure](./vm-naming-conventions.md).