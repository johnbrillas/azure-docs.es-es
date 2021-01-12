---
title: Compilación, implementación y extensión del puente IoT Plug and Play | Microsoft Docs
description: Identifique los componentes del puente IoT Plug and Play. Aprenda a extender el puente y a ejecutarlo en dispositivos IoT, puertas de enlace y como módulo IoT Edge.
author: usivagna
ms.author: ugans
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: ece9f62e64eb64b1f34af46b42d57ec583f8f214
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97675739"
---
# <a name="build-deploy-and-extend-the-iot-plug-and-play-bridge"></a>Compilación, implementación y extensión del puente IoT Plug and Play

El puente IoT Plug and Play le permite conectar los dispositivos existentes conectados a una puerta de enlace a su centro de IoT. El puente se usa para asignar interfaces IoT Plug and Play a los dispositivos conectados. Una interfaz IoT Plug and Play define la telemetría que un dispositivo envía, las propiedades sincronizadas entre el dispositivo y la nube, y los comandos a los que responde el dispositivo. Puede instalar y configurar la aplicación de puente de código abierto en puertas de enlace de Windows o Linux.

En este artículo se explica detalladamente lo siguiente:

- Configuración de un puente.
- Extensión de un puente mediante la creación de adaptadores.
- Procedimiento para compilar y ejecutar el puente en diversos entornos.

Para ver un ejemplo sencillo en el que se muestra cómo usar el puente, vea [Procedimiento para conectar el ejemplo de puente IoT Plug and Play que se ejecuta en Linux o Windows a IoT Hub](howto-use-iot-pnp-bridge.md).

En las instrucciones y los ejemplos de este artículo se presupone que está familiarizado con [Azure Digital Twins](../digital-twins/overview.md) y [IoT Plug and Play](overview-iot-plug-and-play.md).

## <a name="general-prerequisites"></a>Requisitos previos generales

### <a name="supported-os-platforms"></a>Plataformas de sistema operativo admitidas

Se admiten las siguientes plataformas y versiones de SO:

|Plataforma  |Versiones admitidas  |
|---------|---------|
|Windows 10 |     Se admiten todas las SKU de Windows. Por ejemplo: IoT Enterprise, Server, Desktop y IoT Core. *Para la funcionalidad de seguimiento de estado de la cámara, se recomienda 20H1 o una versión posterior. El resto de funcionalidades están disponibles en todas las compilaciones de Windows 10.*  |
|Linux     |Esta funcionalidad, probada y admitida en Ubuntu 18.04, no se ha probado en otras distribuciones.         |
||

### <a name="hardware"></a>Hardware

- Cualquier plataforma de hardware que admita las SKU y versiones anteriores del sistema operativo.
- Los sensores y periféricos de serie, USB, Bluetooth y cámara se admiten de forma nativa. El puente IoT Plug and Play se puede extender para admitir cualquier periférico o sensor personalizado.

### <a name="azure-iot-products-and-tools"></a>Productos y herramientas de Azure IoT

- **Azure IoT Hub**: necesitará una instancia de [Azure IoT Hub](../iot-hub/index.yml) en su suscripción de Azure para conectar el dispositivo. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. Si no tiene una instancia de IoT Hub, siga [estas instrucciones para crear una](../iot-hub/iot-hub-create-using-cli.md). El soporte técnico de IoT Plug and Play no está incluido en los centros de IoT de nivel básico.
- **Azure IoT Explorer**: para interactuar con el dispositivo IoT Plug and Play, puede usar la herramienta Azure IoT Explorer. [Descargue e instale la versión de Azure IoT Explorer más reciente](./howto-use-iot-explorer.md) para su sistema operativo. Configure la herramienta Azure IoT Explorer para conectarse a su instancia de IoT Hub y buscar definiciones de modelo en la carpeta *pnpbridge\docs\schemas* del **iot-plug-and-play-bridge** que clonó.

## <a name="configure-a-bridge"></a>Configuración de un puente

Existen dos formas de configurar el puente:

- Si el puente IoT Plug and Play se ejecuta de forma nativa en un dispositivo o puerta de enlace IoT, use el archivo de configuración. En este escenario se puede usar una máquina Linux o Windows.
- Si el puente se ejecuta como módulo IoT Edge en el entorno de ejecución de Azure IoT Edge, use la propiedad de módulo gemelo deseada. En este escenario se supone que el entorno de ejecución de Azure IoT Edge se hospeda en un entorno Linux.

