---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5687fb99c27b8b2141e0a2a817327cfbb124951a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102109181"
---
## <a name="create-a-manually-triggered-webjob"></a><a name="CreateOnDemand"></a> Creación de un WebJob desencadenado de manera manual

1. Vaya a [Azure Portal](https://portal.azure.com).
1. Vaya a la instancia de **App Service** de su <abbr title="El recurso de aplicación puede ser una aplicación web, una aplicación de API o una aplicación móvil.">recurso de aplicación</abbr>.
1. Seleccione **WebJobs**.

    ![Selección de WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

2. En la página **WebJobs**, seleccione **Agregar**.

   ![Página WebJob](../media/web-sites-create-web-jobs/wjblade.png)

3. Use la configuración **Agregar WebJob** que se especifica en la tabla.

    ![Captura de pantalla que muestra la configuración que debe establecerse para crear un WebJob desencadenado manualmente.](../media/web-sites-create-web-jobs/addwjtriggered.png)
    
    | Configuración      | Valor de ejemplo   | 
    | ------------ | ----------------- | 
   | <abbr title="Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales, salvo por `-` y `_`.">Nombre</abbr> | myTriggeredWebJob | 
    | <abbr title="Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script.">Carga de archivos</abbr> | ConsoleApp.zip |
    | <abbr title="Entre los tipos, se incluyen "continuo" y "desencadenado".">Tipo</abbr> | Desencadenado | 
    | <abbr title="Entre los tipos, se incluyen "manual" o "programado".">Desencadenadores</a> | Manual | |

4. Haga clic en **OK**. 

   El WebJob nuevo aparece en la página **WebJobs**.

   ![Lista de WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)

7. **Para ejecutar un WebJob**, seleccione su nombre en la lista y haga clic en **Ejecutar**.
   
    ![Ejecución de WebJobs](../media/web-sites-create-web-jobs/runondemand.png)

