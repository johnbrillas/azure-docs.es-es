---
title: Enlaces de Azure RabbitMQ para Azure Functions
description: Aprenda a enviar desencadenadores y enlaces de Azure RabbitMQ en Azure Functions.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/17/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: 2a480f25821f5022295b18ca24abfd2c0fb8a50c
ms.sourcegitcommit: 6e2d37afd50ec5ee148f98f2325943bafb2f4993
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97746531"
---
# <a name="rabbitmq-bindings-for-azure-functions-overview"></a>Introducción a los enlaces de RabbitMQ para Azure Functions

> [!NOTE]
> Los enlaces de RabbitMQ solo se admiten por completo en planes **Premium y Dedicado**. El consumo no se admite.

Azure Functions se integra con [RabbitMQ](https://www.rabbitmq.com/) mediante [desencadenadores y enlaces](./functions-triggers-bindings.md). La extensión de RabbitMQ para Azure Functions permite enviar y recibir mensajes mediante RabbitMQ API con Functions.

| Acción | Tipo |
|---------|---------|
| Ejecutar una función cuando llega un mensaje de RabbitMQ a través de la cola | [Desencadenador](./functions-bindings-rabbitmq-trigger.md) |
| Enviar mensajes de RabbitMQ |[Enlace de salida](./functions-bindings-rabbitmq-output.md) |

## <a name="add-to-your-functions-app"></a>Adición a la aplicación de Functions

Para empezar a desarrollar con esta extensión, asegúrese de [configurar primero un punto de conexión de RabbitMQ](https://github.com/Azure/azure-functions-rabbitmq-extension/wiki/Setting-up-a-RabbitMQ-Endpoint). Para más información sobre RabbitMQ, consulte la [página de introducción](https://www.rabbitmq.com/getstarted.html).

### <a name="functions-3x-and-higher"></a>Functions 3.x y versiones posteriores

Para trabajar con el desencadenador y los enlaces, es necesario hacer referencia al paquete adecuado. En las bibliotecas de clases de .NET se usa el paquete NuGet, mientras que en los demás tipos de aplicaciones se emplea el conjunto de extensiones.

| Idioma                                        | Agregar mediante...                                   | Observaciones
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Instalación del [paquete NuGet], versión 4.x | |
| Script de C#, Java, JavaScript, Python, PowerShell | Registro de [conjunto de extensiones]          | Se recomienda usar la [extensión Azure Tools] con Visual Studio Code. |
| Script de C# (solo en línea en Azure Portal)         | Adición de un enlace                            | Para actualizar extensiones de enlace existentes sin tener que volver a publicar la aplicación de funciones, consulte [Actualización de las extensiones]. |

[Paquete NuGet]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.RabbitMQ
[core tools]: ./functions-run-local.md
[conjunto de extensiones]: ./functions-bindings-register.md#extension-bundles
[Actualización de las extensiones]: ./functions-bindings-register.md
[Extensión Azure Tools]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x-and-2x"></a>Functions 1.x y 2.x

Functions 1.x y 2.x no admiten las extensiones de enlace de RabbitMQ. Use Functions 3.x y versiones posteriores.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de una función cuando se crea un mensaje RabbitMQ (desencadenador)](./functions-bindings-rabbitmq-trigger.md)
- [Envío de mensajes de RabbitMQ desde Azure Functions (enlace de salida)](./functions-bindings-rabbitmq-output.md)