---
title: Referencia de configuración de aplicación para Azure Functions
description: Documentación de referencia para la configuración de la aplicación de Azure Functions o de variables de entorno.
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 6fa8e2d9fb2270d53d8c0419ac7b4d88d79f30fd
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102425709"
---
# <a name="app-settings-reference-for-azure-functions"></a>Referencia de configuración de aplicación para Azure Functions

La configuración de la aplicación en una aplicación de función contiene opciones de configuración global que afectan a todas las funciones de dicha aplicación. Cuando se ejecuta localmente, se accede a esta configuración como [variables de entorno](functions-run-local.md#local-settings-file) locales. Este artículo incluye una lista de las opciones de configuración disponibles en las aplicaciones de funciones.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Hay otras opciones de configuración global en el archivo [host.json](functions-host-json.md) y en [local.settings.json](functions-run-local.md#local-settings-file).

> [!NOTE]  
> Puede usar la configuración de la aplicación para reemplazar los valores de la configuración de host.json sin tener que cambiar el propio archivo host.json. Esto es algo que resulta útil cuando hace falta configurar o modificar valores de host.json concretos para un entorno específico. También permite cambiar la configuración de host.json sin tener que volver a publicar el proyecto. Para más información, consulte el [artículo de referencia de host.json](functions-host-json.md#override-hostjson-values). Para aplicar los cambios realizados en la configuración de la aplicación de funciones, es necesario reiniciar la aplicación de funciones.

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Clave de instrumentación de Application Insights. Use solo uno entre `APPINSIGHTS_INSTRUMENTATIONKEY` y `APPLICATIONINSIGHTS_CONNECTION_STRING`. Cuando Application Insights se ejecuta en una nube soberana, use `APPLICATIONINSIGHTS_CONNECTION_STRING`. Para obtener más información, consulte [Configuración de la supervisión para Azure Functions](configure-monitoring.md). 

|Clave|Valor de ejemplo|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|55555555-af77-484b-9032-64f83bb83bb|

## <a name="applicationinsights_connection_string"></a>APPLICATIONINSIGHTS_CONNECTION_STRING

Cadena de conexión para Application Insights. Use `APPLICATIONINSIGHTS_CONNECTION_STRING` en lugar de `APPINSIGHTS_INSTRUMENTATIONKEY` en los casos siguientes:

+ Cuando la aplicación de funciones requiera las personalizaciones agregadas admitidas mediante el uso de la cadena de conexión. 
+ Cuando la instancia de Application Insights se ejecute en una nube soberana, lo que requiere un punto de conexión personalizado.

Para obtener más información, consulte [Cadenas de conexión](../azure-monitor/app/sdk-connection-string.md). 

|Clave|Valor de ejemplo|
|---|------------|
|APPLICATIONINSIGHTS_CONNECTION_STRING|InstrumentationKey=[key];IngestionEndpoint=[url];LiveEndpoint=[url];ProfilerEndpoint=[url];SnapshotEndpoint=[url];|

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

De forma predeterminada, [Functions Proxies](functions-proxies.md) usará accesos directos para enviar llamadas API desde servidores proxy directamente a funciones en la misma aplicación de funciones. Se usa este acceso directo en lugar de crear una nueva solicitud HTTP. Esta configuración le permite deshabilitar el comportamiento de ese acceso directo.

|Clave|Valor|Descripción|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|Las llamadas con una dirección URL de back-end que apunte a una función en la aplicación de funciones local se no se envará directamente a la función. En su lugar, las solicitudes se devuelven al front-end HTTP para la aplicación de funciones.|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|false|Las llamadas con una dirección URL de back-end que apunte a una función en la aplicación de funciones local se reenvían directamente a la función. Este es el valor predeterminado. |

## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

Esta configuración controla si se descodifican los caracteres `%2F` como barras diagonales en los parámetros de ruta cuando se insertan en la dirección URL de back-end. 

|Clave|Valor|Descripción|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|Los parámetros de ruta con barras diagonales codificadas se descodifican. |
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|false|Todos los parámetros de ruta se pasan sin cambios, que es el comportamiento predeterminado. |

Por ejemplo, considere el archivo proxies.json para una aplicación de función en el dominio `myfunction.com`.

```JSON
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "root": {
            "matchCondition": {
                "route": "/{*all}"
            },
            "backendUri": "example.com/{all}"
        }
    }
}
```

Cuando `AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES` se establece en `true`, la dirección URL `example.com/api%2ftest` se resuelve como `example.com/api/test`. De forma predeterminada, la dirección URL permanece sin cambios como `example.com/test%2fapi`. Para más información, vea [Functions Proxies](functions-proxies.md).

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

En la versión 2.x y posteriores del entorno en tiempo de ejecución de Functions, se configura el comportamiento de la aplicación en función del entorno en tiempo de ejecución. Este valor se lee durante la inicialización y se puede establecer en cualquier valor. El tiempo de ejecución solo respeta los valores de `Development`, `Staging` y `Production`. Cuando esta configuración de la aplicación no está presente durante la ejecución en Azure, se supone que el entorno es `Production`. Use esta opción en lugar de `ASPNETCORE_ENVIRONMENT` si necesita cambiar el entorno de tiempo de ejecución de Azure por uno distinto de `Production`. Azure Functions Core Tools establece `AZURE_FUNCTIONS_ENVIRONMENT` en `Development` cuando se ejecuta en un equipo local y no se puede invalidar en el archivo local.settings.json. Para más información, consulte [Métodos y clase Startup basados en entorno](/aspnet/core/fundamentals/environments#environment-based-startup-class-and-methods).

## <a name="azurefunctionsjobhost__"></a>AzureFunctionsJobHost__\*

En la versión 2.x y versiones posteriores del entorno de ejecución de Functions, la configuración de aplicación puede invalidar la configuración de [host.json](functions-host-json.md) en el entorno actual. Estas invalidaciones se expresan como valores de configuración de la aplicación denominados `AzureFunctionsJobHost__path__to__setting`. Para más información, consulte [Invalidación de valores de host.json](functions-host-json.md#override-hostjson-values).

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Cadena de conexión de la cuenta de almacenamiento opcional para almacenar los registros y mostrarlos en la pestaña **Monitor** del portal. Esta opción solo es válida para las aplicaciones que utilizan la versión de destino 1.x de Azure Functions Runtime. La cuenta de almacenamiento debe ser una de uso general que admite blobs, colas y tablas. Para más información, consulte [Requisitos de la cuenta de almacenamiento](storage-considerations.md#storage-account-requirements).

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>|

> [!NOTE]
> Para disfrutar de un mayor rendimiento y una mejor experiencia, las versiones 2.x y posteriores del entorno de ejecución utilizan APPINSIGHTS_INSTRUMENTATIONKEY y App Insights para realizar la supervisión en lugar de `AzureWebJobsDashboard`.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` significa deshabilitar la página de aterrizaje predeterminada que se muestra para la dirección URL raíz de una aplicación de función. El valor predeterminado es `false`.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Cuando esta configuración de la aplicación se omite o se establece en `false`, una página similar a la del siguiente ejemplo se muestra en respuesta a la dirección URL `<functionappname>.azurewebsites.net`.

![Página de aterrizaje de la aplicación de función](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` significa utilizar el modo de versión cuando se compila código .NET y `false` significa utilizar el modo de depuración. El valor predeterminado es `true`.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Una lista delimitada por comas de características de la versión beta que se van a habilitar. Las características de la versión beta habilitadas por estas marcas no están listas para la producción, pero se pueden habilitar para su uso experimental antes de su publicación.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsFeatureFlags|feature1,feature2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Especifica el repositorio o el proveedor que se utilizará para el almacenamiento de claves. Actualmente, los repositorios admitidos son almacenamiento de blobs ("Blob") y sistema de archivos local ("Files"). El valor predeterminado es blob en la versión 2 y sistema de archivos en la versión 1.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsSecretStorageType|Archivos|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

El entorno de ejecución de Azure Functions usa esta cadena de conexión de la cuenta de almacenamiento para la operativa normal. Algunos usos de esta cuenta de almacenamiento son la administración de claves, la administración de desencadenadores de temporizador y los puntos de comprobación de Event Hubs. La cuenta de almacenamiento debe ser una de uso general que admite blobs, colas y tablas. Consulte [Almacenamiento de la cuenta](functions-infrastructure-as-code.md#storage-account) y [Requisitos de almacenamiento de la cuenta](storage-considerations.md#storage-account-requirements).

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[nombre];AccountKey=[clave]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

Ruta de acceso al compilador que se usa para TypeScript. Le permite reemplazar el valor predeterminado si lo necesita.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="function_app_edit_mode"></a>MODO DE\_EDICIÓN DE\_LA APLICACIÓN DE\_FUNCIÓN

Determina si está habilitada la edición en Azure Portal. Los valores válidos son "readwrite" y "readonly".

|Clave|Valor de ejemplo|
|---|------------|
|MODO DE\_EDICIÓN DE\_LA APLICACIÓN DE\_FUNCIÓN|readonly|

## <a name="functions_extension_version"></a>VERSIÓN DE LA \_EXTENSIÓN\_ DE FUNCTIONS

Versión del tiempo de ejecución de Functions que hospeda la aplicación de funciones. Una tilde (`~`) con la versión principal significa que se utiliza la versión más reciente de esa versión principal (por ejemplo, "~3"). Cuando haya disponibles versiones nuevas de la misma versión principal, se instalarán automáticamente en la aplicación de función. Para anclar la aplicación a una versión específica, use el número completo de la versión (por ejemplo, "3.0.12345"). El valor predeterminado es "~3". Un valor de `~1` ancla la aplicación a la versión 1.x del tiempo de ejecución. Para más información, consulte [Selección de un destino para versiones de runtime de Azure Functions](functions-versions.md).

|Clave|Valor de ejemplo|
|---|------------|
|VERSIÓN DE LA \_EXTENSIÓN\_ DE FUNCTIONS|~3|

## <a name="functions_v2_compatibility_mode"></a>FUNCTIONS\_V2\_COMPATIBILITY\_MODE

Esta opción permite que la aplicación de funciones se ejecute en un modo compatible con la versión 2.x en el tiempo de ejecución de la versión 3.x. Use esta opción solo si se encuentra con problemas al [actualizar la aplicación de funciones de la versión 2.x a la 3.x del tiempo de ejecución](functions-versions.md#migrating-from-2x-to-3x). 

>[!IMPORTANT]
> Esta opción está pensada solo como una solución alternativa a corto plazo mientras actualiza la aplicación para que se ejecute correctamente en la versión 3.x. Esta opción se admite siempre que [se admita el tiempo de ejecución 2.x](functions-versions.md). Si encuentra problemas que impiden que la aplicación se ejecute en la versión 3.x sin usar esta configuración, [informe de su problema](https://github.com/Azure/azure-functions-host/issues/new?template=Bug_report.md).

Requiere que [FUNCTIONS\_EXTENSION\_VERSION](functions-app-settings.md#functions_extension_version) se establezca en `~3`.

|Clave|Valor de ejemplo|
|---|------------|
|FUNCTIONS\_V2\_COMPATIBILITY\_MODE|true|

## <a name="functions_worker_process_count"></a>FUNCTIONS\_WORKER\_PROCESS\_COUNT

Especifica el número máximo de procesos de trabajo de lenguaje, con un valor predeterminado de `1`. El valor máximo permitido es `10`. Las invocaciones de función se distribuyen uniformemente entre los procesos de trabajo de lenguaje. Los procesos de trabajo de lenguaje se generan cada diez segundos hasta que se alcanza el número establecido por FUNCTIONS\_WORKER\_PROCESS\_COUNT. El uso de varios procesos de trabajo de lenguaje no es el mismo que el [escalado](functions-scale.md). Considere la posibilidad de usar esta opción cuando la carga de trabajo tenga una combinación de invocaciones vinculadas a la E/S y vinculadas a la CPU. Esta configuración se aplica a todos los lenguajes que no son .NET.

|Clave|Valor de ejemplo|
|---|------------|
|FUNCTIONS\_WORKER\_PROCESS\_COUNT|2|

## <a name="python_threadpool_thread_count"></a>PYTHON\_THREADPOOL\_THREAD\_COUNT

Especifica el número máximo de subprocesos que un trabajo de lenguaje Python usaría para ejecutar invocaciones de función, con un valor predeterminado de `1` para la versión de Python `3.8` y anteriores. En el caso de la versión de Python `3.9` y posteriores, el valor se establece en `None`. Tenga en cuenta que esta configuración no garantiza el número de subprocesos que se establecerán durante las ejecuciones. La configuración permite que Python amplíe el número de subprocesos al valor especificado. La configuración solo se aplica a las aplicaciones de funciones de Python. Además, la configuración se aplica a la invocación de funciones sincrónicas y no a las corrutinas.

|Clave|Valor de ejemplo|Valor máximo|
|---|------------|---------|
|PYTHON\_THREADPOOL\_THREAD\_COUNT|2|32|


## <a name="functions_worker_runtime"></a>FUNCTIONS\_WORKER\_RUNTIME

Tiempo de ejecución del trabajo del lenguaje que se cargará en la aplicación de función.  Se corresponderá con el lenguaje usado en la aplicación (por ejemplo, "dotnet"). Para las funciones en varios lenguajes deberá publicarlas en varias aplicaciones, cada una con un valor de tiempo de ejecución de trabajo correspondiente.  Los valores válidos son `dotnet` (C#/F#), `node` (JavaScript/TypeScript), `java` (Java), `powershell` (PowerShell) y `python` (Python).

|Clave|Valor de ejemplo|
|---|------------|
|FUNCTIONS\_WORKER\_RUNTIME|dotnet|

## <a name="pip_extra_index_url"></a>PIP\_EXTRA\_INDEX\_URL

El valor de esta configuración indica una dirección URL de índice de paquetes personalizado para las aplicaciones de Python. Use esta configuración cuando necesite ejecutar una compilación remota mediante dependencias personalizadas que se encuentran en un índice de paquetes adicional.   

|Clave|Valor de ejemplo|
|---|------------|
|PIP\_EXTRA\_INDEX\_URL|http://my.custom.package.repo/simple |

Para más información, vea [Dependencias personalizadas](functions-reference-python.md#remote-build-with-extra-index-url) en la referencia para desarrolladores de Python.

## <a name="scale_controller_logging_enabled"></a>SCALE\_CONTROLLER\_LOGGING\_ENABLED

_Esta configuración se encuentra actualmente en versión preliminar_.  

Esta configuración controla el registro del controlador de escala de Azure Functions. Para más información, vea [Registros del controlador de escala](functions-monitoring.md#scale-controller-logs).

|Clave|Valor de ejemplo|
|-|-|
|SCALE_CONTROLLER_LOGGING_ENABLED|AppInsights:Verbose|

El valor de esta clave se proporciona en el formato `<DESTINATION>:<VERBOSITY>`, que se define de la siguiente manera:

[!INCLUDE [functions-scale-controller-logging](../../includes/functions-scale-controller-logging.md)]

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE\_CONTENTAZUREFILECONNECTIONSTRING

Cadena de conexión para la cuenta de almacenamiento donde se almacenan el código de la aplicación de funciones y la configuración en planes de escalado controlados por eventos que se ejecutan en Windows. Para más información, consulte [Creación de una aplicación de funciones](functions-infrastructure-as-code.md#windows).

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[nombre];AccountKey=[clave]|

Solo se usa cuando se implementa en un plan prémium o en un plan de consumo que se ejecuta en Windows. No se admite para los planes de consumo que se ejecutan en Linux. El cambio o la eliminación de esta configuración puede hacer que la aplicación de funciones no se inicie. Para más información, consulte [este artículo de solución de problemas](functions-recover-storage-account.md#storage-account-application-settings-were-deleted). 

## <a name="website_contentovervnet"></a>WEBSITE\_CONTENTOVERVNET

Solo para planes Premium. Un valor de `1` permite escalar la aplicación de funciones cuando tiene una cuenta de almacenamiento restringida a una red virtual. Debe habilitar esta opción al restringir la cuenta de almacenamiento a una red virtual. Para obtener más información, consulte [Restricción de la cuenta de almacenamiento a una red virtual](functions-networking-options.md#restrict-your-storage-account-to-a-virtual-network).

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE_CONTENTOVERVNET|1|

## <a name="website_contentshare"></a>WEBSITE\_CONTENTSHARE

La ruta de acceso del archivo al código y la configuración de la aplicación de funciones de un plan de escalado controlado por eventos en Windows. Se usa con WEBSITE_CONTENTAZUREFILECONNECTIONSTRING. El valor predeterminado es una cadena única que comienza con el nombre de aplicación de función. Consulte [Creación de una aplicación de función](functions-infrastructure-as-code.md#windows).

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

Solo se usa cuando se implementa en un plan prémium o en un plan de consumo que se ejecuta en Windows. No se admite para los planes de consumo que se ejecutan en Linux. El cambio o la eliminación de esta configuración puede hacer que la aplicación de funciones no se inicie. Para más información, consulte [este artículo de solución de problemas](functions-recover-storage-account.md#storage-account-application-settings-were-deleted).

Al usar una instancia de Azure Resource Manager para crear una aplicación de funciones durante la implementación, no incluya WEBSITE_CONTENTSHARE en la plantilla. Esta configuración de aplicación se genera durante la implementación. Para más información, consulte [Automatización de la implementación de recursos para una aplicación de funciones](functions-infrastructure-as-code.md#windows).   

## <a name="website_max_dynamic_application_scale_out"></a>ESCALABILIDAD\_HORIZONTAL\_MÁXIMA\_DE LA\_APLICACIÓN\_DINÁMICA

Número máximo de instancias al que se puede escalar horizontalmente la aplicación de función. El valor predeterminado es sin límite.

> [!IMPORTANT]
> Esta opción está en versión preliminar.  Se ha agregado una [propiedad de aplicación para la escalabilidad horizontal máxima de la función](./event-driven-scaling.md#limit-scale-out) y es la manera recomendada de limitar la escalabilidad horizontal.

|Clave|Valor de ejemplo|
|---|------------|
|ESCALABILIDAD\_HORIZONTAL\_MÁXIMA\_DE LA\_APLICACIÓN\_DINÁMICA|5|

## <a name="website_node_default_version"></a>DEFAULT_VERSION\_DEL NODO\_DEL SITIO WEB

_Solo Windows._  
Establece la versión de Node.js que se usará al ejecutar la aplicación de funciones en Windows. Debe usar una tilde (~) para que el entorno de ejecución use la última versión disponible de la versión principal de destino. Por ejemplo, cuando se establece en `~10`, se usa la versión más reciente de Node.js 10. Cuando se especifica una versión principal con una tilde, no es necesario actualizar manualmente la versión secundaria. 

|Clave|Valor de ejemplo|
|---|------------|
|DEFAULT_VERSION\_DEL NODO\_DEL SITIO WEB|~10|

## <a name="website_run_from_package"></a>WEBSITE\_RUN\_FROM\_PACKAGE

Permite que la aplicación de función se ejecute desde un archivo de paquete montado.

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE\_RUN\_FROM\_PACKAGE|1|

Los valores válidos son una dirección URL que se resuelve en la ubicación de un archivo de paquete de implementación o `1`. Cuando se establece en `1`, el paquete debe estar en la carpeta `d:\home\data\SitePackages`. Cuando se usa la implementación de ZIP con esta configuración, el paquete se carga automáticamente en esta ubicación. En la versión preliminar, este ajuste se denomina `WEBSITE_RUN_FROM_ZIP`. Para más información, vea [Run your functions from a package file](run-functions-from-deployment-package.md) (Ejecución de Azure Functions desde un archivo de paquete).

## <a name="website_time_zone"></a>WEBSITE\_TIME\_ZONE

Permite establecer la zona horaria para la aplicación de funciones. 

|Clave|SO|Valor de ejemplo|
|---|--|------------|
|WEBSITE\_TIME\_ZONE|Windows|Hora estándar del Este|
|WEBSITE\_TIME\_ZONE|Linux|América/Nueva_York|

[!INCLUDE [functions-timezone](../../includes/functions-timezone.md)]

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de cómo actualizar la configuración de la aplicación](functions-how-to-use-azure-function-app-settings.md#settings)

[Consulte la configuración global en el archivo host.json](functions-host-json.md)

[Consulte otros valores de aplicación para aplicaciones de App Service](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
