---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 33dc766643355a5f5ebb6138e000595fd1bfe6fc
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743834"
---
## <a name="create-a-continuous-webjob"></a><a name="CreateContinuous"></a> Creación de un WebJob continuo

1. Vaya a [Azure Portal](https://portal.azure.com).
1. Vaya a la instancia de **App Service** de su <abbr title="El recurso de aplicación puede ser una aplicación web, una aplicación de API o una aplicación móvil.">recurso de aplicación</abbr>.
1. Seleccione **WebJobs**.

   ![Selección de WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

1. En la página **WebJobs**, seleccione **Agregar**.

    ![Página WebJob](../media/web-sites-create-web-jobs/wjblade.png)

1. Use la configuración **Agregar WebJob** que se especifica en la tabla.

   ![Captura de pantalla que muestra la configuración de Agregar WebJob que necesita establecer.](../media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Configuración      | Valor de ejemplo   | 
   | ------------ | ----------------- | 
   | <abbr title="Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales, salvo por `-` y `_`.">Nombre</abbr> | myContinuousWebJob | 
   | <abbr title=" Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script.">Carga de archivos</abbr> | ConsoleApp.zip |
   | <abbr title="Entre los tipos, se incluyen "continuo" y "desencadenado".">Tipo</abbr> | Continuo | 
   | <abbr title="Solo disponible para WebJobs continuos. Determina si el programa o el script se ejecuta en todas las instancias o solo en una. La opción para que se ejecute en varias instancias no se aplica a los planes de tarifa Gratis o Compartido.">Escala</abbr> | Instancias múltiples | 

1. Haga clic en **OK**.

    El WebJob nuevo aparece en la página **WebJobs**.

    ![Lista de WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)

1. **Para detener o reiniciar** un WebJob continuo, haga clic con el botón derecho en el WebJob de la lista y, a continuación, en **Detener** o **Iniciar**.

   ![Detención de un WebJob continuo](../media/web-sites-create-web-jobs/continuousstop.png)
