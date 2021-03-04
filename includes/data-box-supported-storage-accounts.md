---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 02/21/2021
ms.author: alkohli
ms.openlocfilehash: 112c30fdd242c20f11c43f42ba54e3717e074bbb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101706050"
---
Aquí se proporciona una lista de los tipos de cuentas de almacenamiento y almacenamientos compatibles con un dispositivo Data Box. Para una lista completa de todas las funcionalidades para todos los tipos de cuentas de almacenamiento, consulte [Tipos de cuentas de almacenamiento](../articles/storage/common/storage-account-overview.md#types-of-storage-accounts).

En el caso de los pedidos de importación, en la siguiente tabla se muestran las cuentas de almacenamiento admitidas.

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas** _ |_ *Archivos de Azure** |**Notas**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y |
| Estándar de uso general v1  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v2  |  |Y | | |
| FileStorage Premium de Azure |  |  | Y |  |  
| Estándar de Blob Storage |Y | | |Se admiten frecuentes y esporádicos. |

\* *: los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.*

En el caso de los pedidos de exportación, en la siguiente tabla se muestran las cuentas de almacenamiento admitidas.

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas** _ |_ *Archivos de Azure** |**Niveles de acceso admitidos**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y | |
| Estándar de uso general v1  | Y | Y | Y | Acceso frecuente y esporádico|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Acceso frecuente y esporádico|
| Premium de uso general v2  |  |Y | | |
| FileStorage Premium de Azure |  |  | Y |  |
| Estándar de Blob Storage |Y | | |Acceso frecuente y esporádico |
| Almacenamiento prémium de blobs en bloques |Y | | |Acceso frecuente y esporádico |
| Almacenamiento prémium de blobs en páginas | |Y | | |

> [!IMPORTANT]
> - En el caso de las cuentas de uso general, Data Box no admite los tipos de almacenamiento Queue, Table y Disk para los pedidos de importación. En el caso de los pedidos de exportación, Data Box no admite los tipos de almacenamiento Queue, Table, Disk y Azure Data Lake Gen 2 para las cuentas de uso general.
> - Data Box no admite blobs en anexos en las cuentas de Blob Storage y de almacenamiento de blobs en bloque.
> - La compatibilidad con el protocolo Network File System (NFS) 3.0 en Azure Blob Storage no se admite con Data Box.
> - Los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.
> - Se pueden exportar hasta 80 TB.
> - El historial de archivos y las instantáneas de blobs no se exportan.