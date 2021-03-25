---
title: 'Supervisiones en Durable Functions (Python): Azure'
description: Aprenda a implementar un monitor de estado con la extensión Durable Functions para Azure Functions (Python).
author: davidmrdavid
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: azfuncdf
ms.openlocfilehash: 62b3c9bb1c6fd53d9f11227a9d7e774d56859d04
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434770"
---
# <a name="monitor-scenario-in-durable-functions---github-issue-monitoring-sample"></a>Ejemplo de supervisión de problemas del escenario de supervisión en Durable Functions: GitHub

El patrón de supervisión hace referencia a un proceso periódico flexible de un flujo de trabajo; por ejemplo, realizar un sondeo hasta que se cumplan determinadas condiciones. En este artículo se explica un ejemplo que usa Durable Functions para implementar la supervisión.

## <a name="prerequisites"></a>Prerrequisitos

* [Complete el artículo de inicio rápido](quickstart-python-vscode.md).
* [Clone o descargue el proyecto de ejemplos de GitHub](https://github.com/Azure/azure-functions-durable-python/tree/main/samples/).


## <a name="scenario-overview"></a>Información general de escenario

En este ejemplo se supervisa el recuento de problemas en un repositorio de GitHub y se alerta al usuario si hay más de tres problemas abiertos. Puede usar una función desencadenada por temporizador normal para solicitar los recuentos de problemas abiertos a intervalos regulares. Sin embargo, un problema con este enfoque es la **administración del ciclo de vida**. Si solo se debe enviar una alerta, la supervisión debe deshabilitarse después de detectar tres problemas o más. El patrón de supervisión puede finalizar su propia ejecución, entre otras ventajas:

* Las supervisiones se ejecutan en intervalos, no en programaciones: un desencadenador en temporizador *se ejecuta* cada hora; una supervisión *espera* una hora entre acciones. Las acciones de una supervisión no se solaparán a menos que se especifique, lo que puede ser importante para tareas de ejecución prolongada.
* Las supervisiones pueden tener intervalos dinámicos: el tiempo de espera puede cambiar en función de alguna condición.
* Las supervisiones pueden finalizar cuando se cumple alguna condición o cuando otro proceso las finaliza.
* Las supervisiones pueden aceptar parámetros. En el ejemplo se muestra cómo aplicar el mismo proceso de supervisión de repositorio a cualquier repositorio de GitHub público y número de teléfono solicitados.
* Las supervisiones son escalables. Debido a que cada supervisión es una instancia de orquestación, se pueden crear múltiples supervisiones sin tener que crear nuevas funciones ni definir más código.
* Las supervisiones se integran fácilmente en flujos de trabajo mayores. Una supervisión puede ser una sección de una función de orquestación más compleja, o una [suborquestación](durable-functions-sub-orchestrations.md).

## <a name="configuration"></a>Configuración

### <a name="configuring-twilio-integration"></a>Configuración de la integración de Twilio

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>Funciones

En este artículo se explican las funciones siguientes en la aplicación de ejemplo:

* `E3_Monitor`: una [función de orquestador](durable-functions-bindings.md#orchestration-trigger) que llama periódicamente a `E3_TooManyOpenIssues`. Llama a `E3_SendAlert` si el valor devuelto de `E3_TooManyOpenIssues` es `True`.
* `E3_TooManyOpenIssues`: una [función de actividad](durable-functions-bindings.md#activity-trigger) que comprueba si un repositorio tiene demasiados problemas abiertos. Para fines de demostración, se considera que tener más de tres problemas abiertos es excesivo.
* `E3_SendAlert`: una función de la actividad que envía un mensaje SMS por Twilio.

### <a name="e3_monitor-orchestrator-function"></a>Función de orquestador E3_Monitor


La función **E3_Monitor** utiliza la norma *function.json* para las funciones de orquestador.

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/function.json)]

Este es el código que implementa la función:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/\_\_init\_\_.py)]


Esta función de orquestador realiza las acciones siguientes:

1. Obtiene el *repositorio* que se va a supervisar y el *número de teléfono* al que se enviará una notificación por SMS.
2. Determina el tiempo de expiración de la supervisión. El ejemplo utiliza un valor modificable por brevedad.
3. Llama a **E3_TooManyOpenIssues** para determinar si hay demasiados problemas abiertos en el repositorio solicitado.
4. Si es así, llama a **E3_SendAlert** para enviar una notificación por SMS al número de teléfono solicitado.
5. Crea un temporizador duradero para reanudar la orquestación en el siguiente intervalo de sondeo. El ejemplo utiliza un valor modificable por brevedad.
6. Continúa ejecutándose hasta que la hora universal coordinada actual pasa la hora de expiración de la supervisión o se envía una alerta por SMS.

