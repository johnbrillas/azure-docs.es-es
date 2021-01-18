---
title: Ingesta de telemetría desde IoT Hub
titleSuffix: Azure Digital Twins
description: Consulte cómo ingerir los mensajes de telemetría de dispositivos desde IoT Hub.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 9/15/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9ecc14aa9591d6e62dccd9899a80de44411928a1
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051095"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Ingesta de telemetría de IoT Hub en Azure Digital Twins

Azure Digital Twins se basa en los datos de los dispositivos IoT y otros orígenes. Un origen común de los datos del dispositivo que se va a usar en Azure Digital Twins es [IoT Hub](../iot-hub/about-iot-hub.md).

El proceso de ingesta de datos en Azure Digital Twins consiste en configurar un recurso de proceso externo como, por ejemplo, una función creada con [Azure Functions](../azure-functions/functions-overview.md). La función recibe los datos y usa las [API de DigitalTwins](/rest/api/digital-twins/dataplane/twins) para establecer propiedades o desencadenar eventos de telemetría en los [gemelos digitales](concepts-twins-graph.md) en consecuencia. 

Este documento de procedimientos le guía en el proceso de escritura de una función que pueda ingerir datos de telemetría de IoT Hub.

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar con este ejemplo, debe configurar los siguientes recursos como requisito previo:
* **Una instancia de IoT Hub**. Para instrucciones, consulte la sección *Creación de una instancia de IoT Hub* de [este inicio rápido de IoT Hub](../iot-hub/quickstart-send-telemetry-cli.md).
* **Una función** con los permisos correctos para llamar a la instancia de Digital Twins. Para instrucciones, consulte [*Procedimiento: Configuración de una función de Azure para procesar datos*](how-to-create-azure-function.md). 
* **Una instancia de Azure Digital Twins** que recibirá la telemetría del dispositivo. Para instrucciones, consulte [*Procedimiento: Configuración de una instancia de Azure Digital Twins y autenticación*](./how-to-set-up-instance-portal.md).

### <a name="example-telemetry-scenario"></a>Escenario de telemetría de ejemplo

En este procedimiento se explica cómo enviar mensajes desde IoT Hub a Azure Digital Twins, mediante una función de Azure. Hay muchas configuraciones y estrategias de coincidencia posibles que puede usar para enviar mensajes, pero el ejemplo de este artículo contiene los siguientes elementos:
* Un dispositivo de termómetro en IoT Hub, con un identificador de dispositivo conocido
* Un gemelo digital para representar el dispositivo, con un identificador coincidente

> [!NOTE]
> En este ejemplo se usa una coincidencia de identificador sencillo entre el identificador de dispositivo y el identificador de gemelo digital correspondiente, pero es posible proporcionar asignaciones más sofisticadas del dispositivo a su gemelo (por ejemplo, con una tabla de asignación).

Siempre que el dispositivo de termostato envía un evento de telemetría de temperatura, una función procesa la telemetría y la propiedad *temperatura* del gemelo digital debe actualizarse. Este escenario se describe en un diagrama a continuación:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Diagrama que muestra un gráfico de flujo. En el gráfico, un dispositivo de IoT Hub envía la telemetría de temperatura con IoT Hub a una función de Azure que actualiza una propiedad de temperatura de un gemelo en Azure Digital Twins." border="false":::

## <a name="add-a-model-and-twin"></a>Adición de un modelo y un gemelo

Puede agregar o cargar un modelo mediante el comando siguiente de la CLI y luego crear un gemelo con este modelo que se actualizará con información de IoT Hub.

El modelo tiene este aspecto:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Para **cargar este modelo en la instancia de Digital Twins**, abra el CLI de Azure y ejecute el siguiente comando:

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

A continuación, deberá **crear un gemelo con este modelo**. Use el comando siguiente para crear un gemelo y establezca 0,0 como valor de temperatura inicial.

