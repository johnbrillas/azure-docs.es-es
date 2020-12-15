---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 11d426016cfe1a8a9ff843da518f57c08881be5d
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96842274"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

1. Para ejecutar una función, inicie el host en tiempo de ejecución local de Azure Functions desde la carpeta *LocalFunctionProj*:

    ```
    func start
    ```

    Hacia el final de la salida, deberían aparecer las líneas siguientes: 
    
    <pre>
    ...
    
    Now listening on: http://0.0.0.0:7071
    Application started. Press Ctrl+C to shut down.
    
    Http Functions:
    
            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ...
    
    </pre>
    
    >[!NOTE]  
    > Si HttpExample no aparece como se ha mostrado arriba, es probable que haya iniciado el host desde fuera de la carpeta raíz del proyecto. En ese caso, use **Ctrl**+**C** para detener el host, vaya a la carpeta raíz del proyecto y vuelva a ejecutar el comando anterior.

1. Copie la dirección URL de la función `HttpExample` de esta salida en un explorador y anexe la cadena de consulta `?name=<YOUR_NAME>`, lo que hará que la dirección URL completa sea `http://localhost:7071/api/HttpExample?name=Functions`. El explorador debe mostrar un mensaje como este `Hello Functions`:

    ![Resultado de la ejecución de la función localmente en el explorador](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

1. El terminal en el que inició el proyecto también muestra la salida del registro cuando realiza solicitudes.

1. Cuando termine, presione **Ctrl**+**C** y seleccione `y` para detener el host de Functions.
