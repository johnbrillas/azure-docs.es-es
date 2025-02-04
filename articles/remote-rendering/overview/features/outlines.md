---
title: Representación de esquemas
description: Explica cómo realizar la representación de esquemas de selección
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: c04f2312926d3b6d668dff712eedb57d816c8bf3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99592014"
---
# <a name="outline-rendering"></a>Representación de esquemas

Los objetos seleccionados se pueden resaltar visualmente si se agrega la representación de contornos a través del [componente de invalidación de estado jerárquico](../../overview/features/override-hierarchical-state.md). En este capítulo se explica cómo se cambian los parámetros globales para la representación de contornos mediante la API de cliente.

Las propiedades de contorno son una configuración global. Todos los objetos que usen la representación de contorno usarán la misma configuración; no es posible utilizar un color de contorno por cada objeto.

## <a name="parameters-for-outlinesettings"></a>Parámetros de `OutlineSettings`

La clase `OutlineSettings` contiene la configuración relacionada con las propiedades globales del contorno. Expone los siguientes miembros:

| Parámetro      | Tipo    | Descripción                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | Color que se usa para dibujar el contorno. Se omite la parte alfa.         |
| `PulseRateHz`    | FLOAT   | Velocidad a la que el contorno oscila por segundo.|
| `PulseIntensity` | FLOAT   | Intensidad del efecto de pulso de contorno. Debe estar entre 0,0 para que no haya ningún pulso y 1,0 para el pulso completo. La intensidad establece implícitamente la opacidad mínima del contorno como `MinOpacity = 1.0 - PulseIntensity`. |

![Un objeto representado tres veces con distintos parámetros de esquema](./media/outlines.png) El efecto de cambiar el parámetro `color` de amarillo (izquierda) a magenta (centro) y `pulseIntensity` de 0 a 0,8 (derecha).

## <a name="example"></a>Ejemplo

En el código siguiente se muestra un ejemplo de configuración de parámetros de contorno a través de la API:

```cs
void SetOutlineParameters(RenderingSession session)
{
    OutlineSettings outlineSettings = session.Connection.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

```cpp
void SetOutlineParameters(ApiHandle<RenderingSession> session)
{
    ApiHandle<OutlineSettings> outlineSettings = session->Connection()->GetOutlineSettings();
    Color4Ub outlineColor;
    outlineColor.channels = { 255, 255, 0, 255 };
    outlineSettings->SetColor(outlineColor);
    outlineSettings->SetPulseRateHz(2.0f);
    outlineSettings->SetPulseIntensity(0.5f);
}
```

## <a name="performance"></a>Rendimiento

La representación de contornos puede tener un efecto significativo en el rendimiento de la representación. Este efecto varía en función de la relación espacial en el espacio de la pantalla entre los objetos seleccionados y no seleccionados para un marco determinado.

## <a name="api-documentation"></a>Documentación de la API

* [Propiedad RenderingConnection.OutlineSettings de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.outlinesettings)
* [RenderingConnection::OutlineSettings() de C++](/cpp/api/remote-rendering/renderingconnection#outlinesettings)

## <a name="next-steps"></a>Pasos siguientes

* [Componente de invalidación de estado jerárquico](../../overview/features/override-hierarchical-state.md)