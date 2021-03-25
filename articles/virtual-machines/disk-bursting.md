---
title: Seguridad de disco administrado
description: Obtenga información sobre la expansión de disco para discos de Azure y para máquinas virtuales de Azure
author: albecker1
ms.author: albecker
ms.date: 03/02/2021
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 4024d2b1357f3dda8216e9ebdd2055b28b064d33
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101677477"
---
# <a name="managed-disk-bursting"></a>Seguridad de disco administrado
[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting.md)]

Los [discos SSD Prémium](disks-types.md#premium-ssd) de Azure ofrecen dos modelos de expansión:

- Un modelo de expansión a petición (versión preliminar), en el que el disco se expande cada vez que sus necesidades superan su capacidad actual. Este modelo incurre en cargos adicionales cada vez que el disco se expande. La expansión sin crédito solo está disponible en discos con un tamaño superior a 512 GiB.
- Un modelo basado en el crédito, en el que el disco solo se expandirá si tiene créditos de expansión acumulados en su cubo de crédito. Este modelo no incurrirá en cargos adicionales cuando el disco se expanda. La expansión basada en el crédito solo está disponible en los discos de 512 GiB, y menores.

Además, el [nivel de rendimiento de los discos administrados puede cambiar](disks-change-performance.md), lo que podría ser ideal si la carga de trabajo se ejecutara en la expansión.

|  |Expansión basada en crédito  |Expansión a petición  |Cambio del nivel de rendimiento  |
|---------|---------|---------|---------|
| Escenarios|Ideal para el escalado a corto plazo (30 minutos o menos).|Ideal para el escalado a corto plazo (sin restricción de tiempo).|Ideal si la carga de trabajo se ejecutaría continuamente en la expansión.|
|Coste     |Gratuito         |El costo es variable; para más información, consulte la sección de [facturación](#billing).        |El costo de cada nivel de rendimiento es fijo, para más información, consulte [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).         |
|Disponibilidad     |Solo está disponible para SSD Prémium con un tamaño de 512 GiB, y menores.         |Solo está disponible para SSD Prémium de más dé que 512 GiB.         |Disponible para todos los tamaños de SSD Prémium.         |
|Habilitación     |Habilitado de forma predeterminada en los discos aptos.         |Debe habilitarlo el usuario.         |El usuario debe cambiar su nivel de forma manual.         |

## <a name="common-scenarios"></a>Escenarios frecuentes
Los siguientes escenarios pueden beneficiarse en gran medida de la expansión:
- **Mejora de los tiempos de arranque**: con la expansión, la instancia se iniciará a una velocidad considerablemente más rápida. Por ejemplo, el disco de SO predeterminado para las VM habilitadas para Premium es el P4, con un rendimiento aprovisionado de hasta 120 IOPS y 25 MB/s. Con la expansión, el disco P4 puede llegar hasta 3500 IOPS y 170 MB/s, lo que permite multiplicar por 6 la aceleración del arranque.
- **Controlar trabajos por lotes**: algunas cargas de trabajo de aplicaciones son cíclicas por naturaleza. Requieren un rendimiento de línea base la mayor parte del tiempo y un rendimiento mayor durante breves períodos de tiempo. Un ejemplo de esto es un programa de contabilidad que procesa transacciones diariamente y que requiere una pequeña cantidad de tráfico de disco. Al finalizar el mes, el programa finalizará la conciliación de los informes que requieren una cantidad mucho mayor de tráfico de disco.
- **Picos de tráfico**: tanto los servidores web como sus aplicaciones pueden experimentar picos de tráfico en cualquier momento. Si el servidor web está respaldado por máquinas virtuales o discos que usan la expansión, los servidores estarán mejor equipados para controlar los picos de tráfico. 

[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting-2.md)]

## <a name="next-steps"></a>Pasos siguientes

Para habilitar la expansión a petición, consulte [Habilitación de la expansión a petición](disks-enable-bursting.md).
Para aprender a obtener información sobre los recursos de expansión, consulte [Métricas de expansión de disco](disks-metrics.md).
