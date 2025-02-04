---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/12/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 93bfb515c46413400ecd47105de378c7b677cecd
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102564972"
---
## <a name="prepare-your-repository"></a>Preparación del repositorio

Para obtener compilaciones automáticas del servidor de compilación de Azure App Service, asegúrese de que la raíz del repositorio tenga los archivos correctos del proyecto.

| Tiempo de ejecución | Archivos del directorio raíz |
|-|-|
| ASP.NET (solo Windows) | _*.sln_, _*.csproj_ o _default.aspx_ |
| ASP.NET Core | _*.sln_ o _*.csproj_ |
| PHP | _index.php_ |
| Ruby (solo Linux) | _Gemfile_ |
| Node.js | _server.js_, _app.js_ o _package.json_ con un script de inicio |
| Python | _\*.py_, _requirements.txt_ o _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_ o _iisstart.htm_ |
| Trabajos web | _\<job_name>/run.\<extension>_ en _App\_Data/jobs/continuous_ para WebJobs continuos, o _App\_Data/jobs/triggered_ para WebJobs desencadenados. Para más información, consulte la [documentación de WebJobs de Kudu](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| Functions | Consulte [Implementación continua para Azure Functions](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Para personalizar la implementación, puede incluir un archivo *.deployment* en la raíz del repositorio. Para más información, consulte el artículo sobre la [personalización de las implementaciones](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) y el artículo sobre el [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Si desarrolla en Visual Studio, deje que [Visual Studio cree un repositorio en su lugar](/azure/devops/repos/git/creatingrepo?tabs=visual-studio). El proyecto está listo inmediatamente para su implementación mediante GIT.
>

