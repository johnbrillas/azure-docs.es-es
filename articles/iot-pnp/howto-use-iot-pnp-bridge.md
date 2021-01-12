---
title: Procedimiento para conectar una muestra de puente IoT Plug and Play que se ejecuta en Linux o Windows a una instancia de IoT Hub | Microsoft Docs
description: Compile y ejecute un puente IoT Plug and Play en Linux o Windows que se conecta a un centro de IoT. Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.
author: usivagna
ms.author: ugans
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: bf730dbc28d15c3d036e9ebeedbe035db087c5d8
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97673043"
---
# <a name="how-to-connect-an--iot-plug-and-play-bridge-sample-running-on-linux-or-windows-to-iot-hub"></a>Procedimiento para conectar una muestra de puente de IoT Plug and Play que se ejecuta en Linux o Windows a IoT Hub

En este artículo se muestra cómo compilar el adaptador de muestra de puente IoT Plug and Play, conectarlo al centro de IoT y usar la herramienta Azure IoT Explorer para ver los datos de telemetría que envía. El puente IoT Plug and Play se escribe en C e incluye el SDK de dispositivo IoT de Azure para C. Al final de este tutorial, debería poder ejecutar el puente IoT Plug and Play y ver los informes de telemetría en Azure IoT Explorer:

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-explorer-telemetry.png" alt-text="Captura de pantalla que muestra Azure IoT Explorer con una tabla de telemetría (humedad, temperatura) notificada del puente IoT Plug and Play.":::

## <a name="prerequisites"></a>Requisitos previos

Puede ejecutar el ejemplo en el artículo sobre Windows o Linux. Los comandos de shell de este procedimiento siguen la convención de Windows para los separadores de ruta de acceso "`\`". Si utiliza Linux, asegúrese de cambiar estos separadores por "`/`".

### <a name="azure-iot-explorer"></a>Azure IoT Explorer

Para interactuar con el dispositivo de ejemplo en la segunda parte de este artículo, use la herramienta **Azure IoT Explorer**. [Descargue e instale la versión de Azure IoT Explorer más reciente](./howto-use-iot-explorer.md) para su sistema operativo.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para su centro. Tome nota de esta cadena de conexión, ya que la usará más adelante en este artículo:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

Ejecute el siguiente comando para obtener la _cadena de conexión del dispositivo_ que ha agregado al centro. Tome nota de esta cadena de conexión, ya que la usará más adelante en este artículo:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

## <a name="download-and-run-the-bridge"></a>Descarga y ejecución del puente

En este artículo, tiene dos opciones para ejecutar el puente. Puede:

