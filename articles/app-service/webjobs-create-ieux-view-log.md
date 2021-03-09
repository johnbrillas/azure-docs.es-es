---
title: Visualización del historial de registros de WebJobs
description: Vea el historial de registros de los trabajos correctos y los que han generado errores.
author: ggailey777
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4b4e421458438aecc7c08e397f1fc919a8cc2225
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743839"
---
# <a name="view-webjob-history-in-the-azure-portal"></a>Visualización del historial de WebJob en Azure Portal

Vea el historial de registros de los trabajos correctos y los que han generado errores.

1. Seleccione el WebJob cuyo historial desea ver y, luego, seleccione el botón **Registros**.

    ![Botón Registros](./media/web-sites-create-web-jobs/wjbladelogslink.png)

1. En la página **Detalles del WebJob**, seleccione una hora para ver los detalles de una ejecución.

    ![Detalles del WebJob](./media/web-sites-create-web-jobs/webjobdetails.png)

1. En la página **Detalles de ejecución del WebJob**, seleccione **Alternar salida** para ver el texto del contenido del registro.

    ![Detalles de ejecución del trabajo web](./media/web-sites-create-web-jobs/webjobrundetails.png)

    Para ver el texto de salida en otra ventana del explorador, seleccione **Descargar**. Para descargar el texto propiamente tal, haga clic con el botón derecho en **Descargar** y use las opciones del explorador para guardar el contenido del archivo.

1. Seleccione el vínculo de la ruta de navegación de los **WebJobs** que se encuentra en la parte superior de la página para ir a una lista de los WebJobs.

    ![Ruta de navegación de WebJob](./media/web-sites-create-web-jobs/breadcrumb.png)

    ![Lista de WebJobs el panel del historial](./media/web-sites-create-web-jobs/webjobslist.png)

## <a name="next-steps"></a>Pasos siguientes

* Uso del [SDK de WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki) para simplificar muchas tareas de programación

* [Desarrollo e implementación de WebJobs mediante Visual Studio](webjobs-dotnet-deploy-vs.md)