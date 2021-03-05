---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 43c8c578587c65b6e0317caf77ff2d0604cb4fa3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "102044673"
---
### <a name="build-and-deploy-the-app"></a>Compilación e implementación de la aplicación

Abra el archivo `.sln` generado por Unity. Cambie la configuración de compilación a la siguiente.

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-build-config.png" alt-text="Configuración de compilación":::

A continuación, tendrá que configurar la **dirección IP de la máquina remota** para implementar y depurar la aplicación.

Haga clic con el botón derecho en el proyecto de la aplicación y seleccione **Propiedades**. En la página de propiedades, seleccione **Propiedades de configuración-> Depuración**. Cambie el valor de **Nombre de la máquina** a la dirección IP del dispositivo HoloLens y haga clic en **Aplicar**.

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-remote-debug.png" alt-text="depuración remota":::

Cierre la página de propiedades. Haga clic en **Máquina remota**. La aplicación debe empezar a compilarse e implementarse en el dispositivo remoto. Asegúrese de que el dispositivo está activo.
