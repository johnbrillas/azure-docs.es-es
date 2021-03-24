---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2021
ms.author: glenga
ms.openlocfilehash: 5bc1d2ddb5b9ddb185a8349fb42d5f5db79c1498
ms.sourcegitcommit: 5f32f03eeb892bf0d023b23bd709e642d1812696
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2021
ms.locfileid: "103201373"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Visual Studio Code se integra con [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) para que pueda ejecutar este proyecto en un equipo de desarrollo local antes de publicarlo en Azure.

1. Para llamar a la función, presione <kbd>F5</kbd> para iniciar el proyecto de aplicación de funciones. La salida de Core Tools aparece en el panel **Terminal**. La aplicación se inicia en el panel **Terminal**. Puede ver el punto de conexión de la dirección URL de la función desencadenada por HTTP que se ejecuta localmente.

    ![Salida de la función local de VS Code](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

    Si tiene problemas para ejecutarlo en Windows, asegúrese de que el terminal predeterminado de Visual Studio Code no esté establecido en **WSL Bash**.

1. Mientras se ejecuta Core Tools, vaya al área de **Azure: Área Functions**. En **Functions**, expanda **Proyecto local** > **Functions**. Haga clic con el botón derecho (Windows) o <kbd>Ctrl</kbd> + clic en (macOS) en la función `HttpExample` y elija **Execute Function Now...** (Ejecutar la función ahora...).

    :::image type="content" source="media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="Ejecutar la función ahora desde Visual Studio Code":::
    
1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`. Presione Entrar para enviar este mensaje de solicitud a la función. 

   En su lugar, podría haber enviado una solicitud HTTP GET a la dirección <http://localhost:7071/api/HttpExample> en un explorador web.

1. Cuando la función se ejecuta localmente y devuelve una respuesta, se genera una notificación en Visual Studio Code. La información sobre la ejecución de la función se muestra en el panel **Terminal**.

1. Presione <kbd>Ctrl + C</kbd> para detener Core Tools y desconectar el depurador.
