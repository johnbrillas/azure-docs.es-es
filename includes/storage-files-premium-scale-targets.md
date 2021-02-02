---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: 86bf4911026e46c997469b956f9e7c75c4f17164
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98698001"
---
#### <a name="additional-premium-file-share-level-limits"></a>Límites del nivel de recurso compartido de archivos prémium adicionales

|Área  |Destino  |
|---------|---------|
|Aumento o disminución del tamaño mínimo    |1 GiB      |
|IOPS base    |400 + 1 IOPS por GiB hasta 100 000|
|Ampliación de IOPS    |Máx. (4000, 3 x IOPS por GiB), hasta 100 000|
|Velocidad de salida         |60 MiB/s + 0,06 * GiB aprovisionado        |
|Velocidad de entrada| 40 MiB/s + 0,04 * GiB aprovisionado |

#### <a name="file-level-limits"></a>Límites de nivel de archivo

|Área  |Archivo estándar  |Archivo prémium  |
|---------|---------|---------|
|Size     |1 TiB         |4 TiB         |
|IOPS máximas por archivo      |1,000         |Hasta 8000*         |
|Identificadores simultáneos     |2\.000         |2\.000         |
|Salida     |Consulte los valores de rendimiento de archivo estándar         |300 MiB/s (hasta 1 GiB/s con la versión SMB multicanal)**         |
|Entrada     |Consulte los valores de rendimiento de archivo estándar         |200 MiB/s (hasta 1 GiB/s con la versión preliminar de SMB multicanal)**        |
|Throughput     |Hasta 60 MiB/s         |Consulte los valores de entrada y salida de archivos prémium         |

\* <sup> Se aplica a entradas y salidas de lectura y escritura (normalmente tamaños de E/S más pequeños <= 64 K). Las operaciones de metadatos, que no sean lecturas y escrituras, pueden ser más lentas. </sup>

\*\* <sup> En función de los límites de red de la máquina, el ancho de banda disponible, los tamaños de E/S, la profundidad de la cola y otros factores. Para más información, consulte [Rendimiento de SMB multicanal](../articles/storage/files/storage-files-smb-multichannel-performance.md). </sup>
