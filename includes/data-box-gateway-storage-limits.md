---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: 624df1185f86004b48bcca921e76d55bfb14c48d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98901185"
---
En esta sección se describen los límites del servicio de Azure Storage y las convenciones de nomenclatura necesarias para Azure Files, blobs en bloques de Azure y blobs en páginas de Azure, según corresponda para el servicio Azure Stack Edge o Data Box Gateway. Revise cuidadosamente los límites de almacenamiento y siga todas las recomendaciones.

Para conocer la información más reciente sobre los límites del servicio de almacenamiento de Azure y los procedimientos recomendados para asignar nombres a recursos compartidos, contenedores y archivos, vaya a:

- [Nomenclatura y referencia de contenedores](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Nomenclatura y referencia de recursos compartidos](/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Convenciones de blobs en bloques y blobs en páginas](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Si hay archivos o directorios que superen los límites del servicio Azure Storage, o no cumplen las convenciones de nomenclatura de archivos y blobs de Azure, estos archivos o directorios no se ingieren en Azure Storage mediante el servicio Azure Stack Edge o Data Box Gateway.