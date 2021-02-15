---
title: Deshabilitamiento de funciones en Azure Functions
description: Aprenda a deshabilitar y a habilitar las funciones de Azure Functions.
ms.topic: conceptual
ms.date: 02/03/2021
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: cbb84308507ea15f1c44c00122a9a59472f12a88
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99551050"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Deshabilitamiento de funciones en Azure Functions

En este artículo se explica cómo deshabilitar una función de Azure Functions. *Deshabilitar* una función implica que el entorno en tiempo de ejecución omita el desencadenador automático que se ha definido para esa función. Esto le permite evitar que una función específica se ejecute sin detener toda la aplicación de funciones.

La forma recomendada de deshabilitar una función es mediante una configuración de aplicación en el formato `AzureWebJobs.<FUNCTION_NAME>.Disabled` que se establece en `true`. Puede crear y modificar esta configuración de aplicación de varias maneras. Por ejemplo, puede usar la [CLI de Azure](/cli/azure/) o la pestaña **Información general** de la función en [Azure Portal](https://portal.azure.com). 

> [!NOTE]  
> Cuando se deshabilita una función desencadenada mediante HTTP utilizando los métodos que se describen en este artículo, el punto de conexión puede seguir estando accesible si se ejecuta en el equipo local.  

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

En el CLI de Azure, use el comando [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) para crear y modificar la configuración de la aplicación. En el siguiente comando se deshabilita una función denominada `QueueTrigger` al crear una configuración de aplicaciones denominada `AzureWebJobs.QueueTrigger.Disabled` y establecerla en `true`. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

Para volver a habilitar la función, vuelva a ejecutar el mismo comando con un valor `false`.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Uso del portal

También puede usar los botones **Habilitar** y **Deshabilitar** de la página de **información general** de la función. Estos botones funcionan mediante el cambio del valor de la configuración de la aplicación `AzureWebJobs.<FUNCTION_NAME>.Disabled`. Esta configuración específica de la función se crea la primera vez que se deshabilita. 

![Conmutador Estado de la función](media/disable-function/function-state-switch.png)

Aunque publique en la aplicación de funciones desde un proyecto local, puede seguir usando el portal para deshabilitar las funciones de la aplicación de funciones. 

> [!NOTE]  
> La funcionalidad de pruebas integrada en el portal omite el valor `Disabled`. Esto significa que una función deshabilitada todavía se ejecuta cuando se inicia desde la ventana **Prueba** en el portal. 

## <a name="localsettingsjson"></a>local.settings.json

Las funciones se pueden deshabilitar de la misma manera cuando se ejecutan localmente. Para deshabilitar una función denominada `HttpExample`, agregue una entrada a la colección de valores del archivo local.settings.json, como se indica a continuación:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true", 
    "AzureWebJobs.HttpExample.Disabled": "true"
  }
}
``` 

## <a name="other-methods"></a>Otros métodos

Aunque se recomienda usar el método de configuración de la aplicación para todos los lenguajes y todas las versiones en tiempo de ejecución, existen otras formas de deshabilitar funciones. Estos métodos, que varían según el lenguaje y la versión en tiempo de ejecución, se mantienen para compatibilidad con versiones anteriores. 

### <a name="c-class-libraries"></a>Biblioteca de clases de C#

En una función de la biblioteca de clases, puede usar también el atributo `Disable` para evitar que se desencadene la función. Este atributo permite personalizar el nombre de la configuración utilizada para deshabilitar la función. Use la versión del atributo que le permite definir un parámetro de constructor que hace referencia a una configuración de aplicación booleana, como se muestra en el ejemplo siguiente:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Este método le permite habilitar y deshabilitar la función cambiando la configuración de la aplicación, sin necesidad de volver a compilar y a implementar. Cambiar una configuración de aplicación hace que la aplicación de función se reinicie, por lo que el estado deshabilitado se reconocerá inmediatamente.

También hay un constructor para el parámetro que no acepta una cadena para el nombre de la configuración. No se recomienda esta versión del atributo. Si utiliza esta versión, debe volver a compilar e implementar el proyecto para cambiar el estado deshabilitado de la función.

### <a name="functions-1x---scripting-languages"></a>Functions 1.x: lenguajes de scripting

En la versión 1.x, también puede usar la propiedad `disabled` del archivo *function.json* para indicar al entorno en tiempo de ejecución que no desencadene una función. Este método solo funciona para lenguajes de scripting como Script de C# y JavaScript. La propiedad `disabled` se puede establecer en `true` o en el nombre de una configuración de aplicación:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
or 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

En el segundo ejemplo, la función se deshabilita cuando hay una configuración de aplicación que se denomina IS_DISABLED y se establece en `true` o 1.

>[!IMPORTANT]  
>El portal ahora usa la configuración de la aplicación para deshabilitar las funciones de v1.x. Cuando un valor de configuración de la aplicación entra en conflicto con el archivo function.jason, puede producirse un error. Debe quitar la propiedad `disabled` del archivo function.json para evitar errores. 


## <a name="next-steps"></a>Pasos siguientes

Este artículo trata acerca de cómo deshabilitar los desencadenadores automáticos. Para más información sobre desencadenadores, consulte [Desencadenadores y enlaces](functions-triggers-bindings.md).