### <a name="configuration-file"></a>Archivo de configuración

Cuando el puente IoT Plug and Play se ejecuta de forma nativa en un dispositivo o puerta de enlace IoT, usa un archivo de configuración para:

- Obtener la información de conexión de IoT Central o IoT Hub.
- Configurar los dispositivos cuyas interfaces IoT Plug and Play se publican.

Use una de las siguientes opciones para proporcionar el archivo de configuración al puente:

- Pasar la ruta de acceso del archivo de configuración como parámetro de línea de comandos al ejecutable del puente.
- Usar el archivo *config.json* existente en la carpeta *pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console*.

El [esquema del archivo de configuración](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/pnpbridge/src/pnpbridge_config_schema.json) se encuentra disponible en el repositorio de GitHub.

> [!TIP]
> Si edita un archivo de configuración de puente en VS Code, puede usar este archivo de esquema para validar su archivo.

### <a name="iot-edge-module-configuration"></a>Configuración del módulo IoT Edge

Cuando el puente se ejecuta como módulo IoT Edge en un entorno de ejecución de Azure IoT Edge, el archivo de configuración se envía desde la nube como actualización de la propiedad `PnpBridgeConfig` deseada. El puente espera la actualización de esta propiedad para configurar los adaptadores y los componentes.

## <a name="extend-the-bridge"></a>Extensión del puente

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

## <a name="build-and-run-the-bridge-on-an-iot-device-or-gateway"></a>Compilación y ejecución del puente en una puerta de enlace o un dispositivo IoT

| Plataforma | Compatible |
| :-----------: | :-----------: |
| Windows |  Sí |
| Linux | Sí |

### <a name="prerequisites"></a>Requisitos previos

Para completar esta sección, es preciso instalar el siguiente software en la máquina local:

