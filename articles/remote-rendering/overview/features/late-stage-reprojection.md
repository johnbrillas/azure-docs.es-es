---
title: Reproyección de fase tardía
description: Información sobre la reproyección de fase tardía y cómo se usa.
author: sebastianpick
ms.author: sepick
ms.date: 02/04/2020
ms.topic: article
ms.openlocfilehash: f0951415bba22a226dadb7f2a115cede451399bc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92205649"
---
# <a name="late-stage-reprojection"></a>Reproyección de fase tardía

La *reproyección de fase tardía* (LSR) es una característica de hardware que ayuda a estabilizar los hologramas cuando el usuario se mueve.

Se espera que los modelos estáticos mantengan visualmente su posición al desplazarse por ellos. Si parece que son inestables, se puede sugerir este comportamiento en los problemas de LSR. Tenga en cuenta que las transformaciones dinámicas adicionales, como las animaciones o las vistas de explosión, pueden enmascarar este comportamiento.

Puede elegir entre dos modos de LSR diferentes, en concreto, **LSR planar** o **LSR de profundidad**. El que se active dependerá de si la aplicación cliente envía un búfer de profundidad.

Ambos modos de LSR mejoran la estabilidad del holograma, aunque tienen limitaciones diferentes. Empiece por probar el LSR de profundidad, ya que es probable que se obtengan mejores resultados en la mayoría de los casos.

## <a name="choose-lsr-mode-in-unity"></a>Elección del modo de LSR en Unity

En el Editor de Unity, vaya a *:::no-loc text="File > Build Settings":::* . Seleccione *:::no-loc text="Player Settings":::* en la parte inferior izquierda y, a continuación, compruebe en *:::no-loc text="Player > XR Settings > Virtual Reality SDKs > Windows Mixed Reality":::* si la opción **:::no-loc text="Enable Depth Buffer Sharing":::** está activada:

![Marca del uso compartido del búfer de profundidad habilitado](./media/unity-depth-buffer-sharing-enabled.png)

Si es así, la aplicación usará LSR de profundidad, de lo contrario, usará LSR planar.

## <a name="depth-lsr"></a>LSR de profundidad

Para que el LSR de profundidad funcione, la aplicación cliente debe proporcionar un búfer de profundidad válido que contenga toda la geometría pertinente que se debe tener en cuenta durante LSR.

LSR de profundidad intenta estabilizar el fotograma de vídeo en función del contenido del búfer de profundidad proporcionado. Como consecuencia, el contenido que no se haya presentado en él, como los objetos transparentes, no se puede ajustar mediante LSR y pueden mostrarse artefactos de inestabilidad y de reproyección. 

Para mitigar la inestabilidad de la reproyección de objetos transparentes, puede forzar la escritura del búfer de profundidad. Vea la marca de material *TransparencyWritesDepth* para los materiales de [Color](color-materials.md) y [PBR](pbr-materials.md). Sin embargo, tenga en cuenta que la calidad visual de la interacción de un objeto transparente u opaco puede verse afectada al habilitar esta marca.

## <a name="planar-lsr"></a>LSR planar

El LSR planar no tiene información de profundidad por píxel, como el LSR de profundidad. En su lugar, reproyecta todo el contenido en función de un plano que debe proporcionar cada fotograma.

El LSR planar reproyecta los objetos que se encuentran cerca del plano proporcionado. Cuanto más lejos esté un objeto, mayor será la inestabilidad. Aunque LSR de profundidad es mejor en la reproyección de objetos con profundidades diferentes, el LSR planar puede funcionar de forma más adecuada para alinear el contenido con un plano.

### <a name="configure-planar-lsr-in-unity"></a>Configuración de LSR planar en Unity

Los parámetros de plano se derivan del denominado *punto de enfoque*, que debe proporcionar cada fotograma mediante `UnityEngine.XR.WSA.HolographicSettings.SetFocusPointForFrame`. Para más información, consulte la [API del punto de enfoque de Unity](/windows/mixed-reality/focus-point-in-unity). Si no establece un punto de enfoque, se elegirá una reserva. Sin embargo, la reserva automática suele dar lugar a resultados poco óptimos.

Puede calcular el punto de enfoque usted mismo, aunque es posible que tenga sentido basarlo en el calculado por el host de Remote Rendering. Llame a `RemoteManagerUnity.CurrentSession.GraphicsBinding.GetRemoteFocusPoint` para obtenerlo. Se le pide que proporcione un marco de coordenadas en el que expresar el punto de enfoque. En la mayoría de los casos, solo querrá proporcionar el resultado de `UnityEngine.XR.WSA.WorldManager.GetNativeISpatialCoordinateSystemPtr` aquí.

Normalmente, tanto el cliente como el host representan contenido del que el otro lado no es consciente, como los elementos de la interfaz de usuario en el cliente. Por lo tanto, puede ser conveniente combinar el punto de enfoque remoto con uno calculado localmente.

Los puntos de enfoque calculados en dos marcos sucesivos pueden ser bastante diferentes. Si solo se usan tal cual puede parecer que los hologramas saltan. Para evitar este comportamiento, se recomienda la interpolación entre los puntos de enfoque anterior y actual.

## <a name="next-steps"></a>Pasos siguientes

* [Consultas de rendimiento en el lado servidor](performance-queries.md)