Se pueden ejecutar varias instancias de orquestador de forma simultanea llamando varias veces a la función de orquestador. Se pueden especificar el repositorio que se va a supervisar y el número de teléfono al que se va a enviar una alerta por SMS. Por último, tenga en cuenta que la función de orquestador *no* se ejecuta mientras se espera el temporizador, por lo que no se aplica ningún cargo por ello.


### <a name="e3_toomanyopenissues-activity-function"></a>Función de actividad E3_TooManyOpenIssues

Al igual que otros ejemplos, las funciones auxiliares de actividad son básicamente funciones normales que usan el enlace del desencadenador `activityTrigger`. La función **E3_TooManyOpenIssues** obtiene una lista de los problemas abiertos actualmente en el repositorio y determina estos son "demasiados": más de tres según el ejemplo.


*function.json* se define como sigue:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

Y esta es la implementación.

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/\_\_init\_\_.py)]


### <a name="e3_sendalert-activity-function"></a>Función de actividad E3_SendAlert

La función **E3_SendAlert** usa el enlace de Twilio para enviar un mensaje SMS que notifica al usuario final que existen por lo menos tres problemas abiertos que esperan una resolución.


La *function.json* es simple:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

Y este es el código que envía el mensaje SMS:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_SendAlert/\_\_init\_\_.py)]


## <a name="run-the-sample"></a>Ejecución del ejemplo

También necesita una cuenta de [GitHub](https://github.com/). Úsela para crear un repositorio público temporal en el que pueda abrir problemas.

Con las funciones desencadenadas mediante HTTP del ejemplo, puede iniciar la orquestación con el envío de la siguiente solicitud HTTP POST:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "repo": "<your github handle>/<a new github repo under your user>", "phone": "+1425XXXXXXX" }
```

Por ejemplo, si el nombre de usuario de GitHub es `foo` y el repositorio es `bar`, del valor de `"repo"` debe ser `"foo/bar"`.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

Se inicia la instancia de **E3_Monitor** y realiza la consulta de los problemas abiertos en el repositorio proporcionado. Si se encuentran tres problemas abiertos como mínimo, llama a una función de actividad para enviar una alerta; de lo contrario, establece un temporizador. Cuando expire el temporizador, se reanudará la orquestación.

Puede ver los resultados de la actividad de orquestación al examinar los registros de la función en el portal de Azure Functions.

```
[2020-12-04T18:24:30.007Z] Executing 'Functions.HttpStart' (Reason='This function was programmatically 
called via the host APIs.', Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6)
[2020-12-04T18:24:30.769Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=058e656e-bcb1-418c-95b3-49afcd07bd08)
[2020-12-04T18:24:30.847Z] Started orchestration with ID = '788420bb31754c50acbbc46e12ef4f9c'.
[2020-12-04T18:24:30.932Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=058e656e-bcb1-418c-95b3-49afcd07bd08, Duration=174ms)
[2020-12-04T18:24:30.955Z] Executed 'Functions.HttpStart' (Succeeded, Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6, Duration=1028ms)
[2020-12-04T18:24:31.229Z] Executing 'Functions.E3_TooManyOpenIssues' (Reason='(null)', Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3)
[2020-12-04T18:24:31.772Z] Executed 'Functions.E3_TooManyOpenIssues' (Succeeded, Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3, Duration=555ms)
[2020-12-04T18:24:40.754Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=23915e4c-ddbf-46f9-b3f0-53289ed66082)
[2020-12-04T18:24:40.789Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=23915e4c-ddbf-46f9-b3f0-53289ed66082, Duration=38ms)
(...trimmed...)
```

La orquestación finalizará una vez que se alcance el tiempo de espera o se detecten más de tres problemas abiertos. También puede usar la API `terminate` dentro de otra función o invocar el webhook HTTP POST **terminatePostUri** al que se hace referencia en la respuesta 202 anterior. Para usar el webhook, reemplace `{text}` por el motivo de la terminación temprana. La dirección URL de HTTP POST tendrá un aspecto similar al siguiente:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Pasos siguientes

Este ejemplo ha demostrado cómo usar Durable Functions para supervisar el estado de un origen externo mediante [temporizadores durables](durable-functions-timers.md) y lógica condicional. En el ejemplo siguiente se muestra cómo usar eventos externos y [temporizadores duraderos](durable-functions-timers.md) para controlar la interacción humana.

> [!div class="nextstepaction"]
> [Ejecución de la interacción humana de ejemplo](durable-functions-phone-verification.md)
