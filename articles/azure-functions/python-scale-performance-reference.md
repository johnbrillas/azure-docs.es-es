---
title: Mejora del rendimiento de las aplicaciones de Python en Azure Functions
description: Obtenga información sobre cómo desarrollar aplicaciones de Azure Functions con Python que tengan un alto rendimiento y escalen bien bajo carga.
ms.topic: article
ms.date: 10/13/2020
ms.custom: devx-track-python
ms.openlocfilehash: e3bbdb8819062d45d071633e0208fb58a003da54
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98786113"
---
# <a name="improve-throughput-performance-of-python-apps-in-azure-functions"></a>Mejora del rendimiento de las aplicaciones de Python en Azure Functions

Al desarrollar contenido para Azure Functions con Python, necesita comprender el rendimiento de las funciones y cómo afecta a la manera en la que se escala la aplicación de funciones. Esto es todavía más importante al diseñar aplicaciones de alto rendimiento. Los principales factores que hay que tener en cuenta al diseñar, escribir y configurar aplicaciones de funciones son las configuraciones de rendimiento y de escalado horizontal.

## <a name="horizontal-scaling"></a>Escalado horizontal
De forma predeterminada, Azure Functions supervisa automáticamente la carga en la aplicación y crea instancias de host adicionales para Python según sea necesario. Azure Functions usa umbrales integrados para diferentes tipos de desencadenadores a la hora de decidir cuándo se deben agregar instancias, como la antigüedad de los mensajes y el tamaño de la cola para QueueTrigger. Estos umbrales no puede configurarlos el usuario. Para obtener más información, consulte [Escalado basado en eventos en Azure Functions](event-driven-scaling.md).

## <a name="improving-throughput-performance"></a>Mejora del rendimiento

Las configuraciones predeterminadas son adecuadas para la mayoría de las aplicaciones de Azure Functions. Sin embargo, puede mejorar el rendimiento de las aplicaciones mediante configuraciones basadas en el perfil de la carga de trabajo. El primer paso es comprender el tipo de carga de trabajo que se está ejecutando.

| Tipo de carga de trabajo | Características de la aplicación de funciones       | Ejemplos                                          |
| ------------- | ---------------------------------- | ------------------------------------------------- |
| **Relacionado con la E/S**     | • La aplicación necesita controlar muchas invocaciones simultáneas.<br>• La aplicación procesa un gran número de eventos de E/S, como llamadas de red y lecturas o escrituras de disco. | • API web                                          |
| **Relacionado con la CPU**     | • La aplicación realiza cálculos de ejecución prolongada, como el cambio de tamaño de las imágenes.<br>• La aplicación realiza la transformación de datos.                                                | • Procesamiento de datos<br>• Inferencia de Machine Learning<br> |

 
Como las cargas de trabajo de las funciones reales suelen ser una combinación dependiente de E/S y CPU, debe generar un perfil de la aplicación bajo cargas de producción realistas.


### <a name="performance-specific-configurations"></a>Configuraciones específicas del rendimiento

Después de entender el perfil de la carga de trabajo de la aplicación de funciones, puede usar las configuraciones que se muestran a continuación para mejorar el rendimiento de las funciones.

