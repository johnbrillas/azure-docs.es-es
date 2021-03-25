---
title: Evento de inicio de eliminación de un grupo de Azure Batch
description: Referencia del evento de inicio de eliminación de grupo de Batch. Este evento se genera cuando se inicia una operación de eliminación del grupo.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: 86f6eb8e7b269cb45f692398e9e60390375ca073
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97803755"
---
# <a name="pool-delete-start-event"></a>Evento de inicio de eliminación del grupo

 Este evento se genera cuando se inicia una operación de eliminación del grupo. Puesto que la eliminación de grupo es un evento asincrónico, puede esperar que se genere un evento completo de eliminación del grupo cuando se haya completado la operación de eliminación.

 En el siguiente ejemplo, se muestra el cuerpo de un evento de inicio de eliminación del grupo.

```
{
   "id": "myPool1"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|String|El identificador del grupo.|
