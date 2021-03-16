---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d09d69db33cadee1b5a214e9cb2f09a508f37242
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431858"
---
|Idioma                                 |1.x         |2.x| 3.x |
|-----------------------------------------|------------|---| --- |
|[C#](../articles/azure-functions/functions-reference-csharp.md)|Disponibilidad general (.NET Framework 4.7)|Disponibilidad general (.NET Core 2.2<sup>1</sup>)| Disponibilidad general (.NET Core 3.1)<br/>[Versión preliminar (.NET 5.0)](../articles/azure-functions/dotnet-isolated-process-guide.md) |
|[JavaScript](../articles/azure-functions/functions-reference-node.md#node-version)|Disponibilidad general (nodo 6)|Disponibilidad general (nodos 10 y 8)| Disponibilidad general (nodos 12 y 10)<br />Versión preliminar (nodo 14) |
|[F#](../articles/azure-functions/functions-reference-fsharp.md)|Disponibilidad general (.NET Framework 4.7)|Disponibilidad general (.NET Core 2.2<sup>1</sup>)| Disponibilidad general (.NET Core 3.1) |
|[Java](../articles/azure-functions/functions-reference-java.md)|N/D|Disponibilidad general (Java 8)| Disponibilidad general (Java 11 y 8)|
|[PowerShell](../articles/azure-functions/functions-reference-powershell.md) |N/D|Disponibilidad general (PowerShell Core 6)| Disponibilidad general (PowerShell 7 y Core 6)|
|[Python](../articles/azure-functions/functions-reference-python.md#python-version)|N/D|Disponibilidad general (Python 3.7 y 3.6)| Disponibilidad general (Python 3.8, 3.7 y 3.6) <br/> Versión preliminar (Python 3.9)|
|[TypeScript](../articles/azure-functions/functions-reference-node.md#typescript) |N/D|GA<sup>2</sup>| GA<sup>2</sup> |


<sup>1</sup> Las aplicaciones de la biblioteca de clases de .NET que tienen como destino la versión 2.x del entorno de ejecución se pueden ejecutar ahora en .NET Core 3.1 en el modo de compatibilidad de .NET Core 2.x. Para más información, consulte [Consideraciones sobre Functions v2.x](../articles/azure-functions/functions-dotnet-class-library.md#functions-v2x-considerations).  
<sup>2</sup> Se admite mediante la transpilación de JavaScript.

Para más información sobre las versiones del lenguaje que se admiten, consulte el artículo de la guía para desarrolladores específica del lenguaje.   
Para información sobre los cambios planeados en la compatibilidad de lenguaje, consulte [Azure roadmap](https://azure.microsoft.com/roadmap/?tag=functions).
