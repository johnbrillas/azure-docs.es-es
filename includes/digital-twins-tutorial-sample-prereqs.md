---
author: baanders
description: 'Archivo de inclusión para los tutoriales de Azure Digital Twins: requisitos previos del proyecto de ejemplo'
ms.service: digital-twins
ms.topic: include
ms.date: 1/20/2021
ms.author: baanders
ms.openlocfilehash: 43cc3dfc5b425df6d9dd5e2c2f35a792907ccdea
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103622116"
---
## <a name="prerequisites"></a>Requisitos previos

Para realizar los pasos de este tutorial, debe completar primero los siguientes requisitos previos. 

Si no tiene una suscripción a Azure, **cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)** antes de empezar.

### <a name="get-required-resources"></a>Obtención de los recursos necesarios

Para realizar este tutorial, **instale [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), versión 16.5 o posterior** en la máquina de desarrollo. Si ya tiene instalada una versión anterior, puede abrir la aplicación *Instalador de Visual Studio* en la máquina y seguir las indicaciones para actualizar la instalación.

>[!NOTE]
> Asegúrese de que la instalación de Visual Studio 2019 incluye la **[carga de trabajo de desarrollo de Azure](/dotnet/azure/configure-visual-studio)** . Esta carga de trabajo permite a una aplicación publicar Azure Functions y realizar otras tareas de desarrollo de Azure.

El tutorial utiliza un proyecto de ejemplo escrito en C#. El ejemplo se encuentra aquí: [Ejemplos de Azure Digital Twins de un extremo a otro](/samples/azure-samples/digital-twins-samples/digital-twins-samples). Para **obtener el proyecto de ejemplo** en la máquina, vaya al vínculo del ejemplo y seleccione el botón *Browse code* (Examinar código) situado debajo del título. Esto le llevará al repositorio de GitHub para los ejemplos, que puede descargar como *.ZIP* al seleccionar el botón *Código* y *Descargar archivo ZIP*.

:::image type="content" source="../articles/digital-twins/media/includes/download-repo-zip.png" alt-text="Vista del repositorio digital-gemelos-samples en GitHub. El botón Código está seleccionado, lo que genera un pequeño cuadro de diálogo en el que el botón Descargar archivo ZIP está resaltado." lightbox="../articles/digital-twins/media/includes/download-repo-zip.png":::

Se descargará una carpeta *.ZIP* en la máquina denominada **digital-twins-samples-master.zip**. Descomprima la carpeta y extraiga los archivos.

### <a name="prepare-an-azure-digital-twins-instance"></a>Preparación de una instancia de Azure Digital Twins

[!INCLUDE [Azure Digital Twins: instance prereq](digital-twins-prereq-instance.md)]
