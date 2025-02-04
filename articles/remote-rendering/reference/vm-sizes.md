---
title: Tamaños de servidor
description: Se describen los distintos tamaños de máquina virtual que se pueden asignar.
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.custom: devx-track-csharp
ms.openlocfilehash: 0e2687954fb05ce826e780ae0dbd3931d899885f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99594407"
---
# <a name="server-sizes"></a>Tamaños de servidor

Azure Remote Rendering está disponible en dos configuraciones de servidor: `Standard` y `Premium`.

## <a name="polygon-limits"></a>Límites de polígonos

Remote Rendering con el servidor de tamaño `Standard` tiene un tamaño máximo de escena de polígonos de 20 millones. Remote Rendering con el tamaño `Premium` no exige un máximo forzado, pero el rendimiento puede verse afectado si el contenido supera las funcionalidades de representación del servicio.

Cuando el representador de un tamaño de servidor Standard alcanza esta limitación, cambia la representación a un fondo de tablero de ajedrez:

![Captura de pantalla que muestra una cuadrícula de cuadrados negros y blancos con un menú herramientas.](media/checkerboard.png)

## <a name="specify-the-server-size"></a>Especificación del tamaño del servidor

El tipo deseado de configuración del servidor tiene que especificarse en el momento de la inicialización de la sesión de representación. No se puede cambiar en una sesión en ejecución. Los ejemplos de código siguientes muestran el lugar en el que debe especificarse el tamaño del servidor:

```cs
async void CreateRenderingSession(RemoteRenderingClient client)
{
    RenderingSessionCreationOptions sessionCreationOptions = default;
    sessionCreationOptions.Size = RenderingSessionVmSize.Standard; // or  RenderingSessionVmSize.Premium

    CreateRenderingSessionResult result = await client.CreateNewRenderingSessionAsync(sessionCreationOptions);
    if (result.ErrorCode == Result.Success)
    {
        RenderingSession session = result.Session;
        // do something with the session
    }
}
```

```cpp
void CreateRenderingSession(ApiHandle<RemoteRenderingClient> client)
{
    RenderingSessionCreationOptions sessionCreationOptions;
    sessionCreationOptions.Size = RenderingSessionVmSize::Standard; // or  RenderingSessionVmSize::Premium

    client->CreateNewRenderingSessionAsync(sessionCreationOptions, [](Status status, ApiHandle<CreateRenderingSessionResult> result) {
        if (status == Status::OK && result->GetErrorCode() == Result::Success)
        {
            ApiHandle<RenderingSession> session = result->GetSession();
            // do something with the session
        }
    });
}

```

Para los [scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md), el tamaño de servidor deseado debe especificarse dentro del archivo `arrconfig.json`:

```json
{
  "accountSettings": {
    ...
  },
  "renderingSessionSettings": {
    "vmSize": "<standard or premium>",
    ...
  },
```

### <a name="how-the-renderer-evaluates-the-number-of-polygons"></a>Procedimientos para que el representador evalúe el número de polígonos

El número de polígonos que se considera para la prueba de limitación es el número de polígonos que se pasa al representador. Normalmente, esta geometría es la suma de todos los modelos de los que se han creado instancias, aunque también hay excepciones. La siguiente geometría **no se incluye**:
* Instancias de modelos cargados que están totalmente fuera del tronco de la vista.
* Modelos o partes de modelos que se cambian a invisible mediante el [componente de invalidación del estado jerárquico](../overview/features/override-hierarchical-state.md).

En consecuencia, es posible escribir una aplicación que se destina al tamaño `standard` que carga varios modelos con un número de polígonos próximo al límite de cada modelo. Cuando la aplicación muestra únicamente un único modelo cada vez, el tablero de ajedrez no se desencadena.

### <a name="how-to-determine-the-number-of-polygons"></a>Procedimientos para determinar el número de polígonos

Hay dos maneras de determinar el número de polígonos de un modelo o una escena que contribuyan al límite del presupuesto del tamaño de configuración `standard`:
* En la conversión del modelo, recupere el [archivo JSON de salida de la conversión](../how-tos/conversion/get-information.md) y compruebe la entrada `numFaces` en la [sección *inputStatistics*](../how-tos/conversion/get-information.md#the-inputstatistics-section).
* Si la aplicación está tratando con contenido dinámico, el número de polígonos representados se puede consultar dinámicamente durante el tiempo de ejecución. Use una [consulta de evaluación de rendimiento](../overview/features/performance-queries.md#performance-assessment-queries) y busque el miembro `polygonsRendered` en la estructura `FrameStatistics`. El campo `PolygonsRendered` se establecerá en `bad` cuando el representador alcance la limitación de polígono. El fondo de tablero de ajedrez siempre se atenúa con algún retraso para garantizar que se pueda realizar la acción del usuario después de esta consulta asincrónica. La acción del usuario puede consistir, por ejemplo, en ocultar o eliminar instancias de modelo.

## <a name="pricing"></a>Precios

Para obtener un desglose detallado de los precios de cada tipo de configuración, consulte la página [Precios de Remote Rendering](https://azure.microsoft.com/pricing/details/remote-rendering).

## <a name="next-steps"></a>Pasos siguientes
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
* [Conversión de modelos](../how-tos/conversion/model-conversion.md)

