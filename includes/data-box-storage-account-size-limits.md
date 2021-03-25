---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 01/12/2021
ms.author: alkohli
ms.openlocfilehash: 6cabac4c59b09d146c1e42762402043622edb60e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98225437"
---
Estos son los límites del tamaño de los datos que se copian en una cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para obtener la información más actualizada sobre estos límites, consulte [Objetivos de rendimiento y escalabilidad de Blob Storage](../articles/storage/blobs/scalability-targets.md) y [Objetivos de rendimiento y escalabilidad de Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blob en páginas                                            | El límite máximo es el mismo que el [límite de almacenamiento definido para la suscripción de Azure](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) e incluye datos de todos los orígenes, incluido Data Box.   |
| Azure Files                                                          | Data Box admite recursos compartidos de archivos de gran tamaño (100 TiB) si se habilita antes de la creación del pedido de Data Box. <br> Si no se habilita antes de la creación del pedido, se admitirá un tamaño máximo de recurso compartido de archivos de 5 TiB. <br> Data Box admite recursos compartidos de archivos Premium de Azure, que permiten un total de 100 TiB para todos los recursos compartidos de la cuenta de almacenamiento. <br> La capacidad máxima utilizable es ligeramente inferior debido al espacio que usan los registros de copia y de auditoría. Se reserva un mínimo de 100 GiB para cada uno de ellos. Para obtener más información, consulte [Registros de auditoría para Azure Data Box y Azure Data Box Heavy](../articles/databox/data-box-audit-logs.md). <br> Todas las carpetas de *StorageAccount_AzureFiles* deben seguir este límite. <br> Para más información, consulte [Habilitación y creación de recursos compartidos de archivos grandes](../articles/storage/files/storage-files-how-to-create-large-file-share.md).      |
