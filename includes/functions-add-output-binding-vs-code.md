---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/30/2020
ms.author: glenga
ms.openlocfilehash: 3759dce2ab527cab5b2d2afe8eae30461cbc9031
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99493959"
---
Visual Studio Code le permite agregar enlaces a su archivo function.json siguiendo un conjunto práctico de mensajes. 

Para agregar un enlace, abra la paleta de comandos (F1) y escriba **Azure Functions: agregar enlace...** , elija la función para el nuevo enlace y, a continuación, siga las indicaciones, que pueden variar según el tipo de enlace que se agregue a la función. 

Los siguientes son mensajes de ejemplo para definir un nuevo enlace de salida de almacenamiento:

| Solicitud | Value | Descripción |
| -------- | ----- | ----------- |
| **Select binding direction** (Seleccionar dirección de enlace) | `out` | El enlace es un enlace de salida. |
| **Select binding with direction** (Seleccionar enlace con dirección) | `Azure Queue Storage` | El enlace es un enlace de cola de Azure Storage. |
| **The name used to identify this binding in your code** (Nombre identificativo del enlace en el código) | `msg` | Nombre que identifica el parámetro de enlace al que se hace referencia en el código. |
| **The queue to which the message will be sent** (Cola donde se enviará el mensaje) | `outqueue` | El nombre de la cola en la que escribe el enlace. Cuando no existe *queueName*, el enlace lo crea durante el primer uso. |
| **Seleccione la configuración en "local.settings.json"** | `MyStorageConnection` | El nombre de una configuración de la aplicación que contiene la cadena de conexión de la cuenta de almacenamiento. El valor `AzureWebJobsStorage` contiene la cadena de conexión de la cuenta de almacenamiento que creó con la aplicación de función. |

También puede hacer clic con el botón derecho (o Ctrl+clic en macOS) directamente en el archivo **function.json** en la carpeta de función, seleccionar **Agregar enlace** y seguir las mismas indicaciones.

En este ejemplo, se agrega el siguiente enlace a la matriz `bindings` en el archivo function.json:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```