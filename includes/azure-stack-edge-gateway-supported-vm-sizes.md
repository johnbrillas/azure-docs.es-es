---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/09/2020
ms.author: alkohli
ms.openlocfilehash: ae7cb05aeda296ffb3aa9d7f6e2c88fe59364975
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99475054"
---
El tamaño de la máquina virtual determina la cantidad de recursos de proceso (como memoria, CPU y GPU) que están disponibles para la máquina virtual. Debe crear máquinas virtuales con un tamaño adecuado para la carga de trabajo. Aunque todas las máquinas se ejecutarán en el mismo hardware, los tamaños de máquina tienen límites diferentes para el acceso al disco, lo que puede ayudarle a administrar el acceso total al disco en todas las máquinas virtuales. Si una carga de trabajo aumenta, también se puede cambiar el tamaño de una máquina virtual existente.

Se admiten las siguientes máquinas virtuales para la creación en un dispositivo de Azure Stack Edge.

### <a name="dv2-series"></a>Serie Dv2
|Size     |vCPU     |Memoria (GiB) | Tamaño del disco de recursos (GiB)  | Tamaño del disco del SO (GiB) | Discos de datos máx. | Nº máx. NIC |
|-------------------|----|----|-----|----|------|------------|
|**Standard_D1_v2** |1   |3,5 |50   |1000| 4    |2 |
|**Standard_D2_v2** |2   |7   |100  |1000| 8    |4 |
|**Standard_D3_v2** |4   |14  |200  |1000| 16  |4 |
|**Standard_D4_v2** |8   |28  |400  |1000| 32  |8 |
|**Standard_D5_v2** |16  |56  |800  |1000| 64  |8 |
|**Standard_D11_v2** |2   |14  |100  |1000| 8     |2 |
|**Standard_D12_v2** |4   |28  |200  |1000| 16   |4 |
|**Standard_D13_v2** |8   |56  |400  |1000| 32  |8 |

### <a name="dsv2-series"></a>DSv2-series
|Size     |vCPU     |Memoria (GiB) |  Tamaño del disco de recursos (GiB)  | Tamaño del disco del SO (GiB) | Discos de datos máx.| Nº máx. NIC |
|--------------------|----|----|----|-----|------|-------------|
|**Standard_DS1_v2** |1   |3,5 |7  |4000  |1000 |4  |2 |
|**Standard_DS2_v2** |2   |7   |14 |8000  |1000 |8  |4 |
|**Standard_DS3_v2** |4   |14  |28 |16000 |1000 |16 |4 |
|**Standard_DS4_v2** |8   |28  |56 |32000 |1000 |32 |8 |
|**Standard_DS5_v2** |16  |56  |112|64000 |1000 |64 |8 |
|**Standard_DS11_v2**|2   |14  |28 |8000  |1000 |4  |2 |
|**Standard_DS12_v2**|4   |28  |56 |16000 |1000 |8  |4 |
|**Standard_DS13_v2**|8   |56  |112|32000 |1000 |16 |8 |


Para obtener más información, vea [Series Dv2 y Dsv2](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series).

### <a name="ncast4_v3-series-preview"></a>NCasT4_v3-series (versión preliminar)

Estos tamaños se admiten en las máquinas virtuales de GPU incluidas en el dispositivo y están optimizados para aplicaciones con aceleración de GPU de proceso intensivo. Esta serie se centra en las cargas de trabajo de inferencia que incluyen la GPU Tesla T4 de NVIDIA. 

|Size     |vCPU     |Memoria (GiB) | Tamaño del disco de recursos (GiB)  |Tamaño del disco del SO (GiB)| GPU | Memoria de GPU (GiB) | Nº máx. NIC |
|---------------------|----|----|-----|-----|-------|--------------|
|**Standard_NC4as_T4_v3** |4   |28  |180   |1000|1 |16   |4 |
|**Standard_NC8as_T4_v3** |8   |56  |360   |1000|1 |16  |8 |

Para más información, consulte [Serie NCasT4_v3](../articles/virtual-machines/nct4-v3-series.md).

### <a name="f-series"></a>Serie F

Estas series están optimizadas para cargas de trabajo de cálculo y se ejecutan en procesadores Intel Xeon. 

| Size | vCPU | Memoria: GiB |Tamaño del disco de recursos (GiB) |Tamaño del disco del SO (GiB)|  Discos de datos máx. | Nº máx. NIC |
|---|---|---|---|---|---|---|---|---|
| Standard_F1  | 1  | 2   |16      |1000| 4  |  2 |
| Standard_F2 | 2  | 4 |32      |1000| 8  |  4 |
| Standard_F4  | 4  | 8 |64   |1000| 16 |  4 |
| Standard_F8 | 8 | 16  |132    |1000| 32 |  8 |
| Standard_F16 | 16 | 32  |262   |1000| 64 |  8 |
| Standard_F1s | 1 | 2  | 4  |1000| 4 | 1 |
| Standard_F2s | 2 | 4 |8   |1000| 8 | 4 |
| Standard_F4s | 4 | 8 |16 |1000| 16 |  4 |
| Standard_F8s | 8 | 16 |32 |1000| 32 |  8 |
| Standard_F16s | 16 | 32 |64 |1000| 64 |  8 |

Para obtener más información, vea [Serie Fsv2](../articles/virtual-machines/fsv2-series.md).

