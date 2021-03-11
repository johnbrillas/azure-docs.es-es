---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 03/02/2021
ms.author: crtreasu
ms.openlocfilehash: d8dfc3d4b7a8447250481b98c1adadc865a29da1
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102532713"
---
### <a name="upload-your-model"></a>Carga del modelo

Si aún no tiene un modelo de Object Anchors, siga las instrucciones de [Creación de un modelo](/azure/object-anchors/quickstarts/get-started-model-conversion) para crear uno. A continuación, vuelva aquí.

Con el dispositivo HoloLens conectado al Portal de dispositivos Windows, siga estos pasos para cargar un modelo y que lo use la aplicación:

1. En el Portal de dispositivos Windows, vaya a **Sistema > Explorador de archivos > LocalAppData**. Allí, debería aparecer la aplicación en la lista de aplicaciones.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localappdata.png" alt-text="Explorador de archivos":::

2. Abra la aplicación y haga clic en la carpeta `LocalState`.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localstate.png" alt-text="Abra la carpeta LocalState":::

3. Cargue el archivo de modelo en la carpeta `LocalState`.

    :::image type="content" source="./media/object-anchors-quickstarts/portal-upload-model.png" alt-text="Cargue el modelo en el portal":::

    Vuelva a iniciar la aplicación desde el dispositivo HoloLens. Ahora puede detectar objetos que coincidan con el modelo.
