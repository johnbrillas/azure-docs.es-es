---
title: Pruebas comparativas de la aplicación en Azure Disk Storage
description: Obtenga información sobre el proceso de comparación de la aplicación en Azure.
author: roygara
ms.author: rogarana
ms.date: 01/29/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: bfda14acc2e50005e25faafa3037805af871c1df
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99094649"
---
# <a name="benchmark-a-disk"></a>Realización de pruebas comparativas de un disco

Las pruebas comparativas consisten en el proceso de simular cargas de trabajo diferentes en la aplicación y medir el rendimiento de las aplicaciones para cada carga de trabajo. Mediante los pasos descritos en el artículo sobre el [diseño con alto rendimiento](premium-storage-performance.md), recopiló los requisitos de rendimiento de las aplicaciones. Al ejecutar las herramientas de pruebas comparativas en las máquinas virtuales en las que se hospeda la aplicación, puede determinar los niveles de rendimiento que la aplicación puede lograr con discos SSD prémium. En este artículo, se proporcionan ejemplos de pruebas comparativas realizadas con una máquina virtual Standard_D8ds_v4 aprovisionada con discos SSD prémium de Azure.

Hemos usado las herramientas de pruebas comparativas comunes DiskSpd y FIO, para Windows y Linux respectivamente. Estas herramientas generan varios subprocesos que simulan una carga de trabajo de producción y miden el rendimiento del sistema. Con estas herramientas, también puede configurar parámetros como la profundidad de la cola y el tamaño de bloque, que normalmente no se puede cambiar de una aplicación. Esto proporciona más flexibilidad para controlar el rendimiento máximo en una máquina virtual a gran escala aprovisionada con discos SSD prémium para diferentes tipos de cargas de trabajo de la aplicación. Para más información sobre la herramienta de pruebas comparativas, visite [DiskSpd](https://github.com/Microsoft/diskspd/wiki/) y [FIO](http://freecode.com/projects/fio).

Para seguir los ejemplos siguientes, cree una máquina virtual Standard_D8ds_v4 y conecte cuatro discos SSD prémium a la máquina virtual. De los cuatro discos, configure tres con almacenamiento en caché de host como "None" y márquelos en un volumen denominado NoCacheWrites. Configure el almacenamiento en caché de host como "ReadOnly" en el disco restante y cree un volumen denominado CacheReads con dicho disco. Con esta configuración, puede ver el rendimiento máximo de lectura y escritura de una máquina virtual Standard_D8ds_v4. Para obtener los pasos detallados para crear una máquina virtual Standard_D8ds_v4 con discos SSD prémium, consulte [Diseño para alto rendimiento](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Pasos siguientes

Diríjase al artículo sobre el [diseño para lograr un alto rendimiento](premium-storage-performance.md).

En dicho artículo, se crea una lista de comprobación similar a la aplicación existente para el prototipo. Con herramientas de pruebas comparativas puede simular las cargas de trabajo y medir el rendimiento de las aplicaciones de prototipo. De este modo, puede determinar qué oferta de disco puede alcanzar o superar los requisitos de rendimiento de las aplicaciones. A continuación, puede implementar las mismas directrices para la aplicación de producción.
