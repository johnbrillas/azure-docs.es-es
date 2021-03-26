---
title: Rendimiento del tamaño de las máquinas virtuales de la serie HBv3
description: Conozca los resultado de la prueba de rendimiento de los distintos tamaños de las máquinas virtuales de la serie HBv3 en Azure.
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/12/2021
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 87c3e4e9b509589624a228ea2e1f4b68e86e3fa8
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104721135"
---
# <a name="hbv3-series-virtual-machine-performance"></a>Rendimiento de las máquinas virtuales de la serie HBv3

Los usuarios de acceso anticipado de las máquinas virtuales HBv3 pueden esperar las siguientes cifras de rendimiento en micropuntos de referencia de HPC comunes.

| Carga de trabajo                                        | HBv3                                                              |
|-------------------------------------------------|-------------------------------------------------------------------|
| STREAM Triad                                    | 330-350 GB/s (~82-86 GB/s por NUMA)                                     |
| High-Performance Linpack (HPL)                  | 4 TF (Rpeak, FP64), 8 TF (Rpeak, FP32) para el tamaño de máquina virtual de 120 núcleos               |
| Latencia y ancho de banda de RDMA                        | 1,2 microsegundos (1 byte), 192 GB/s (unidireccional)                                        |
| FIO en SSD de NVMe local (RAID0)                  | 7 GB/s de lecturas, 3 GB/s de escrituras; lecturas de IOPS de 186k, escrituras de IOPS de 201k |

## <a name="process-pinning"></a>Anclaje de procesos

El anclaje de procesos funciona bien en las máquinas virtuales de la serie HBv3 porque exponemos el silicio subyacente tal cual está a la máquina virtual invitada. Se recomienda encarecidamente anclar los procesos para disfrutar de una coherencia y un rendimiento óptimos.

## <a name="mpi-latency"></a>Latencia de MPI

Se puede ejecutar la prueba de latencia de MPI del conjunto de pruebas de micropuntos de referencia OSU según se indica a continuación. Los scripts de ejemplo se encuentran en [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh).

```bash 
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency
``` 
## <a name="mpi-bandwidth"></a>Ancho de banda de MPI
La prueba de ancho de banda de MPI del conjunto de pruebas de micropuntos de referencia OSU se puede ejecutar según se indica a continuación. Los scripts de ejemplo se encuentran en [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh).
```bash
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```
## <a name="mellanox-perftest"></a>Mellanox Perftest
El [paquete Mellanox Perftest](https://community.mellanox.com/s/article/perftest-package) tiene muchas pruebas de InfiniBand, como la de latencia (ib_send_lat) y la de ancho de banda (ib_send_bw). El siguiente es un ejemplo de comando. 
```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```
## <a name="next-steps"></a>Pasos siguientes
- Más información sobre el [escalado de aplicaciones MPI](compiling-scaling-applications.md).
- En los [blogs de Azure Compute Community Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute), encontrará los anuncios más recientes, ejemplos de la carga de trabajo HPC y resultados de HPC.
- Si quiere una visión general de la arquitectura de la ejecución de cargas de trabajo de HPC, consulte [Informática de alto rendimiento (HPC) en Azure](/azure/architecture/topics/high-performance-computing/).