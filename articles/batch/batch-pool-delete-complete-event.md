---
title: Evento completo de eliminación de grupo de Azure Batch
description: Referencia del evento completo de eliminación de grupo de Batch. Este evento se genera cuando se finaliza una operación de eliminación del grupo.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: be6411a150ae6be424c0621eed768157154c7408
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97803738"
---
# <a name="pool-delete-complete-event"></a>Evento de finalización de eliminación del grupo

 Este evento se genera cuando se finaliza una operación de eliminación del grupo.

 En el siguiente ejemplo, se muestra el cuerpo de un evento de finalización de eliminación del grupo.

```
{
   "id": "myPool1",
   "startTime": "2016-09-09T22:13:48.579Z",
   "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|String|El identificador del grupo.|
|`startTime`|DateTime|La hora en que se inició la eliminación del grupo.|
|`endTime`|DateTime|La hora en que finalizó la eliminación del grupo.|

## <a name="remarks"></a>Comentarios

Para más información sobre los estados y códigos de error para la operación de cambio de tamaño del grupo, consulte [Eliminar un grupo de una cuenta](/rest/api/batchservice/delete-a-pool-from-an-account).
