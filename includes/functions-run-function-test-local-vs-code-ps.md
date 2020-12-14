---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: a5c113849296275432acf1f5603377a1909a2c04
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96842285"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Azure Functions Core Tools se integra con Visual Studio Code para permitirle ejecutar y depurar un proyecto de Azure Functions localmente. Para más información sobre cómo depurar en Visual Studio Code, consulte [Depuración local de Azure Functions de PowerShell](../articles/azure-functions/functions-debug-powershell-local.md). 
1. Para llamar a la función, presione <kbd>F5</kbd> para iniciar el proyecto de aplicación de funciones. La salida de Core Tools aparece en el panel **Terminal**. Si tiene problemas para ejecutarlo en Windows, asegúrese de que el terminal predeterminado de Visual Studio Code no esté establecido en **WSL Bash**.

1. En el panel **Terminal**, copie el punto de conexión de la dirección URL de la función desencadenada por HTTP.

    ![Salida local de Azure](./media/functions-run-function-test-local-vs-code-ps/functions-vscode-f5.png)

1. Anexe la cadena de consulta `?name=<yourname>` a esta dirección URL y use después `Invoke-RestMethod` en un segundo símbolo del sistema de PowerShell para ejecutar la solicitud, del modo siguiente:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    También puede ejecutar la solicitud GET de un explorador desde la siguiente dirección URL.

    `http://localhost:7071/api/HttpExample?name=PowerShell`

    Cuando se llama al punto de conexión HttpTrigger sin pasar un parámetro `name` como un parámetro de consulta o en el cuerpo, la función devuelve un error `BadRequest`. Al revisar el código de run.ps1, verá que este error se produce por diseño.

1. La información sobre la solicitud se muestra en el panel **Terminal**.

    ![Ejecución de función en el panel Terminal](./media/functions-run-function-test-local-vs-code-ps/function-execution-terminal.png)

1. Cuando haya terminado, presione **Ctrl + C** para detener Core Tools.

Después de comprobar que la función se ejecuta correctamente en el equipo local es el momento de publicar el proyecto en Azure.
