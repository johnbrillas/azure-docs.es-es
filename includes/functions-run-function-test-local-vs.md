---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: bff2f05a95faf9c475189cb5a8003cb7fd9f69be
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101701407"
---
1. Para ejecutar la función, presione <kbd>F5</kbd> en Visual Studio. Es preciso habilitar una excepción de firewall para que las herramientas para controlen las solicitudes de HTTP. Los niveles de autorización nunca se aplican cuando se ejecuta una función localmente.

2. Copie la dirección URL de la función de los resultados del runtime de Azure Functions.

    ![Runtime local de Azure](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<YOUR_NAME>` a esta dirección URL y ejecute la solicitud. La siguiente imagen muestra la respuesta en el explorador para la solicitud GET local devuelta por la función: 

    ![Respuesta de localhost de la función en el explorador](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Para detener la depuración, presione <kbd>Mayús</kbd>+<kbd>F5</kbd> en Visual Studio.
