---
title: Compilación de un adaptador para el puente IoT Plug and Play | Microsoft Docs
description: Identifique los componentes del adaptador del puente IoT Plug and Play. Para obtener información acerca de cómo ampliar el puente, escriba su propio adaptador.
author: usivagna
ms.author: ugans
ms.date: 1/20/2021
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 9a7028dfaeb94e87366de7acfa8cebc4c2f4c767
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98746814"
---
# <a name="extend-the-iot-plug-and-play-bridge"></a>Extensión del puente IoT Plug and Play
El [puente IoT Plug and Play](concepts-iot-pnp-bridge.md#iot-plug-and-play-bridge-architecture) le permite conectar los dispositivos existentes conectados a una puerta de enlace a su centro de IoT. El puente se usa para asignar interfaces IoT Plug and Play a los dispositivos conectados. Una interfaz IoT Plug and Play define la telemetría que un dispositivo envía, las propiedades sincronizadas entre el dispositivo y la nube, y los comandos a los que responde el dispositivo. Puede instalar y configurar la aplicación de puente de código abierto en puertas de enlace de Windows o Linux. Además, el puente puede ejecutarse como un módulo de entorno de ejecución de Azure IoT Edge.

En este artículo se explica detalladamente lo siguiente:

- Extensión del puente IoT Plug and Play con un adaptador.
- Implementación de devoluciones de llamada comunes para un adaptador de puente.

Para ver un ejemplo sencillo en el que se muestra cómo usar el puente, vea [Procedimiento para conectar el ejemplo de puente IoT Plug and Play que se ejecuta en Linux o Windows a IoT Hub](howto-use-iot-pnp-bridge.md).

En las instrucciones y los ejemplos de este artículo se presupone que está familiarizado con [Azure Digital Twins](../digital-twins/overview.md) y [IoT Plug and Play](overview-iot-plug-and-play.md). Además, en este artículo se da por hecho que está familiarizado con la [compilación e implementación del puente IoT Plug and Play](howto-build-deploy-extend-pnp-bridge.md).

## <a name="design-guide-to-extend-the-iot-plug-and-play-bridge-with-an-adapter"></a>Guía de diseño para extender el puente IoT Plug and Play con un adaptador

Para extender las capacidades del puente, puede crear sus propios adaptadores de puente.

El puente utiliza adaptadores para:

- Establecer una conexión entre un dispositivo y la nube.
- Habilitar el flujo de datos entre un dispositivo y la nube.
- Habilitar la administración de dispositivos desde la nube.

Cada adaptador de puente debe:

- Crear una interface de gemelos digitales.
- Use la interfaz para enlazar la funcionalidad del lado del dispositivo a capacidades basadas en la nube tales como telemetría, propiedades y comandos.
- Establecer la comunicación de datos y control con el hardware o el firmware del dispositivo.

Cada adaptador de puente interactúa con un tipo de dispositivo específico en función de cómo el adaptador se conecta al dispositivo e interactúa con él. Aunque la comunicación con un dispositivo utilice un protocolo de enlace, es posible que un adaptador de puente tenga varias formas de interpretar los datos del dispositivo. En este escenario, el adaptador de puente usa información del adaptador del archivo de configuración a fin de determinar la *configuración de la interfaz* que debe usar el adaptador para analizar los datos.

Para interactuar con el dispositivo, un adaptador de puente utiliza un protocolo de comunicación compatible con el dispositivo y las API proporcionadas por el sistema operativo subyacente o por el proveedor del dispositivo.

Para interactuar con la nube, un adaptador de puente usa las API proporcionadas por el SDK de C de dispositivo Azure IoT para enviar telemetría, crear interfaces de gemelo digital, enviar actualizaciones de propiedades y crear funciones de devolución de llamada para las actualizaciones de propiedades y los comandos.

### <a name="create-a-bridge-adapter"></a>Creación de un adaptador de puente

El puente espera un adaptador de puente para implementar las API definidas en la interfaz [_PNP_ADAPTER](https://github.com/Azure/iot-plug-and-play-bridge/blob/9964f7f9f77ecbf4db3b60960b69af57fd83a871/pnpbridge/src/pnpbridge/inc/pnpadapter_api.h#L296):

```c
typedef struct _PNP_ADAPTER {
  // Identity of the IoT Plug and Play adapter that is retrieved from the config
  const char* identity;

  PNPBRIDGE_ADAPTER_CREATE createAdapter;
  PNPBRIDGE_COMPONENT_CREATE createPnpComponent;
  PNPBRIDGE_COMPONENT_START startPnpComponent;
  PNPBRIDGE_COMPONENT_STOP stopPnpComponent;
  PNPBRIDGE_COMPONENT_DESTROY destroyPnpComponent;
  PNPBRIDGE_ADAPTER_DESTOY destroyAdapter;
} PNP_ADAPTER, * PPNP_ADAPTER;
```

En esta interfaz:

- `PNPBRIDGE_ADAPTER_CREATE` crea el adaptador y configura los recursos de administración de la interfaz. También es posible que un adaptador se base en los parámetros de adaptador globales para la creación de un adaptador. Esta función se llama una vez para un único adaptador.
- `PNPBRIDGE_COMPONENT_CREATE` crea las interfaces cliente de gemelo digital y enlaza las funciones de devolución de llamada. El adaptador inicia el canal de comunicación con el dispositivo. El adaptador puede configurar los recursos para habilitar el flujo de telemetría, pero no inicia los informes de telemetría hasta que se llama a `PNPBRIDGE_COMPONENT_START`. Esta función se llama una vez para cada componente de interfaz en el archivo de configuración.
- `PNPBRIDGE_COMPONENT_START` se llama para permitir que el adaptador de puente empiece a reenviar la telemetría del dispositivo al cliente de gemelo digital. Esta función se llama una vez para cada componente de interfaz en el archivo de configuración.
- `PNPBRIDGE_COMPONENT_STOP` detiene el flujo de telemetría.
- `PNPBRIDGE_COMPONENT_DESTROY` destruye el cliente de gemelo digital y los recursos de interfaz asociados. Esta función se llama una vez para cada componente de interfaz en el archivo de configuración cuando se revoca el puente o cuando se produce un error irrecuperable.
- `PNPBRIDGE_ADAPTER_DESTROY` limpia los recursos del adaptador de puente.

### <a name="bridge-core-interaction-with-bridge-adapters"></a>Interacción del núcleo del puente con adaptadores de puente

En la siguiente lista se describe lo que sucede cuando se inicia el puente:

1. Cuando se inicia el puente, el administrador de adaptadores de puente examina cada componente de interfaz definido en el archivo de configuración y llama a `PNPBRIDGE_ADAPTER_CREATE` en el adaptador adecuado. El adaptador puede usar parámetros de configuración de adaptador globales para configurar los recursos a fin de admitir las diversas *configuraciones de interfaz*.
1. Para cada dispositivo del archivo de configuración, el administrador de puente inicia la creación de la interfaz mediante una llamada a `PNPBRIDGE_COMPONENT_CREATE` en el adaptador de puente adecuado.
1. El adaptador recibe cualquier valor de configuración de adaptador opcional para el componente de interfaz y usa esta información para configurar las conexiones con el dispositivo.
1. El adaptador crea las interfaces cliente de gemelo digital y enlaza las funciones de devolución de llamada para las actualizaciones de propiedades y los comandos. El establecimiento de conexiones de dispositivo no debe bloquear la devolución de las devoluciones de llamada después de que la creación de la interfaz de gemelo digital se realice correctamente. La conexión del dispositivo activo es independiente del cliente de interfaz activa que crea el puente. Si se produce un error en una conexión, el adaptador supone que el dispositivo está inactivo. El adaptador de puente puede optar por reintentar establecer esta conexión.
1. Después de que el administrador de adaptadores de puente cree todos los componentes de interfaz especificados en el archivo de configuración, registra todas las interfaces con Azure IoT Hub. El registro es una llamada asincrónica de bloqueo. Cuando se completa la llamada, se desencadena una devolución de llamada en el adaptador de puente que puede empezar a administrar las devoluciones de llamada de propiedades y comandos desde la nube.
1. El administrador de adaptadores de puente llama entonces a `PNPBRIDGE_INTERFACE_START` en cada componente y el adaptador de puente comienza a notificar la telemetría al cliente de gemelo digital.

### <a name="design-guidelines"></a>Guías de diseño

Siga estas instrucciones cuando desarrolle un nuevo adaptador de puente:

- Determine qué capacidades de los dispositivos se admiten y cuál es la definición de interfaz de los componentes que utilizan este adaptador.
- Determine qué parámetros de interfaz y globales necesita su adaptador que se definan en el archivo de configuración.
- Identifique la comunicación de dispositivo de bajo nivel necesaria para admitir las propiedades y los comandos de los componentes.
- Determine cómo debe el adaptador analizar los datos sin procesar del dispositivo y convertirlos en los tipos de telemetría que especifica la definición de la interfaz IoT Plug and Play.
- Implemente la interfaz del adaptador de puente descrita anteriormente.
- Agregue el nuevo adaptador al manifiesto de aplicación del adaptador y compile el puente.

### <a name="enable-a-new-bridge-adapter"></a>Habilitación de un nuevo adaptador de puente

Los adaptadores se habilitan en el puente agregando una referencia en [adapter_manifest.c](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/shared/adapter_manifest.c):

```c
  extern PNP_ADAPTER MyPnpAdapter;
  PPNP_ADAPTER PNP_ADAPTER_MANIFEST[] = {
    .
    .
    &MyPnpAdapter
  }
```

> [!IMPORTANT]
> Las devoluciones de llamada del adaptador de puente se invocan secuencialmente. Un adaptador no debe bloquear una devolución de llamada porque impide que el núcleo del puente avance.

### <a name="sample-camera-adapter"></a>Adaptador de cámara de ejemplo

El [archivo Léame del adaptador de cámara](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/Camera/readme.md) describe un adaptador de cámara de ejemplo que puede habilitar.

## <a name="code-examples-for-common-adapter-scenarioscallbacks"></a>Ejemplos de código para escenarios de adaptador comunes o devoluciones de llamada

En la siguiente sección se proporcionan detalles sobre cómo un adaptador para el puente implementaría las devoluciones de llamada para una serie de escenarios y usos comunes. En esta sección se tratan las siguientes devoluciones de llamada:
- [Recepción de la actualización de propiedades (de la nube al dispositivo)](#receive-property-update-cloud-to-device)
- [Notificación de la actualización de propiedades (del dispositivo a la nube)](#report-a-property-update-device-to-cloud)
- [Envío de telemetría (del dispositivo a la nube)](#send-telemetry-device-to-cloud)
- [Recepción de la devolución de llamada de actualización de comandos de la nube y su procesamiento en el lado del dispositivo (de la nube al dispositivo)](#receive-command-update-callback-from-the-cloud-and-process-it-on-the-device-side-cloud-to-device)
- [Respuesta a la actualización de comandos en el lado del dispositivo (del dispositivo a la nube)](#respond-to-command-update-on-the-device-side-device-to-cloud)

Los ejemplos anteriores se basan en el [adaptador de ejemplo del sensor del entorno](https://github.com/Azure/iot-plug-and-play-bridge/tree/master/pnpbridge/src/adapters/samples/environmental_sensor).

### <a name="receive-property-update-cloud-to-device"></a>Recepción de la actualización de propiedades (de la nube al dispositivo)
El primer paso es registrar una función de devolución de llamada:

```c
PnpComponentHandleSetPropertyUpdateCallback(BridgeComponentHandle, EnvironmentSensor_ProcessPropertyUpdate);
```
El siguiente paso consiste en implementar la función de devolución de llamada para leer la actualización de la propiedad en el dispositivo:

```c
void EnvironmentSensor_ProcessPropertyUpdate(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    void* userContextCallback
)
{
  // User context for the callback is set to the IoT Hub client handle, and therefore can be type-cast to the client handle type
    SampleEnvironmentalSensor_ProcessPropertyUpdate(userContextCallback, PropertyName, PropertyValue, version, PnpComponentHandle);
}

// SampleEnvironmentalSensor_ProcessPropertyUpdate receives updated properties from the server.  This implementation
// acts as a simple dispatcher to the functions to perform the actual processing.
void SampleEnvironmentalSensor_ProcessPropertyUpdate(
    void * ClientHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
  if (strcmp(PropertyName, sampleEnvironmentalSensorPropertyBrightness) == 0)
    {
        SampleEnvironmentalSensor_BrightnessCallback(ClientHandle, PropertyName, PropertyValue, version, PnpComponentHandle);
    }
    else
    {
        // If the property is not implemented by this interface, presently we only record a log message but do not have a mechanism to report back to the service
        LogError("Environmental Sensor Adapter:: Property name <%s> is not associated with this interface", PropertyName);
    }
}

// Process a property update for bright level.
static void SampleEnvironmentalSensor_BrightnessCallback(
    void * ClientHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
    IOTHUB_CLIENT_RESULT iothubClientResult;
    STRING_HANDLE jsonToSend = NULL;
    char targetBrightnessString[32];

    LogInfo("Environmental Sensor Adapter:: Brightness property invoked...");

    PENVIRONMENT_SENSOR EnvironmentalSensor = PnpComponentHandleGetContext(PnpComponentHandle);

    if (json_value_get_type(PropertyValue) != JSONNumber)
    {
        LogError("JSON field %s is not a number", PropertyName);
    }
    else if(EnvironmentalSensor == NULL || EnvironmentalSensor->SensorState == NULL)
    {
        LogError("Environmental sensor device context not initialized correctly.");
    }
    else if (SampleEnvironmentalSensor_ValidateBrightness(json_value_get_number(PropertyValue)))
    {
        EnvironmentalSensor->SensorState->brightness = (int) json_value_get_number(PropertyValue);
        if (snprintf(targetBrightnessString, sizeof(targetBrightnessString), 
            g_environmentalSensorBrightnessResponseFormat, EnvironmentalSensor->SensorState->brightness) < 0)
        {
            LogError("Unable to create target brightness string for reporting result");
        }
        else if ((jsonToSend = PnP_CreateReportedPropertyWithStatus(EnvironmentalSensor->SensorState->componentName,
                    PropertyName, targetBrightnessString, PNP_STATUS_SUCCESS, g_environmentalSensorPropertyResponseDescription,
                    version)) == NULL)
        {
            LogError("Unable to build reported property response");
        }
        else
        {
            const char* jsonToSendStr = STRING_c_str(jsonToSend);
            size_t jsonToSendStrLen = strlen(jsonToSendStr);

            if ((iothubClientResult = SampleEnvironmentalSensor_RouteReportedState(ClientHandle, PnpComponentHandle, (const unsigned char*)jsonToSendStr, jsonToSendStrLen,
                                        SampleEnvironmentalSensor_PropertyCallback,
                                        (void*) &EnvironmentalSensor->SensorState->brightness)) != IOTHUB_CLIENT_OK)
            {
                LogError("Environmental Sensor Adapter:: SampleEnvironmentalSensor_RouteReportedState for brightness failed, error=%d", iothubClientResult);
            }
            else
            {
                LogInfo("Environmental Sensor Adapter:: Successfully queued Property update for Brightness for component=%s", EnvironmentalSensor->SensorState->componentName);
            }

            STRING_delete(jsonToSend);
        }
    }
}

```

### <a name="report-a-property-update-device-to-cloud"></a>Notificación de la actualización de propiedades (del dispositivo a la nube)
En cualquier momento después de crear el componente, el dispositivo puede notificar las propiedades a la nube con el estado: 
```c
// Environmental sensor's read-only property, device state indiciating whether its online or not
//
static const char sampleDeviceStateProperty[] = "state";
static const unsigned char sampleDeviceStateData[] = "true";
static const int sampleDeviceStateDataLen = sizeof(sampleDeviceStateData) - 1;

// Sends a reported property for device state of this simulated device.
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_ReportDeviceStateAsync(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const char * ComponentName)
{

    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;
    STRING_HANDLE jsonToSend = NULL;

    if ((jsonToSend = PnP_CreateReportedProperty(ComponentName, sampleDeviceStateProperty, (const char*) sampleDeviceStateData)) == NULL)
    {
        LogError("Unable to build reported property response for propertyName=%s, propertyValue=%s", sampleDeviceStateProperty, sampleDeviceStateData);
    }
    else
    {
        const char* jsonToSendStr = STRING_c_str(jsonToSend);
        size_t jsonToSendStrLen = strlen(jsonToSendStr);

        if ((iothubClientResult = SampleEnvironmentalSensor_RouteReportedState(NULL, PnpComponentHandle, (const unsigned char*)jsonToSendStr, jsonToSendStrLen,
            SampleEnvironmentalSensor_PropertyCallback, (void*)sampleDeviceStateProperty)) != IOTHUB_CLIENT_OK)
        {
            LogError("Environmental Sensor Adapter:: Unable to send reported state for property=%s, error=%d",
                                sampleDeviceStateProperty, iothubClientResult);
        }
        else
        {
            LogInfo("Environmental Sensor Adapter:: Sending device information property to IoTHub. propertyName=%s, propertyValue=%s",
                        sampleDeviceStateProperty, sampleDeviceStateData);
        }

        STRING_delete(jsonToSend);
    }

    return iothubClientResult;
}


// Routes the reported property for device or module client. This function can be called either by passing a valid client handle or by passing
// a NULL client handle after components have been started such that the client handle can be extracted from the PnpComponentHandle
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_RouteReportedState(
    void * ClientHandle,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const unsigned char * ReportedState,
    size_t Size,
    IOTHUB_CLIENT_REPORTED_STATE_CALLBACK ReportedStateCallback,
    void * UserContextCallback)
{
    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;

    PNP_BRIDGE_CLIENT_HANDLE clientHandle = (ClientHandle != NULL) ?
            (PNP_BRIDGE_CLIENT_HANDLE) ClientHandle : PnpComponentHandleGetClientHandle(PnpComponentHandle);

    if ((iothubClientResult = PnpBridgeClient_SendReportedState(clientHandle, ReportedState, Size,
            ReportedStateCallback, UserContextCallback)) != IOTHUB_CLIENT_OK)
    {
        LogError("IoTHub client call to _SendReportedState failed with error code %d", iothubClientResult);
        goto exit;
    }
    else
    {
        LogInfo("IoTHub client call to _SendReportedState succeeded");
    }

exit:
    return iothubClientResult;
}

```

### <a name="send-telemetry-device-to-cloud"></a>Envío de telemetría (del dispositivo a la nube)
```c
//
// SampleEnvironmentalSensor_SendTelemetryMessagesAsync is periodically invoked by the caller to
// send telemetry containing the current temperature and humidity (in both cases random numbers
// so this sample will work on platforms without these sensors).
//
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_SendTelemetryMessagesAsync(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
    IOTHUB_CLIENT_RESULT result = IOTHUB_CLIENT_OK;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
    PENVIRONMENT_SENSOR device = PnpComponentHandleGetContext(PnpComponentHandle);

    float currentTemperature = 20.0f + ((float)rand() / RAND_MAX) * 15.0f;
    float currentHumidity = 60.0f + ((float)rand() / RAND_MAX) * 20.0f;

    char currentMessage[128];
    sprintf(currentMessage, "{\"%s\":%.3f, \"%s\":%.3f}", SampleEnvironmentalSensor_TemperatureTelemetry, 
            currentTemperature, SampleEnvironmentalSensor_HumidityTelemetry, currentHumidity);


    if ((messageHandle = PnP_CreateTelemetryMessageHandle(device->SensorState->componentName, currentMessage)) == NULL)
    {
        LogError("Environmental Sensor Adapter:: PnP_CreateTelemetryMessageHandle failed.");
    }
    else if ((result = SampleEnvironmentalSensor_RouteSendEventAsync(PnpComponentHandle, messageHandle,
            SampleEnvironmentalSensor_TelemetryCallback, device)) != IOTHUB_CLIENT_OK)
    {
        LogError("Environmental Sensor Adapter:: SampleEnvironmentalSensor_RouteSendEventAsync failed, error=%d", result);
    }

    IoTHubMessage_Destroy(messageHandle);

    return result;
}

// Routes the sending asynchronous events for device or module client
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_RouteSendEventAsync(
        PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
        IOTHUB_MESSAGE_HANDLE EventMessageHandle,
        IOTHUB_CLIENT_EVENT_CONFIRMATION_CALLBACK EventConfirmationCallback,
        void * UserContextCallback)
{
    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;
    PNP_BRIDGE_CLIENT_HANDLE clientHandle = PnpComponentHandleGetClientHandle(PnpComponentHandle);
    if ((iothubClientResult = PnpBridgeClient_SendEventAsync(clientHandle, EventMessageHandle,
            EventConfirmationCallback, UserContextCallback)) != IOTHUB_CLIENT_OK)
    {
        LogError("IoTHub client call to _SendEventAsync failed with error code %d", iothubClientResult);
        goto exit;
    }
    else
    {
        LogInfo("IoTHub client call to _SendEventAsync succeeded");
    }

exit:
    return iothubClientResult;
}

```
### <a name="receive-command-update-callback-from-the-cloud-and-process-it-on-the-device-side-cloud-to-device"></a>Recepción de la devolución de llamada de actualización de comandos de la nube y su procesamiento en el lado del dispositivo (de la nube al dispositivo)
```c
// SampleEnvironmentalSensor_ProcessCommandUpdate receives commands from the server.  This implementation acts as a simple dispatcher
// to the functions to perform the actual processing.
int SampleEnvironmentalSensor_ProcessCommandUpdate(
    PENVIRONMENT_SENSOR EnvironmentalSensor,
    const char* CommandName,
    JSON_Value* CommandValue,
    unsigned char** CommandResponse,
    size_t* CommandResponseSize)
{
    if (strcmp(CommandName, sampleEnvironmentalSensorCommandBlink) == 0)
    {
        return SampleEnvironmentalSensor_BlinkCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else if (strcmp(CommandName, sampleEnvironmentalSensorCommandTurnOn) == 0)
    {
        return SampleEnvironmentalSensor_TurnOnLightCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else if (strcmp(CommandName, sampleEnvironmentalSensorCommandTurnOff) == 0)
    {
        return SampleEnvironmentalSensor_TurnOffLightCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else
    {
        // If the command is not implemented by this interface, by convention we return a 404 error to server.
        LogError("Environmental Sensor Adapter:: Command name <%s> is not associated with this interface", CommandName);
        return SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_NotImplemented);
    }
}

// Implement the callback to process the command "blink". Information pertaining to the request is
// specified in the CommandValue parameter, and the callback fills out data it wishes to
// return to the caller on the service in CommandResponse.

static int SampleEnvironmentalSensor_BlinkCallback(
    PENVIRONMENT_SENSOR EnvironmentalSensor,
    JSON_Value* CommandValue,
    unsigned char** CommandResponse,
    size_t* CommandResponseSize)
{
    int result = PNP_STATUS_SUCCESS;
    int BlinkInterval = 0;

    LogInfo("Environmental Sensor Adapter:: Blink command invoked. It has been invoked %d times previously", EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled);

    if (json_value_get_type(CommandValue) != JSONNumber)
    {
        LogError("Cannot retrieve blink interval for blink command");
        result = PNP_STATUS_BAD_FORMAT;
    }
    else
    {
        BlinkInterval = (int)json_value_get_number(CommandValue);
        LogInfo("Environmental Sensor Adapter:: Blinking with interval=%d second(s)", BlinkInterval);
        EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled++;
        EnvironmentalSensor->SensorState->blinkInterval = BlinkInterval;

        result = SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_BlinkResponse);
    }

    return result;
}

```
### <a name="respond-to-command-update-on-the-device-side-device-to-cloud"></a>Respuesta a la actualización de comandos en el lado del dispositivo (del dispositivo a la nube)

```c
    static int SampleEnvironmentalSensor_BlinkCallback(
        PENVIRONMENT_SENSOR EnvironmentalSensor,
        JSON_Value* CommandValue,
        unsigned char** CommandResponse,
        size_t* CommandResponseSize)
    {
        int result = PNP_STATUS_SUCCESS;
        int BlinkInterval = 0;
    
        LogInfo("Environmental Sensor Adapter:: Blink command invoked. It has been invoked %d times previously", EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled);
    
        if (json_value_get_type(CommandValue) != JSONNumber)
        {
            LogError("Cannot retrieve blink interval for blink command");
            result = PNP_STATUS_BAD_FORMAT;
        }
        else
        {
            BlinkInterval = (int)json_value_get_number(CommandValue);
            LogInfo("Environmental Sensor Adapter:: Blinking with interval=%d second(s)", BlinkInterval);
            EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled++;
            EnvironmentalSensor->SensorState->blinkInterval = BlinkInterval;
    
            result = SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_BlinkResponse);
        }
    
        return result;
    }
    
    // SampleEnvironmentalSensor_SetCommandResponse is a helper that fills out a command response
    static int SampleEnvironmentalSensor_SetCommandResponse(
        unsigned char** CommandResponse,
        size_t* CommandResponseSize,
        const unsigned char* ResponseData)
    {
        int result = PNP_STATUS_SUCCESS;
        if (ResponseData == NULL)
        {
            LogError("Environmental Sensor Adapter:: Response Data is empty");
            *CommandResponseSize = 0;
            return PNP_STATUS_INTERNAL_ERROR;
        }
    
        *CommandResponseSize = strlen((char*)ResponseData);
        memset(CommandResponse, 0, sizeof(*CommandResponse));
    
        // Allocate a copy of the response data to return to the invoker. Caller will free this.
        if (mallocAndStrcpy_s((char**)CommandResponse, (char*)ResponseData) != 0)
        {
            LogError("Environmental Sensor Adapter:: Unable to allocate response data");
            result = PNP_STATUS_INTERNAL_ERROR;
        }
    
        return result;
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el puente IoT Plug and Play, visite el repositorio del [puente IoT Plug and Play](https://github.com/Azure/iot-plug-and-play-bridge) de GitHub.
