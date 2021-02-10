---
title: Cámara
description: Describe la configuración de la cámara y los casos de uso.
author: christophermanthei
ms.author: chmant
ms.date: 03/07/2020
ms.topic: article
ms.openlocfilehash: dbe86313054706af974ccb324a39e942e9b5ca44
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594136"
---
# <a name="camera"></a>Cámara

Para asegurarse de que el contenido representado de forma local y remota se puede componer sin problemas, es necesario que las distintas propiedades de la cámara permanezcan sincronizadas entre el servidor y el cliente. En particular, la transformación y la proyección de la cámara. Por ejemplo, el contenido representado localmente debe usar la misma transformación y proyección de la cámara con las que se representó el último fotograma remoto.

![Cámara local y remota](./media/camera.png)

En la imagen anterior, la cámara local se ha movido en comparación con el fotograma remoto enviado por el servidor. Como resultado, el contenido local debe representarse desde la misma perspectiva que el fotograma remoto; finalmente, la imagen resultante se vuelve a proyectar en el espacio de la cámara local. Esto se explica de forma detallada en [Reproyección en fase tardía](late-stage-reprojection.md).

El retraso entre la representación local y remota significa que cualquier cambio de esta configuración se aplica con un retraso. Por lo tanto, los nuevos valores no se pueden usar inmediatamente en el mismo fotograma.

Las distancias de plano cercano y plano alejado se pueden definir en la configuración de la cámara. En HoloLens 2, el resto de datos de transformación y proyección se definen mediante el hardware. Al usar la [simulación de escritorio](../../concepts/graphics-bindings.md), se establecen a través de la entrada de su función `Update`.

Los valores modificados se pueden usar localmente en cuanto llega el primer fotograma remoto que se representó con ellos. En HoloLens 2, *HolographicFrame* proporciona los datos que se van a usar para la representación, igual que en cualquier otra aplicación holográfica. En la [simulación de escritorio](../../concepts/graphics-bindings.md), se recuperan a través de la salida de la función `Update`.

## <a name="camera-settings"></a>Configuración de la cámara

Las siguientes propiedades se pueden cambiar en la configuración de la cámara:

**Plano cercano y alejado:**

Para asegurarse de que no se pueden establecer intervalos no válidos, las propiedades **NearPlane** y **FarPlane** son de solo lectura y hay una función independiente, **SetNearAndFarPlane** para cambiar el intervalo. Estos datos se enviarán al servidor al final del fotograma. Al establecer estos valores, **NearPlane** debe ser menor que **FarPlane**. De lo contrario, se producirá un error.

> [!IMPORTANT]
> En Unity, esto se controla automáticamente cuando se cambian los planos cercano y alejado de la cámara.

**EnableDepth**:

En ocasiones, resulta útil deshabilitar la escritura del búfer de profundidad de la imagen remota con fines de depuración. Al deshabilitar la profundidad también se impide que el servidor envíe datos de profundidad, lo que reduce el ancho de banda.

> [!TIP]
> En Unity, se proporciona un componente de depuración llamado **EnableDepthComponent** que se puede usar para activar o desactivar esta característica en la interfaz de usuario del editor.

**InverseDepth**:

> [!NOTE]
> Esta configuración solo es relevante si `EnableDepth` se ha establecido en `true`. En los demás casos, no tendrá ninguna repercusión.

Normalmente, los búferes de profundidad registran valores z en un intervalo de punto flotante de [0;1], donde 0 indica la profundidad del plano cercano y 1 denota la profundidad del plano lejano. También es posible invertir este intervalo y registrar valores de profundidad en el intervalo [1;0]; es decir, la profundidad del plano cercano se convierte en 1 y la profundidad del plano lejano se convierte en 0. Por lo general, la última profundidad mejora la distribución de la precisión de punto flotante en el intervalo z no lineal.

> [!WARNING]
> Un enfoque habitual consiste en invertir los valores de plano cercano y plano lejano en los objetos de cámara. Esto producirá un error en Azure Remote Rendering al probarlo en `CameraSettings`.

La API Azure Remote Rendering debe conocer la convención del búfer de profundidad del representador local para componer correctamente la profundidad remota en el búfer de profundidad local. Si el intervalo del búfer de profundidad es [0;1], deje esta marca como `false`. Si usa un búfer de profundidad invertido con un intervalo de [1;0], establezca la marca `InverseDepth` en `true`.

> [!NOTE]
> En el caso de Unity, el valor correcto ya lo aplica `RenderingConnection`, por lo que no hay necesidad de intervención manual.

Para cambiar la configuración de la cámara, se puede hacer lo siguiente:

```cs
void ChangeCameraSetting(RenderingSession session)
{
    CameraSettings settings = session.Connection.CameraSettings;

    settings.SetNearAndFarPlane(0.1f, 20.0f);
    settings.EnableDepth = false;
    settings.InverseDepth = false;
}
```

```cpp
void ChangeCameraSetting(ApiHandle<RenderingSession> session)
{
    ApiHandle<CameraSettings> settings = session->Connection()->GetCameraSettings();

    settings->SetNearAndFarPlane(0.1f, 20.0f);
    settings->SetEnableDepth(false);
    settings->SetInverseDepth(false);
}
```

## <a name="api-documentation"></a>Documentación de la API

* [CameraSettings de C#](/dotnet/api/microsoft.azure.remoterendering.camerasettings)
* [CameraSettings de C++](/cpp/api/remote-rendering/camerasettings)
* [Función GraphicsBindingSimD3d11.Update de C#](/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11.update)
* [Función GraphicsBindingSimD3d11::Update de C++](/cpp/api/remote-rendering/graphicsbindingsimd3d11#update)

## <a name="next-steps"></a>Pasos siguientes

* [Enlace de gráficos](../../concepts/graphics-bindings.md)
* [Reproyección de fase tardía](late-stage-reprojection.md)