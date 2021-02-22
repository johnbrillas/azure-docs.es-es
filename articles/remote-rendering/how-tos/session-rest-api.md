---
title: La API REST de administración de sesión
description: Describe cómo administrar sesiones
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 9eb1bb87792dbc61e7c85dbc20c136499f23f67c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530219"
---
# <a name="use-the-session-management-rest-api"></a>Uso de la API REST de administración de sesión

Para usar la funcionalidad de Azure Remote Rendering, debe crear una *sesión*. Cada sesión corresponde a un servidor que se va a asignar en Azure al que se puede conectar un dispositivo cliente. Cuando se conecta un dispositivo, el servidor representa los datos solicitados y sirve el resultado como una secuencia de vídeo. Durante la creación de la sesión, se elige el [tipo de servidor](../reference/vm-sizes.md) para la ejecución, que determina los precios. Una vez que la sesión ya no se necesita, debe detenerse. Si no se detiene manualmente, se apagará de forma automática cuando expire el *tiempo de concesión* de la sesión.

## <a name="rest-api-reference"></a>Referencia de la API REST

La referencia de la API REST se puede encontrar [aquí](https://docs.microsoft.com/rest/api/mixedreality/2021-01-01preview/remoterendering) y las definiciones de Swagger [aquí](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality).
Proporcionamos un script de PowerShell en el [repositorio de ejemplos de ARR](https://github.com/Azure/azure-remote-rendering) en la carpeta *Scripts* que se denomina *RenderingSession.ps1* y muestra el uso de nuestro servicio. El script y su configuración se describen aquí: [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md).
También proporcionamos SDK para [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java y Python.

> [!IMPORTANT]
> La latencia es un factor importante a la hora de usar la representación remota. Para obtener la mejor experiencia, cree sesiones en la región más cercana a usted. La [prueba de latencia de Azure](https://www.azurespeed.com/Azure/Latency) se puede usar para determinar la región más cercana.

> [!IMPORTANT]
> Se necesita un SDK del entorno de ejecución de Enrutamiento de solicitud de aplicaciones para que un dispositivo cliente se conecte a una sesión de representación. Estos SDK están disponibles en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering?view=remoterendering) y [C++](https://docs.microsoft.com/cpp/api/remote-rendering/). Además de conectarse al servicio, estos SDK también se pueden usar para iniciar y detener sesiones.

## <a name="next-steps"></a>Pasos siguientes

* [n](frontend-apis.md)
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