```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

La salida de un comando de creación de gemelo correcto debe ser similar a la siguiente:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-a-function"></a>Creación de una función

En esta sección se usan los mismos pasos de inicio de Visual Studio y el esqueleto de la función de [*Procedimientos: Configuración de una función para procesar datos*](how-to-create-azure-function.md). El esqueleto controla la autenticación y crea un cliente de servicio, listo para procesar los datos y llamar a las API de Azure Digital Twins como respuesta. 

En los pasos siguientes, agregará código específico para procesar los eventos de telemetría de IoT desde IoT Hub.  

### <a name="add-telemetry-processing"></a>Procesamiento de datos de telemetría
    
Los eventos de telemetría tienen el formato de mensajes del dispositivo. El primer paso para agregar el código de procesamiento de telemetría es extraer la parte pertinente de este mensaje del dispositivo del evento Event Grid. 

Los distintos dispositivos pueden estructurar sus mensajes de forma diferente, por lo que el código de **este paso depende del dispositivo conectado**. 

En el código siguiente se muestra un ejemplo de un dispositivo simple que envía telemetría como JSON. Este ejemplo se explora completamente en [*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md). El código siguiente extrae el identificador de dispositivo del dispositivo que envió el mensaje, así como el valor de temperatura.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Find_device_ID_and_temperature":::

En el ejemplo de código siguiente se toma el valor de identificador y temperatura y se usan para realizar actualizaciones en ese gemelo.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Update_twin_with_device_temperature":::

### <a name="update-your-function-code"></a>Actualización del código de función

Ahora que comprende el código de los ejemplos anteriores, abra la función en la sección [*Requisitos previos*](#prerequisites) de Visual Studio. (Si no tiene una función que se haya creado en Azure, visite el vínculo de los requisitos previos para crear una ahora).

Reemplace el código de la función por este código de ejemplo.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs":::

Guarde el código de la función y publique la aplicación de funciones en Azure. Para más información sobre cómo hacerlo, consulte [*Publicación de la aplicación de funciones*](./how-to-create-azure-function.md#publish-the-function-app-to-azure) en [*Configuración de una función en Azure para procesar datos*](how-to-create-azure-function.md).

Después de una publicación correcta, verá el resultado en la ventana Comandos de Visual Studio, tal y como se muestra a continuación:

```cmd
1>------ Build started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
1>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>------ Publish started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\Out\
2>Publishing C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\adtIngestFunctionSample - 20200911112545669.zip to https://adtingestfunctionsample20200818134346.scm.azurewebsites.net/api/zipdeploy...
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
========== Publish: 1 succeeded, 0 failed, 0 skipped ==========
```
También puede comprobar el estado del proceso de publicación en [Azure Portal](https://portal.azure.com/). Busque el _grupo de recursos_ y navegue a _Registro de actividad_ y busque _Get web app publishing profile_ (Obtener perfil de publicación de aplicaciones web) en la lista y compruebe que el estado es Correcto.

:::image type="content" source="media/how-to-ingest-iot-hub-data/azure-function-publish-activity-log.png" alt-text="Captura de pantalla de Azure Portal que muestra el estado del proceso de publicación.":::

## <a name="connect-your-function-to-iot-hub"></a>Conexión de la función a IoT Hub

Configure un destino de evento para los datos del centro de conectividad.
En [Azure Portal](https://portal.azure.com/), navegue a la instancia de IoT Hub que creó en la sección [*Requisitos previos*](#prerequisites). En **Eventos**, cree una suscripción para su función.

:::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Captura de pantalla de Azure Portal que muestra la adición de una suscripción de evento.":::

En la página **Crear suscripción de eventos**, rellene los campos como se indica a continuación:
  1. En **Nombre**, asigne a la suscripción el nombre que desee.
  2. En **Esquema de eventos**, elija _Esquema de Event Grid_.
  3. En **Tipos de evento**, active la casilla _Telemetría de dispositivo_ y desactive otros tipos de eventos.
  4. En **Tipo de punto de conexión**, seleccione _Función de Azure_.
  5. En **Tipo de punto de conexión**, elija el vínculo _Seleccione un punto de conexión_ para crear un punto de conexión.
    
:::image type="content" source="media/how-to-ingest-iot-hub-data/create-event-subscription.png" alt-text="Captura de pantalla de Azure Portal que muestra los detalles de una suscripción a eventos":::

En la página _Seleccionar Azure Functions_ que se abre, compruebe los detalles siguientes.
 1. **Suscripción**: suscripción de Azure
 2. **Grupo de recursos**: Su grupo de recursos
 3. **Aplicación de función**: Nombre de la aplicación de funciones
 4. **Slot**: _Producción_
 5. **Función**: Seleccione su función en el menú desplegable.

Guarde los detalles seleccionando el botón _Confirmar selección_.            
      
:::image type="content" source="media/how-to-ingest-iot-hub-data/select-azure-function.png" alt-text="Captura de pantalla de Azure Portal para seleccionar la función.":::

Seleccione el botón _Crear_ para crear una suscripción a eventos.

## <a name="send-simulated-iot-data"></a>Envío de datos de IoT simulados

Para probar la nueva función de entrada, use el simulador de dispositivos de [*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md). El tutorial utiliza un proyecto de ejemplo escrito en C#. El código de ejemplo se encuentra aquí: [Ejemplos de Azure Digital Twins de un extremo a otro](/samples/azure-samples/digital-twins-samples/digital-twins-samples). Usará el proyecto **DeviceSimulator** en ese repositorio.

En este tutorial integral, va a completar los siguientes pasos:
1. [*Registro del dispositivo simulado en el centro de IoT*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*Configuración y ejecución de la simulación*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Validación de los resultados

Mientras se ejecuta el simulador de dispositivos anterior, cambiará el valor de temperatura del gemelo digital. En la CLI de Azure, ejecute el siguiente comando para ver el valor de temperatura.

```azurecli-interactive
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

La salida debe contener un valor de temperatura similar al siguiente:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Para ver el cambio de valor, ejecute repetidamente el comando de consulta anterior.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la entrada y salida de datos con Azure Digital Twins:
* [*Conceptos: Integración con otros servicios*](concepts-integration.md)
