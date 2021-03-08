---
title: Información general de Azure Object Anchors
description: Obtenga información sobre cómo Azure Object Anchors lo ayuda a detectar objetos en el mundo físico.
author: craigktreasure
manager: vriveras
ms.author: crtreasu
ms.date: 02/18/2021
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: 099307ba1085ff6d24bc6bb4000a592aabc8f8f6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747959"
---
# <a name="azure-object-anchors-overview"></a>Información general de Azure Object Anchors

Azure Object Anchors permite a una aplicación detectar un objeto en el mundo físico mediante un modelo 3D y estimar su posición 6-DoF (seis grados de libertad). La posición 6-DoF (seis grados de libertad) se define como una rotación y traslación entre un modelo 3D y su contraparte física, el objeto real. 

Azure Object Anchors consta de un servicio administrado para la conversión de modelos y un SDK de cliente de entorno de ejecución para HoloLens. El servicio acepta un modelo de objetos 3D y genera un modelo de Azure Object Anchors. El modelo de Azure Object Anchors se usa junto con el SDK de entorno de ejecución para permitir que una aplicación de HoloLens cargue un modelo de objetos y detecte y haga el seguimiento de las instancias de ese modelo en el mundo físico.

:::image type="content" source="./media/aoa-overview.jpg" alt-text="Azure Object Anchors en acción":::

## <a name="examples"></a>Ejemplos

Entre los casos de uso de ejemplos habilitados por Azure Object Anchors se incluyen los siguientes:

- **Entrenamiento**. Cree experiencias de aprendizaje de realidad mixta para sus trabajadores, sin necesidad de colocar marcadores o dedicar tiempo para ajustar manualmente la alineación de los hologramas. Si quiere aumentar las experiencias de aprendizaje de realidad mixta con detección y seguimiento automáticos, ingiera su modelo en el servicio de Object Anchors y estará un paso más cerca de disfrutar una experiencia sin marcadores.

- **Guía para las tareas**. Guiar a los empleados por un conjunto de tareas puede ser mucho más sencillo con Mixed Reality. La superposición de instrucciones y procedimientos recomendados digitales aplicada al objeto físico, que puede ser una pieza de maquinaria en una planta de producción o una cafetera en la cocina del equipo, puede disminuir considerablemente la dificultad de completar el conjunto de tareas. Por lo general, desencadenar estas experiencias requiere algún tipo de marcador o alineación manual, pero con Object Anchors, puede crear una experiencia que detecta automáticamente el objeto relacionado con la tarea a mano. Luego, siga sin problemas por las directrices de Mixed Reality sin marcadores ni alineación manual.

- **Búsqueda de recursos**. Si ya tiene un modelo 3D de algún objeto en el espacio físico, Object Anchors puede permitirle localizar instancias de ese objeto en el entorno físico y hacer un seguimiento de las mismas.

## <a name="next-steps"></a>Pasos siguientes

En las secciones siguientes se proporciona información sobre cómo comenzar a usar y compilar aplicaciones con Azure Object Anchors.

> [!div class="nextstepaction"]
> [Ingesta de modelos](quickstarts/get-started-model-ingestion.md)

> [!div class="nextstepaction"]
> [HoloLens en Unity](quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [HoloLens para Unity con MRTK](quickstarts/get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [HoloLens DirectX](quickstarts/get-started-hololens-directx.md)
