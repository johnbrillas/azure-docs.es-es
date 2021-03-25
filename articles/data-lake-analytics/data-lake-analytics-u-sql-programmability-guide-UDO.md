---
title: Guía de programación de UDO con U-SQL para Azure Data Lake
description: Obtenga información general sobre la programación de UDO con U-SQL en Azure Data Lake Analytics para que pueda crear scripts de USQL correctos.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 02360c68e5e830ceee69075fd5532b126d85bec2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512469"
---
# <a name="u-sql-user-defined-objects-overview"></a>Introducción a los objetos definidos por el usuario de U-SQL


## <a name="u-sql-user-defined-objects-udo"></a>U-SQL: objetos definidos por el usuario: UDO
U-SQL le permite definir objetos de programación personalizados, que se denominan objetos definidos por el usuario o UDO.

A continuación aparece una lista de UDO en U-SQL:

* Extractores definidos por el usuario
    * Extraer fila por fila
    * Se usa para implementar la extracción de datos de archivos estructurados personalizados

* Outputters definidos por el usuario
    * Dar salida fila por fila
    * Se usa para la salida de tipos de datos personalizados o para formatos de archivo personalizados

* Procesadores definidos por el usuario
    * Tomar una fila y producir una fila
    * Se usa para reducir el número de columnas o producir nuevas columnas con valores derivados de un conjunto de columnas existente

* Aplicadores definidos por el usuario
    * Tomar una fila y producir de 0 a n filas
    * Se usa con CROSS/OUTER APPLY

* Combinadores definidos por el usuario
    * Combina conjuntos de filas (JOIN definidas por el usuario)

* Reductores definidos por el usuario
    * Tomar n filas y producir una fila
    * Se usa para reducir el número de filas

UDO normalmente se llama explícitamente en el script U-SQL como parte de las siguientes instrucciones U-SQL:

* EXTRACT
* OUTPUT
* PROCESS
* COMBINE
* REDUCE

> [!NOTE]  
> Los UDO están limitados a un consumo de 0,5 GB de memoria.  Esta limitación de memoria no se aplica a las ejecuciones locales.

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)