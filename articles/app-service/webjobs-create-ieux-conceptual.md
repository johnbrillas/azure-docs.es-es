---
title: WebJob, tareas en segundo plano, en Azure
description: Obtenga información sobre WebJobs.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7714b090399b0b184e2e216ff6da7b10f2bf4386
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452278"
---
# <a name="webjobs-run-background-tasks-in-azure-app-service"></a>Ejecución de tareas en segundo plano mediante WebJobs en Azure App Service

En este artículo se muestra cómo implementar WebJobs con [Azure Portal](https://portal.azure.com) para cargar un ejecutable o un script. Para información sobre cómo desarrollar e implementar WebJobs mediante Visual Studio, consulte [Implementación de WebJobs con Visual Studio](webjobs-dotnet-deploy-vs.md).

## <a name="overview"></a>Información general
WebJobs es una característica de [Azure App Service](index.yml) que le permite ejecutar un programa o script en la misma instancia que una aplicación web, una aplicación de API o una aplicación móvil. No hay ningún coste adicional en el uso de trabajos web.

> [!IMPORTANT]
> WebJobs todavía no es compatible con App Service en Linux.

El SDK de Azure WebJobs se puede usar con WebJobs para simplificar muchas tareas de programación. Para obtener más información, consulte [¿Qué es el SDK de Webjobs?](https://github.com/Azure/azure-webjobs-sdk/wiki)

Azure Functions proporciona otra manera de ejecutar programas y scripts. Para una comparación entre WebJobs y Functions, consulte [Elección entre Flow, Logic Apps, Functions y WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>Tipos de WebJob

En la tabla siguiente se describen las diferencias entre los WebJobs *continuo* y *desencadenado*.


|Continuo  |Desencadenado  |
|---------|---------|
| Se inicia inmediatamente cuando se crea el WebJob. Para evitar que el trabajo finalice, el programa o script habitualmente funciona dentro de un bucle sin fin. Si el trabajo finaliza, es posible reiniciarlo. | Se inicia solo cuando se desencadena manualmente o de acuerdo con una programación. |
| Se ejecuta en todas las instancias en que se ejecuta la aplicación web. También puede restringir el WebJob a una sola instancia. |Se ejecuta en una sola instancia que Azure selecciona para el equilibrio de carga.|
| Admite la depuración remota. | No admite la depuración remota.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="add-webjob-to-source-control"></a>Incorporación de WebJob al control de código fuente

Si tiene el control de código fuente configurado con la aplicación, los trabajos web se deben implementar como parte de la integración del control de código fuente. Una vez configurado el control de código fuente con la aplicación, no se puede agregar un trabajo web desde Azure Portal.

## <a name="supported-file-types-for-scripts-or-programs"></a><a name="acceptablefiles"></a>Tipos de archivo admitidos para scripts o programas

Se admiten los tipos de archivo siguientes:

* .cmd, .bat, .exe (con cmd de Windows)
* .ps1 (con PowerShell)
* .sh (con Bash)
* .php (con PHP)
* .py (con Python)
* .js (con Node.js)
* .jar (con Java)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear un WebJob](./webjobs-create-ieux.md)
* Visualización del historial de registros de [WebJobs](./webjobs-create-ieux-view-log.md)