* [Asincrónico](#async)
* [Trabajo en varios lenguajes](#use-multiple-language-worker-processes)
* [Máximo de trabajos en un proceso de trabajo de lenguaje](#set-up-max-workers-within-a-language-worker-process)
* [Bucle de eventos](#managing-event-loop)
* [Escalado vertical](#vertical-scaling)



#### <a name="async"></a>Async

Dado que [Python es un entorno de ejecución de un solo subproceso](https://wiki.python.org/moin/GlobalInterpreterLock), una instancia de host para Python solo puede procesar una invocación de función cada vez de forma predeterminada. En el caso de las aplicaciones que procesan un gran número de eventos de E/S o que son dependientes de las operaciones de E/S, puede mejorar considerablemente el rendimiento mediante la ejecución de funciones de forma asincrónica.

Para ejecutar una función de forma asincrónica, use la instrucción `async def`, que ejecuta la función directamente con [asyncio](https://docs.python.org/3/library/asyncio.html):

```python
async def main():
    await some_nonblocking_socket_io_op()
```
Este es un ejemplo de una función con un desencadenador HTTP que usa el cliente http [aiohttp](https://pypi.org/project/aiohttp/):

```python
import aiohttp

import azure.functions as func

async def main(req: func.HttpRequest) -> func.HttpResponse:
    async with aiohttp.ClientSession() as client:
        async with client.get("PUT_YOUR_URL_HERE") as response:
            return func.HttpResponse(await response.text())

    return func.HttpResponse(body='NotFound', status_code=404)
```


Una función sin la palabra clave `async` se ejecuta de forma automática en un grupo de subprocesos de ThreadPoolExecutor:

```python
# Runs in an ThreadPoolExecutor threadpool. Number of threads is defined by PYTHON_THREADPOOL_THREAD_COUNT. 
# The example is intended to show how default synchronous function are handled.

def main():
    some_blocking_socket_io()
```

Para sacar el máximo beneficio de ejecutar funciones de forma asincrónica, la operación de E/S o la biblioteca que se usa en el código también debe tener implementado el patrón asincrónico. El uso de operaciones de E/S sincrónicas en funciones que se definen como asincrónicas **puede perjudicar** al rendimiento general. Si las bibliotecas que usa no tienen implementada una versión asincrónica, puede beneficiarse igualmente de ejecutar el código de forma asincrónica si [administra el bucle de eventos](#managing-event-loop) en la aplicación. 

Estos son algunos ejemplos de bibliotecas cliente que han implementado el patrón asincrónico:
- [aiohttp](https://pypi.org/project/aiohttp/): cliente/servidor http para asyncio. 
- [Streams API](https://docs.python.org/3/library/asyncio-stream.html): primitivas preparadas para async/await de alto nivel para trabajar con la conexión de red.
- [Janus Queue](https://pypi.org/project/janus/): cola con reconocimiento de asyncio segura para subprocesos para Python
- [pyzmq](https://pypi.org/project/pyzmq/): enlaces de Python para ZeroMQ.
 
##### <a name="understanding-async-in-python-worker"></a>Descripción de async en el trabajo de Python

Al definir `async` delante de una signatura de función, Python marcará la función como una corrutina. Cuando se llama a la corrutina, se puede programar como una tarea en un bucle de eventos. Al llamar a `await` en una función asincrónica, registra una continuación en el bucle de eventos y permite que el bucle procese la siguiente tarea durante el tiempo de espera.

En nuestro trabajo de Python, este comparte el bucle de eventos con la función `async` del cliente y es capaz de controlar varias solicitudes simultáneamente. Recomendamos encarecidamente que los clientes hagan uso de bibliotecas compatibles con asyncio (por ejemplo, [aiohttp](https://pypi.org/project/aiohttp/) y [pyzmq](https://pypi.org/project/pyzmq/)). La aplicación de estas recomendaciones aumentará considerablemente el rendimiento de la función, en comparación con las bibliotecas implementadas de manera sincrónica.

> [!NOTE]
>  Si la función se declara como `async` sin `await` dentro de la implementación, el rendimiento de la función se verá gravemente afectado, ya que se bloqueará el bucle de eventos, lo que impide al trabajo de Python administrar solicitudes simultáneas.

#### <a name="use-multiple-language-worker-processes"></a>Uso de procesos de trabajo de varios lenguajes

De forma predeterminada, cada instancia de host de Functions tiene un único proceso de trabajo de lenguaje. Puede aumentar el número de procesos de trabajo por host (hasta 10) mediante la configuración de la aplicación [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count). Al hacerlo, Azure Functions intenta distribuir uniformemente las invocaciones de función simultáneas en estos trabajos.

En el caso de las aplicaciones dependientes de CPU, debe establecer el número de trabajos de lenguaje para que sea igual o mayor que el número de núcleos disponibles por aplicación de funciones. Para obtener más información, consulte las [SKU de instancias disponibles](functions-premium-plan.md#available-instance-skus). 

Las aplicaciones dependientes de E/S también pueden beneficiarse del aumento del número de procesos de trabajo más allá del número de núcleos disponibles. Tenga en cuenta que establecer un número de roles de trabajo demasiado alto puede afectar al rendimiento general debido al mayor número de cambios de contexto necesarios. 

FUNCTIONS_WORKER_PROCESS_COUNT se aplica a cada host que Functions crea al escalar horizontalmente la aplicación para satisfacer la demanda.

#### <a name="set-up-max-workers-within-a-language-worker-process"></a>Configuración de un máximo de trabajos en un proceso de trabajo de lenguaje

Como se mencionó en la [sección](#understanding-async-in-python-worker) sobre async, el trabajo de lenguaje de Python trata las funciones y las [corrutinas](https://docs.python.org/3/library/asyncio-task.html#coroutines) de manera diferente. Una corrutina se ejecuta dentro del mismo bucle de eventos en el que se ejecuta el trabajo del lenguaje. Por otro lado, una invocación de función se ejecuta dentro de [ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html#threadpoolexecutor), que se mantiene mediante el trabajo del lenguaje, como un subproceso.

Puede establecer el valor del máximo de trabajadores permitido para ejecutar funciones de sincronización mediante la configuración de la aplicación [PYTHON_THREADPOOL_THREAD_COUNT](functions-app-settings.md#python_threadpool_thread_count). Este valor establece el argumento `max_worker` del objeto ThreadPoolExecutor, que permite a Python usar un grupo de `max_worker` subprocesos como máximo para ejecutar llamadas de forma asincrónica. `PYTHON_THREADPOOL_THREAD_COUNT` se aplica a cada trabajo que crea Functions, y Python decide cuándo se crea un subproceso o se reutiliza el subproceso inactivo existente. En el caso de versiones anteriores de Python (es decir, `3.8`, `3.7` y `3.6`), el valor `max_worker` se establece en 1. Para la versión `3.9` de Python, `max_worker` se establece en `None`.

En el caso de las aplicaciones dependientes de la CPU, debe mantener la configuración en un número bajo. Empiece por 1 y vaya aumentándolo a medida que experimente con la carga de trabajo. Esta sugerencia tiene como objetivo reducir el tiempo empleado en los cambios de contexto y permitir que las tareas dependientes de la CPU finalicen.

En el caso de las aplicaciones dependientes de E/S, debería obtener beneficios considerables si aumenta el número de subprocesos que trabajan en cada invocación. Se recomienda comenzar con el valor predeterminado de Python (número de núcleos + 4) y, luego, ajustarlo en función de los valores de rendimiento que se muestren.

En el caso de las aplicaciones con cargas de trabajo mixtas, debe equilibrar las configuraciones de `FUNCTIONS_WORKER_PROCESS_COUNT` y `PYTHON_THREADPOOL_THREAD_COUNT` para maximizar el rendimiento. Para comprender a qué dedican la mayor parte del tiempo las aplicaciones de funciones, se recomienda generar un perfil de ellas y establecer los valores según el comportamiento que presenten. Consulte también esta [sección](#use-multiple-language-worker-processes) para obtener información sobre la configuración de la aplicación FUNCTIONS_WORKER_PROCESS_COUNT.

> [!NOTE]
>  Aunque estas recomendaciones se aplican tanto a las funciones desencadenadas por HTTP como a las no desencadenadas por HTTP, puede que necesite ajustar otras configuraciones específicas del desencadenador para las funciones no desencadenadas por HTTP para obtener el rendimiento esperado de las aplicaciones de funciones. Para obtener más información al respecto, consulte este [artículo](functions-best-practices.md).


#### <a name="managing-event-loop"></a>Administración del bucle de eventos

Debe usar bibliotecas de terceros compatibles con asyncio. Si ninguna de las bibliotecas de terceros satisface sus necesidades, también puede administrar los bucles de eventos en Azure Functions. La administración de los bucles de eventos proporciona más flexibilidad para la administración de los recursos de proceso y, además, permite encapsular las bibliotecas de E/S sincrónicas en corrutinas.

Existen muchos documentos oficiales de Python de gran utilidad en los que se describen [corrutinas y tareas](https://docs.python.org/3/library/asyncio-task.html), así como el [bucle de eventos](https://docs.python.org/3.8/library/asyncio-eventloop.html), mediante la biblioteca **asyncio** integrada.

Tome como ejemplo la siguiente biblioteca [requests](https://github.com/psf/requests). Este fragmento de código usa la biblioteca **asyncio** para encapsular el método `requests.get()` en una corrutina, que ejecuta varias solicitudes web para SAMPLE_URL simultáneamente.


```python
import asyncio
import json
import logging

import azure.functions as func
from time import time
from requests import get, Response


async def invoke_get_request(eventloop: asyncio.AbstractEventLoop) -> Response:
    # Wrap requests.get function into a coroutine
    single_result = await eventloop.run_in_executor(
        None,  # using the default executor
        get,  # each task call invoke_get_request
        'SAMPLE_URL'  # the url to be passed into the requests.get function
    )
    return single_result

async def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    eventloop = asyncio.get_event_loop()

    # Create 10 tasks for requests.get synchronous call
    tasks = [
        asyncio.create_task(
            invoke_get_request(eventloop)
        ) for _ in range(10)
    ]

    done_tasks, _ = await asyncio.wait(tasks)
    status_codes = [d.result().status_code for d in done_tasks]

    return func.HttpResponse(body=json.dumps(status_codes),
                             mimetype='application/json')
```
#### <a name="vertical-scaling"></a>Escalado vertical
Si quiere obtener más unidades de procesamiento, especialmente en el caso de una operación dependiente de la CPU, podría conseguirlo al actualizar al plan Prémium con especificaciones más altas. Con unidades de procesamiento más altas, puede ajustar el número de procesos de trabajo en función del número de núcleos disponibles y lograr un mayor grado de paralelismo. 

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el desarrollo de Python de Azure Functions, consulte los siguientes recursos:

* [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md)
* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)

