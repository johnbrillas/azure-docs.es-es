---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ef0aa8ba1983ca30fd44c27fe570b6b5f51733a5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743838"
---
## <a name="create-a-scheduled-webjob"></a><a name="CreateScheduledCRON"></a> Creación de un WebJob programado


1. Vaya a [Azure Portal](https://portal.azure.com).
1. Vaya a la instancia de **App Service** de su <abbr title="El recurso de aplicación puede ser una aplicación web, una aplicación de API o una aplicación móvil.">recurso de aplicación</abbr>.
1. Seleccione **WebJobs**.

   ![Selección de WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

1. En la página **WebJobs**, seleccione **Agregar**.

    ![Página WebJob](../media/web-sites-create-web-jobs/wjblade.png)

1. Use la configuración **Agregar WebJob** que se especifica en la tabla.

    ![Página Agregar WebJob](../media/web-sites-create-web-jobs/addwjscheduled.png)
    
    | Configuración      | Valor de ejemplo   |
    | ------------ | ----------------- | 
    | <abbr title="Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales, salvo por `-` y `_`.">Nombre</a> | myScheduledWebJob |  |
    | <abbr title="Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script.">Carga de archivos</abbr> | ConsoleApp.zip |
    | <abbr title="Entre los tipos, se incluyen "continuo" y "desencadenado".">Tipo</abbr> | Desencadenado |
    | <abbr title="Para que la programación funcione de manera confiable, habilite la característica AlwaysOn. AlwaysOn solo está disponible en los planes de tarifa Básico, Estándar y Premium.">Desencadenadores</a> | Programado |
    | Expresión CRON</a> | 0 0/20 * * * * | 
    
    <br>
    
    <details>
     <summary>Más información sobre las expresiones CRON</summary>
     <a name="#ncrontab-expressions"></a>
    
     Puede especificar una [expresión NCRONTAB](../../azure-functions/functions-bindings-timer.md#ncrontab-expressions) en el portal o incluir un archivo `settings.job` en la raíz del archivo *.zip* de WebJob, como en el ejemplo siguiente:
     
     ```json
     {
         "schedule": "0 */15 * * * *"
     }
     ```
     
     Para más información, consulte [Programar un trabajo Web desencadenado](../webjobs-dotnet-deploy-vs.md#scheduling-a-triggered-webjob).
     
     [!INCLUDE [webjobs-cron-timezone-note](../../../includes/webjobs-cron-timezone-note.md)]
     </details>
     <br>

1. Haga clic en **OK**.

    El WebJob nuevo aparece en la página **WebJobs**.
    
    ![Lista de WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)
