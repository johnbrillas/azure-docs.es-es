---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8bdd8b9d900cc50fdeb34ff7d233ac4d7e17a45c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "78191082"
---
Actualice *HttpExample\\\_\_init\_\_.py* para que se ajuste al siguiente código y agregue el parámetro `msg` a la definición de la función y `msg.set(name)` en la instrucción `if name:`.

:::code language="python" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/__init__.py":::

El parámetro `msg` es una instancia de [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest). Su método `set` escribe un mensaje de cadena en la cola; en este caso, el nombre que se pasa a la función en la cadena de consulta de la dirección URL.