- Descargar un archivo ejecutable precompilado y ejecutarlo tal y como se describe en esta sección.
- Descargar el código fuente y, posteriormente, [compilar y ejecutar el puente](#build-and-run-the-bridge) tal y como se describe en la sección siguiente.

Para descargar y ejecutar el puente:

1. Vaya a la [página de versiones](https://github.com/Azure/iot-plug-and-play-bridge/releases) de IoT Plug and Play.
1. Descargue el archivo ejecutable precompilado para su sistema operativo: **pnpbridge_bin.exe** para Windows o **pnpbridge_bin** para Linux.
1. Descargue el archivo [config.json](https://raw.githubusercontent.com/Azure/iot-plug-and-play-bridge/master/pnpbridge/src/adapters/samples/environmental_sensor/config.json) de configuración de ejemplo para el ejemplo de sensor ambiental. Asegúrese de que el archivo de configuración esté en la misma carpeta que el ejecutable.
1. Edite el archivo *config.json*:

    - Agregue el valor de `connection-string` que sea la _cadena de conexión del dispositivo_ que anotó anteriormente.
    - Agregue el valor `symmetric_key` que es el valor de clave de acceso compartida de la _cadena de conexión del dispositivo_.
    - Reemplace el valor de `root_interface_model_id` por `dtmi:com:example:PnpBridgeEnvironmentalSensor;1`.

    La primera sección del archivo *config.json* ahora es similar al siguiente fragmento de código:

    ```json
    {
      "$schema": "../../../pnpbridge/src/pnpbridge_config_schema.json",
      "pnp_bridge_connection_parameters": {
        "connection_type" : "connection_string",
        "connection_string" : "HostName=youriothub.azure-devices.net;DeviceId=yourdevice;SharedAccessKey=TTrz8fR7ylHKt7DC/e/e2xocCa5VIcq5x9iQKxKFVa8=",
        "root_interface_model_id": "dtmi:com:example:PnpBridgeEnvironmentalSensor;1",
        "auth_parameters": {
            "auth_type": "symmetric_key",
            "symmetric_key": "TTrz8fR7ylHKt7DC/e/e2xocCa5VIcq5x9iQKxKFVa8="
        },
    ```

1. Ejecute el archivo ejecutable en el entorno de línea de comandos. El puente genera una salida como la siguiente:

    ```output
    c:\temp\temp-bridge>dir
     Volume in drive C is OSDisk
     Volume Serial Number is 38F7-DA4A
    
     Directory of c:\temp\temp-bridge
    
    10/12/2020  12:24    <DIR>          .
    10/12/2020  12:24    <DIR>          ..
    08/12/2020  15:26             1,216 config.json
    10/12/2020  12:21         3,617,280 pnpbridge_bin.exe
                   2 File(s)      3,618,496 bytes
                   2 Dir(s)  12,999,147,520 bytes free
    
    c:\temp\temp-bridge>pnpbridge_bin.exe
    Info:
     -- Press Ctrl+C to stop PnpBridge
    
    Info: Using default configuration location
    Info: Starting Azure PnpBridge
    Info: Pnp Bridge is running as am IoT egde device.
    Info: Pnp Bridge creation succeeded.
    Info: Connection_type is [connection_string]
    Info: Tracing is disabled
    Info: WARNING: SharedAccessKey is included in connection string. Ignoring symmetric_key in config file.
    Info: IoT Edge Device configuration initialized successfully
    Info: Building Pnp Bridge Adapter Manager, Adapters & Components
    Info: Adapter with identity environment-sensor-sample-pnp-adapter does not have any associated global parameters. Proceeding with adapter creation.
    Info: Pnp Adapter with adapter ID environment-sensor-sample-pnp-adapter has been created.
    Info: Pnp Adapter Manager created successfully.
    Info: Pnp components created successfully.
    Info: Pnp components built in model successfully.
    Info: Connected to Azure IoT Hub
    Info: Environmental Sensor: Starting Pnp Component
    Info: IoTHub client call to _SendReportedState succeeded
    Info: Environmental Sensor Adapter:: Sending device information property to IoTHub. propertyName=state, propertyValue=true
    Info: Pnp components started successfully.
    ```

## <a name="build-and-run-the-bridge"></a>Compilación y ejecución del puente

Si prefiere compilar el archivo ejecutable usted mismo, puede descargar el código fuente y los scripts de compilación.

Abra un símbolo del sistema en la carpeta de su elección. Ejecute el siguiente comando para clonar el repositorio de GitHub del [puente IoT Plug and Play](https://github.com/Azure/iot-plug-and-play-bridge) en esta ubicación:

```cmd
git clone https://github.com/Azure/iot-plug-and-play-bridge.git
```

Después de clonar el repositorio, actualice los submódulos. Los submódulos incluyen el SDK de IoT de Azure para C:

```cmd
cd iot-plug-and-play-bridge
git submodule update --init --recursive
```

Esta operación puede tardar varios minutos en completarse.

> [!TIP]
> Si surgen problemas con la actualización del submódulo del clon de Git, se trata de un problema conocido con las rutas de acceso de archivos de Windows. Para resolver el problema, puede probar el comando siguiente: `git config --system core.longpaths true`

Los requisitos previos para la creación del puente varían según el sistema operativo:

### <a name="windows"></a>Windows

Para compilar el puente IoT Plug and Play en Windows, instale el software siguiente:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir la carga de trabajo **Desarrollo para el escritorio con C++** cuando [instale](/cpp/build/vscpp-step-0-installation?preserve-view=true&view=vs-2019) Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="linux"></a>Linux

En este artículo se presupone que usa Ubuntu Linux. Los pasos de este artículo se han probado con Ubuntu 18.04.

Para compilar el puente IoT Plug and Play en Linux, instale **GCC**, **Git**, **cmake** y todas las dependencias necesarias con el comando `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Compruebe que la versión de `cmake` es posterior a **2.8.12** y que la versión de **GCC** es posterior a **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="build-the-iot-plug-and-play-bridge"></a>Compilación del puente IoT Plug and Play

Navegue hasta la carpeta *pnpbridge* en el directorio del repositorio.

En Windows, ejecute lo siguiente en un [Símbolo del sistema para desarrolladores para Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs):

```cmd
cd scripts\windows
build.cmd
```

Del mismo modo, para Linux, ejecute el siguiente comando:

```bash
cd scripts/linux
./setup.sh
./build.sh
```

>[!TIP]
>En Windows, puede abrir la solución generada por el comando cmake en Visual Studio 2019. Abra el archivo de proyecto *azure_iot_pnp_bridge.sln* en el directorio cmake y establezca el proyecto *pnpbridge_bin* como proyecto de inicio en la solución. Ahora puede compilar el ejemplo en Visual Studio y ejecutarlo en modo de depuración.

### <a name="edit-the-configuration-file"></a>Editar el archivo de configuración

Puede obtener más información acerca de los archivos de configuración en el [documento sobre conceptos de puente IoT Plug and Play](concepts-iot-pnp-bridge.md).

Abra el archivo *iot-plug-and-play-bridge\pnpbridge\src\adapters\samples\environmental_sensor\config.json* en un editor de texto.

- Agregue el valor de `connection-string` que sea la _cadena de conexión del dispositivo_ que anotó anteriormente.
- Agregue el valor `symmetric_key` que es el valor de clave de acceso compartida de la _cadena de conexión del dispositivo_.
- Reemplace el valor de `root_interface_model_id` por `dtmi:com:example:PnpBridgeEnvironmentalSensor;1`.

La primera sección del archivo *config.json* ahora es similar al siguiente fragmento de código:

```json
{
  "$schema": "../../../pnpbridge/src/pnpbridge_config_schema.json",
  "pnp_bridge_connection_parameters": {
    "connection_type" : "connection_string",
    "connection_string" : "HostName=youriothub.azure-devices.net;DeviceId=yourdevice;SharedAccessKey=TTrz8fR7ylHKt7DC/e/e2xocCa5VIcq5x9iQKxKFVa8=",
    "root_interface_model_id": "dtmi:com:example:PnpBridgeEnvironmentalSensor;1",
    "auth_parameters": {
        "auth_type": "symmetric_key",
        "symmetric_key": "TTrz8fR7ylHKt7DC/e/e2xocCa5VIcq5x9iQKxKFVa8="
    },
```

### <a name="run-the-iot-plug-and-play-bridge"></a>Ejecución del puente IoT Plug and Play

Inicie el ejemplo de sensor ambiental de puente IoT Plug and Play Bridge. El parámetro es la ruta de acceso al archivo `config.json` que editó en la sección anterior:

```cmd
REM Windows
cd iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console
Debug\pnpbridge_bin.exe ..\..\..\..\..\..\src\adapters\samples\environmental_sensor\config.json
```

El puente genera una salida como la siguiente:

```output
c:\temp>cd iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console

c:\temp\iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console>Debug\pnpbridge_bin.exe ..\..\..\..\..\..\src\adapters\samples\environmental_sensor\config.json
Info:
 -- Press Ctrl+C to stop PnpBridge

Info: Using configuration from specified file path: ..\..\..\..\..\..\src\adapters\samples\environmental_sensor\config.json
Info: Starting Azure PnpBridge
Info: Pnp Bridge is running as am IoT egde device.
Info: Pnp Bridge creation succeeded.
Info: Connection_type is [connection_string]
Info: Tracing is disabled
Info: WARNING: SharedAccessKey is included in connection string. Ignoring symmetric_key in config file.
Info: IoT Edge Device configuration initialized successfully
Info: Building Pnp Bridge Adapter Manager, Adapters & Components
Info: Adapter with identity environment-sensor-sample-pnp-adapter does not have any associated global parameters. Proceeding with adapter creation.
Info: Pnp Adapter with adapter ID environment-sensor-sample-pnp-adapter has been created.
Info: Pnp Adapter Manager created successfully.
Info: Pnp components created successfully.
Info: Pnp components built in model successfully.
Info: Connected to Azure IoT Hub
Info: Environmental Sensor: Starting Pnp Component
Info: IoTHub client call to _SendReportedState succeeded
Info: Environmental Sensor Adapter:: Sending device information property to IoTHub. propertyName=state, propertyValue=true
Info: Pnp components started successfully.
Info: IoTHub client call to _SendEventAsync succeeded
Info: PnpBridge_PnpBridgeStateTelemetryCallback called, result=0, telemetry=PnpBridge configuration complete
Info: Processing property update for the device or module twin
Info: Environmental Sensor Adapter:: Successfully delivered telemetry message for <environmentalSensor>
```

Ejecute los siguientes comandos para ejecutar el puente en Linux:

```bash
cd iot-plug-and-play-bridge/pnpbridge/cmake/pnpbridge_x86/src/pnpbridge/samples/console
./pnpbridge_bin ../../../../../../src/adapters/samples/environmental_sensor/config.json
```

## <a name="download-the-model-files"></a>Descarga de los archivos de modelo

Más adelante, usará Azure IoT Explorer para ver el dispositivo cuando se conecte al centro de IoT. Azure IoT Explorer necesita una copia local del archivo de modelo que coincida con el **identificador de modelo** que envía el dispositivo. El archivo de modelo permite que Azure IoT Explorer muestre los datos de telemetría, las propiedades y los comandos que implementa el dispositivo.

Para descargar los modelos para Azure IoT Explorer:

1. Cree una carpeta denominada *models* en la máquina local.
1. Guarde [EnvironmentalSensor.json](https://raw.githubusercontent.com/Azure/iot-plug-and-play-bridge/master/pnpbridge/docs/schemas/EnvironmentalSensor.json) en la carpeta *models* que creó en el paso anterior.
1. Si abre este archivo de modelo en un editor de texto, puede ver que el modelo define un componente con `dtmi:com:example:PnpBridgeEnvironmentalSensor;1` como su identificador. Este es el mismo identificador de modelo que usó en el archivo *config.json*.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

Una vez iniciado el puente, compruebe que funciona con la herramienta Azure IoT Explorer. Puede ver la telemetría, las propiedades y los comandos definidos en el modelo `dtmi:com:example:PnpBridgeEnvironmentalSensor;1`.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

* [¿Qué es un puente IoT Plug and Play?](./concepts-iot-pnp-bridge.md)
* [Compilación, implementación y ampliación del puente IoT Plug and Play](howto-build-deploy-extend-pnp-bridge.md)
* [Puente IoT Plug and Play en GitHub](https://github.com/Azure/iot-plug-and-play-bridge)
