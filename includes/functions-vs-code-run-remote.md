---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2021
ms.author: glenga
ms.openlocfilehash: 121b10cc568cce089c90e66b9c6f292c74f4acbe
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809576"
---
## <a name="run-the-function-in-azure"></a>Ejecución de la función en Azure

1. Vuelva al área **Azure: Functions** en la barra lateral y expanda su suscripción, la nueva aplicación de funciones y **Functions**. Haga clic con el botón derecho (Windows) o <kbd>Ctrl</kbd> + clic en (macOS) en la función `HttpExample` y elija **Execute Function Now...** (Ejecutar la función ahora...).

    :::image type="content" source="media/functions-vs-code-run-remote/execute-function-now.png" alt-text="Ejecutar la función ahora en Azure desde Visual Studio Code":::

1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`. Presione Entrar para enviar este mensaje de solicitud a la función.  

1. Cuando la función se ejecuta en Azure y devuelve una respuesta, se genera una notificación en Visual Studio Code.
