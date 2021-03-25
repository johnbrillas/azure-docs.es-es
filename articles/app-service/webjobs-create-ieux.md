---
title: Ejecución de tareas en segundo plano con WebJobs
description: Aprenda a usar WebJobs para ejecutar tareas en segundo plano en Azure App Service. Elija entre diversos formatos de script y ejecútelos con expresiones CRON.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ms.reviewer: msangapu;suwatch;pbatum;naren.soni
ms.custom: seodec18
zone_pivot_groups: app-service-webjob
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0d49323e2bc3c0522b1fb9ad49ffcc14f476e2dc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452805"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Ejecución de tareas en segundo plano con WebJobs en Azure App Service

El concepto de ejecutar [tareas en segundo plano](./webjobs-create-ieux-conceptual.md) en Azure está ligado a los trabajos web de Azure App Service. Obtenga más información sobre cómo implementar <abbr title="Un programa o script en la misma instancia que una aplicación web, API o móvil.">Trabajos web</abbr> mediante [Azure Portal](https://portal.azure.com) para cargar un archivo ejecutable o script. 

Tres tipos de WebJobs compatibles son:

* **Continuous**: se inicia inmediatamente y se suele ejecutar en un bucle sin fin.
* **Scheduled**: se inicia a partir de un desencadenador programado.
* **Manual**: se inicia a partir de un desencadenar manual.

::: zone pivot="webjob-type-continuous"

[!INCLUDE [Continuous](./includes/webjobs-create-ieux-continuous.md)]

::: zone-end

::: zone pivot="webjob-type-scheduled"

[!INCLUDE [Scheduled](./includes/webjobs-create-ieux-scheduled.md)]

::: zone-end

::: zone pivot="webjob-type-manual"

[!INCLUDE [Manual](./includes/webjobs-create-ieux-manual.md)]

::: zone-end
   
## <a name="next-steps"></a><a name="NextSteps"></a> Pasos siguientes

* [Más información sobre las tareas en segundo plano como WebJobs](./webjobs-create-ieux-conceptual.md)
* [Visualización del historial de registros de WebJobs](./webjobs-create-ieux-view-log.md)

* Uso del [SDK de WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki) para simplificar muchas tareas de programación

* [Desarrollo e implementación de WebJobs mediante Visual Studio](webjobs-dotnet-deploy-vs.md)
