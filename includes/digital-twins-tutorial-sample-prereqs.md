---
author: baanders
description: 'Archivo de inclusión para los tutoriales de Azure Digital Twins: requisitos previos del proyecto de ejemplo'
ms.service: digital-twins
ms.topic: include
ms.date: 1/20/2021
ms.author: baanders
ms.openlocfilehash: 5a1baf9631f2d30dd14ff16d2d34beda04605c6c
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98660528"
---
## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, **cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)** antes de empezar.

Asimismo, antes de empezar, **instale [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), versión 16.5 o posterior** en la máquina de desarrollo. Si ya tiene instalada una versión anterior, puede abrir la aplicación *Instalador de Visual Studio* en la máquina y seguir las indicaciones para actualizar la instalación.

El tutorial utiliza un proyecto de ejemplo escrito en C#. El ejemplo se encuentra aquí: [Ejemplos de Azure Digital Twins de un extremo a otro](/samples/azure-samples/digital-twins-samples/digital-twins-samples). Para **obtener el proyecto de ejemplo** en la máquina, vaya al vínculo del ejemplo y seleccione el botón *Browse code* (Examinar código) situado debajo del título. Esto le llevará al repositorio de GitHub para los ejemplos, que puede descargar como *.ZIP* al seleccionar el botón *Código* y *Descargar archivo ZIP*.

:::image type="content" source="../articles/digital-twins/media/includes/download-repo-zip.png" alt-text="Vista del repositorio digital-twins-samples en GitHub. El botón Código está seleccionado, lo que genera un pequeño cuadro de diálogo en el que el botón Descargar archivo ZIP está resaltado." lightbox="../articles/digital-twins/media/includes/download-repo-zip.png":::

Se descargará una carpeta *.ZIP* en la máquina denominada **digital-twins-samples-master.zip**. Descomprima la carpeta y extraiga los archivos.

### <a name="prepare-an-azure-digital-twins-instance"></a>Preparación de una instancia de Azure Digital Twins

[!INCLUDE [Azure Digital Twins: instance prereq](digital-twins-prereq-instance.md)]

[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](digital-twins-local-credentials-outer.md)]