- Un entorno de desarrollo compatible con la compilación de C++ como [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir el componente Administrador de paquetes NuGet y la carga de trabajo **Desarrollo para el escritorio con C++** al instalar Visual Studio.
- [CMake](https://cmake.org/download/): al instalar CMake, seleccione la opción **Add CMake to the system PATH** (Agregar CMake a la ruta de acceso del sistema).
- Si compila en Windows, también tendrá que descargar el [SDK 17763 de Windows](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

### <a name="source-code"></a>Código fuente

Clone el repositorio del [puente IoT Plug and Play](https://github.com/Azure/iot-plug-and-play-bridge) en la máquina local:

```cmd/sh
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Se espera que el comando anterior tarde varios minutos en ejecutarse.

> [!NOTE]
> Si surgen problemas con el error de `git submodule update` en Windows, pruebe el siguiente comando para resolver el problema: `git config --system core.longpaths true`.

### <a name="build-the-bridge-on-windows"></a>Compilación del puente en Windows

Abra el **Símbolo del sistema para desarrolladores de VS 2019**, navegue hasta la carpeta que contiene el repositorio clonado y ejecute los siguientes comandos:

```cmd
cd pnpbridge\scripts\windows

build.cmd
```

Se espera que el comando anterior tarde varios minutos en ejecutarse.

Opcionalmente, puede abrir el archivo de soluciones *pnpbridge\cmake\pnpbridge_x86\azure_iot_pnp_bridge.sln* generado en Visual Studio para editar, recompilar y depurar el código.

> [!TIP]
> En este proyecto se usa CMAKE para generar los archivos de proyecto. Cualquier modificación que realice en el proyecto en Visual Studio podría perderse si no se actualizan los archivos de CMAKE adecuados.

### <a name="build-the-bridge-on-linux"></a>Compilación del puente en Linux

Con el shell de Bash, navegue hasta la carpeta que contiene el repositorio clonado y ejecute los siguientes comandos:

```bash
cd scripts/linux

./setup.sh

./build.sh
```

### <a name="configure-the-generic-sensors"></a>Configuración de los sensores genéricos

Modifique los parámetros siguientes en el nodo `pnp_bridge_connection_parameters` del archivo *config.json* en la carpeta *iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console* de Windows o la carpeta *iot-plug-and-play-bridge/pnpbridge/cmake/pnpbridge_linux\src/pnpbridge/samples/console* de Windows:

Si usa una cadena de conexión para conectarse al centro de IoT:

```JSON
{
  "connection_parameters": {
    "connection_type" : "connection_string",
    "connection_string" : "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "root_interface_model_id": "[To fill in]",
    "auth_parameters": {
      "auth_type": "symmetric_key",
      "symmetric_key": "[To fill in]"
    }
  }
}
```

> [!TIP]
> El valor de `symmetric_key` debe coincidir con la clave SAS de la cadena de conexión.

Si usa DPS para conectarse a la aplicación de IoT Hub o IoT Central:

```JSON
{
  "connection_parameters": {
    "connection_type" : "dps",
    "root_interface_model_id": "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "auth_parameters" : {
      "auth_type" : "symmetric_key",
      "symmetric_key" : "[DEVICE KEY]"
    },
    "dps_parameters" : {
      "global_prov_uri" : "[GLOBAL PROVISIONING URI] - typically it is global.azure-devices-provisioning.net",
      "id_scope": "[IoT Central / IoT Hub ID SCOPE]",
      "device_id": "[DEVICE ID]"
    }
  }
}
```

Revise el resto del archivo de configuración para ver qué componentes de interfaz y parámetros globales están configurados en este ejemplo.

### <a name="start-the-bridge-in-windows"></a>Inicio del puente en Windows

Inicie el puente mediante su ejecución en el símbolo del sistema:

```cmd
cd iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console

Debug\pnpbridge_bin.exe
```

> [!TIP]
> Para usar un archivo de configuración de otra ubicación, pase su ruta de acceso como parámetro de línea de comandos al ejecutar el puente.
  
> [!TIP]
> Si tiene una cámara integrada o una cámara USB conectada al equipo, puede iniciar una aplicación que use la cámara, como la aplicación integrada **Cámara**. Cuando se ejecuta la aplicación **Cámara**, la salida de la consola del puente muestra las estadísticas de supervisión y la velocidad de fotogramas se notifica mediante la interfaz de gemelo digital al centro de IoT.

## <a name="build-and-run-the-bridge-as-an-iot-edge-module"></a>Compilación y ejecución del puente como módulo IoT Edge

| Plataforma | Compatible |
| :-----------: | :-----------: |
| Windows |  No |
| Linux | Sí |

### <a name="prerequisites"></a>Requisitos previos

Para completar esta sección, necesita una instancia de Azure IoT Hub gratuita o de nivel estándar. Para obtener información sobre cómo crear un centro de IoT, vea [Creación de un centro de IoT](../iot-hub/iot-hub-create-through-portal.md).

En los pasos de esta sección se supone que tiene el siguiente entorno de desarrollo en una máquina con Windows 10. Estas herramientas permiten crear e implementar un módulo IoT Edge en el dispositivo IoT Edge:

- Subsistema de Windows para Linux (WSL) 2 que ejecute Ubuntu 18.04 LTS. Para obtener más información, consulte la [Guía de instalación del Subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).
- Docker Desktop para Windows configurado para usar WSL 2. Para obtener más información, consulte el artículo sobre el [back-end de Docker Desktop WSL 2](https://docs.docker.com/docker-for-windows/wsl/).
- [Visual Studio Code instalado en el entorno Windows](https://code.visualstudio.com/docs/setup/windows) con las tres extensiones siguientes instaladas:

  - [Paquete de extensión de desarrollo remoto](https://aka.ms/vscode-remote/download/extension): esta extensión permite compilar y ejecutar código en WSL 2.
  - [Docker para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker): esta extensión se debe habilitar en el entorno **WSL: Ubuntu-18.04** de VS Code.
  - [Azure IoT Tools para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools): esta extensión se debe habilitar en el entorno **WSL: Ubuntu-18.04** de VS Code.

- Ejecute los siguientes comandos en el entorno de WSL 2 para instalar las herramientas de compilación necesarias:

  ```bash
  sudo apt-get update
  sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
  ```

- La [CLI de Azure](/cli/azure/install-azure-cli-apt?view=azure-cli-latest&preserve-view=true) instalada en el entorno de WSL 2 para administrar los recursos de Azure.

  > [!TIP]
  > Si lo prefiere, puede ejecutar los comandos `az` en la instancia de [Azure Cloud Shell](https://shell.azure.com/) en la que está preinstalada la CLI.

### <a name="create-an-iot-edge-device"></a>Crear un dispositivo IoT Edge

Los comandos siguientes crean un dispositivo IoT Edge en ejecución en una máquina virtual de Azure. La ejecución de un dispositivo IoT Edge en una máquina virtual resulta útil para probar la implementación si no se tiene acceso a un dispositivo real.

Para crear un registro de dispositivos IoT Edge en el centro de IoT, ejecute los siguientes comandos en el entorno de WSL 2. Use el comando `az login` para iniciar sesión en su suscripción de Azure:

```bash
az iot hub device-identity create --device-id bridge-edge-device --edge-enabled true --hub-name {your IoT hub name}
```

Para crear una máquina virtual de Azure que tenga el entorno de ejecución de Azure IoT Edge instalado, ejecute los siguientes comandos. Actualice los marcadores de posición con los valores adecuados:

```bash
az group create --name bridge-edge-resources --location eastus
az deployment group create \
--resource-group bridge-edge-resources \
--template-uri "https://aka.ms/iotedge-vm-deploy" \
--parameters dnsLabelPrefix='{unique DNS name for virtual machine}' \
--parameters adminUsername='{admin user name}' \
--parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name} -o tsv) \
--parameters authenticationType='password' \
--parameters adminPasswordOrKey="{admin password for virtual machine}"
```

Ahora tiene el entorno de ejecución de Azure IoT Edge en ejecución en una máquina virtual. Puede usar el siguiente comando para comprobar que los módulos **$edgeAgent** y **$edgeHub** se ejecutan en el dispositivo:

```bash
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

> [!CAUTION]
> Se le facturará mientras la máquina virtual esté en ejecución. Asegúrese de cerrarla cuando no la usa y quítela cuando haya terminado de usarla.

### <a name="download-the-source-code"></a>Descarga del código fuente

En los pasos siguientes, compilará la imagen de Docker en el entorno de WSL 2. Para clonar el repositorio de **iot-plug-and-play-bridge**, ejecute los comandos siguientes en un shell de Bash de WSL 2 en una carpeta adecuada:

```bash
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Se espera que el comando anterior tarde varios minutos en ejecutarse.

### <a name="update-the-dockerfile"></a>Actualización del archivo *Dockerfile*

Inicie VS Code, abra la paleta de comandos, escriba *Remote WSL: Open folder in WSL* (Remote WSL: Abrir carpeta en WSL) y abra la carpeta *iot-plug-and-play-bridge* que contiene el repositorio clonado.

Abra el archivo *pnpbridge\Dockerfile.amd64*. Edite las definiciones de variables de entorno de la siguiente manera:

```dockerfile
ENV IOTHUB_DEVICE_CONNECTION_STRING="{Add your device connection string here}"
ENV PNP_BRIDGE_ROOT_MODEL_ID="dtmi:com:example:RootPnpBridgeSampleDevice;1"
ENV PNP_BRIDGE_HUB_TRACING_ENABLED="false"
ENV IOTEDGE_WORKLOADURI="something"
```

> [!TIP]
> Use el siguiente comando para recuperar la cadena de conexión del dispositivo: `az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name}`

Hay archivos *Dockerfile* de ejemplo para otras arquitecturas.

### <a name="build-the-iot-plug-and-play-bridge-module-image"></a>Compilación de la imagen del módulo del puente IoT Plug and Play

En VS Code, abra la paleta de comandos, escriba *Docker Registries: Log In to Docker CLI* (Registros de Docker: Iniciar sesión en la CLI de Docker), seleccione la suscripción de Azure y Container Registry. Este comando permite a Docker conectarse al registro de contenedor y cargar la imagen del módulo.

Abra el archivo *pnpbridge/module.json*. Agregue `{your container registry name}.azurecr.io/iotpnpbridge` como repositorio de imágenes de contenedor y `v1` como versión.

En VS Code, haga clic con el botón derecho en el archivo *pnpbridge/module.json* en la vista **Explorer**, seleccione **Build and Push IoT Edge Module Image** (Compilar e insertar la imagen de módulo IoT Edge) y elija **amd64** como plataforma.

En VS Code, en la vista **Docker**, puede examinar el contenido del registro de contenedor que ahora incluye la imagen de módulo **iotpnpbridge:V1-amd64**.

### <a name="create-a-container-registry"></a>Creación de un registro de contenedor

Un dispositivo IoT Edge descarga sus imágenes de módulo desde un registro de contenedor. En este ejemplo se utiliza Azure Container Registry.

Cree una instancia de Azure Container Registry en el grupo de recursos **bridge-edge-resources**. A continuación, habilite el acceso de administrador a su registro de contenedor y obtenga las credenciales que el dispositivo IoT Edge necesita para descargar las imágenes de módulo:

```bash
az acr create -g bridge-edge-resources --sku Basic -n {your container registry name}
az acr update --admin-enabled true -n {your container registry name}
az acr credential show -n {your container registry name}
```

### <a name="create-the-deployment-manifest"></a>Creación del manifiesto de implementación

Un manifiesto de implementación de IoT Edge especifica los módulos y los valores de configuración que se van a implementar en un dispositivo IoT Edge.

En VS Code, abra el archivo *pnpbridge/deployment.template.json*:

- Actualice el elemento `registryCredentials` por los valores del comando anterior. El valor `address` es similar a `{your container registry name}.azurecr.io`.
- Actualice el valor `image` del objeto `ModulePnpBridge`. La imagen de módulo es similar a: `{your container registry}.azurecr.io/iotpnpbridge:v1-amd64`.
- Reemplace el elemento `PnPBridgeConfig` por el siguiente código JSON para configurar el adaptador de detección de CO2:

  ```json
  "PnpBridgeConfig": {
    "pnp_bridge_interface_components": [
      {
        "_comment": "Component 1 - Modbus Device",
        "pnp_bridge_component_name": "Co2Detector1",
        "pnp_bridge_adapter_id": "modbus-pnp-interface",
        "pnp_bridge_adapter_config": {
          "unit_id": 1,
          "tcp": {
            "host": "10.159.29.2",
            "port": 502
          },
          "modbus_identity": "DL679"
        }
      }
    ],
    "pnp_bridge_adapter_global_configs": {
      "modbus-pnp-interface": {
        "DL679": {
          "telemetry": {
            "co2": {
              "startAddress": "40001",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 5000,
              "conversionCoefficient": 1
            },
            "temperature": {
              "startAddress": "40003",
              "length": 1,
              "dataType": "decimal",
              "defaultFrequency": 5000,
              "conversionCoefficient": 0.01
            }
          },
          "properties": {
            "firmwareVersion": {
              "startAddress": "40482",
              "length": 1,
              "dataType": "hexstring",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "modelName": {
              "startAddress": "40483",
              "length": 2,
              "dataType": "string",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmStatus_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "boolean",
              "defaultFrequency": 1000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmThreshold_co2": {
              "startAddress": "40225",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 30000,
              "conversionCoefficient": 1,
              "access": 2
            }
          },
          "commands": {
            "clearAlarm_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "flag",
              "conversionCoefficient": 1
            }
          }
        }
      }
    }
  }
  ```

  Guarde los cambios.

En VS Code, haga clic con el botón derecho en el archivo *pnpbridge/deployment.template.json* de la vista **Explorer** y seleccione **Generate IoT Edge Deployment Manifest** (Generar un manifiesto de implementación de IoT Edge). Este comando genera el manifiesto de implementación de *pnpbridge/config/deployment.amd64.json*.

### <a name="deploy-the-bridge-to-your-iot-edge-device"></a>Implementación del paquete en el dispositivo IoT Edge

En VS Code, abra la paleta de comandos, escriba *Azure IoT Hub: Select IoT Hub* (Azure IoT Hub: Seleccionar IoT Hub) y seleccione la suscripción y IoT Hub.

En VS Code, haga clic con el botón derecho en el archivo *pnpbridge/config/deployment.amd64.json* en la vista **Explorer**, seleccione **Create Deployment for Single Device** (Crear una implementación para un dispositivo individual) y elija el objeto **bridge-edge-device**.

Para ver el estado de los módulos del dispositivo, ejecute el comando siguiente:

```bash
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

La lista de módulos en ejecución incluye ahora el módulo **ModulePnpBridge** que está configurado para usar el adaptador de detección de CO2.

### <a name="tidy-up"></a>Limpieza

Para quitar la máquina virtual y el registro de contenedor de la suscripción de Azure, ejecute el siguiente comando:

```bash
az group delete -n bridge-edge-resources
```

## <a name="folder-structure"></a>Estructura de carpetas

*pnpbridge\deps\azure-iot-sdk-c-pnp*: Submódulos de Git que contienen el SDK de dispositivo Azure IoT para el SDK de C.

*pnpbridge\scripts*: Scripts de compilación.

*pnpbridge\src*: Código fuente del núcleo del puente IoT Plug and Play.

*pnpbridge\src\adapters*: Código fuente para los distintos adaptadores del puente IoT Plug and Play.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el puente IoT Plug and Play, visite el repositorio del [puente IoT Plug and Play](https://github.com/Azure/iot-plug-and-play-bridge) de GitHub.
