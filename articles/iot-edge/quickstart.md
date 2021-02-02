---
title: 'Guía de inicio rápido: Creación de un dispositivo Azure IoT Edge en Windows | Microsoft Docs'
description: En esta guía de inicio rápido, obtenga información sobre cómo crear un dispositivo IoT Edge y luego implementar código creado previamente de manera remota desde Azure Portal.
author: rsameser
manager: kgremban
ms.author: riameser
ms.date: 01/20/2021
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, devx-track-azurecli
monikerRange: =iotedge-2018-06
ms.openlocfilehash: 71e38059aceb7da63f3545610b9acfe48c5d3150
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98663220"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-windows-device-preview"></a>Inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Windows (versión preliminar)

Pruebe Azure IoT Edge en este inicio rápido mediante la implementación de código en contenedor en un dispositivo IoT Edge para Linux en Windows. IoT Edge permite administrar de forma remota el código de los dispositivos para que pueda enviar más cargas de trabajo al perímetro. Para este inicio rápido, es aconsejable que use su propio dispositivo para ver lo fácil que es utilizar Azure IoT Edge para Linux en Windows.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

* Crear un Centro de IoT.
* Registrar un dispositivo IoT Edge en su instancia de IoT Hub.
* Instalar e iniciar Azure IoT Edge para Linux en el runtime de Windows del dispositivo.
* Implementar un módulo de forma remota en un dispositivo IoT Edge y enviar datos de telemetría.

![Diagrama: Inicio rápido de la arquitectura para el dispositivo y la nube](./media/quickstart/install-edge-full.png)

En este inicio rápido se explica cómo configurar Azure IoT Edge para Linux en un dispositivo Windows. Después, implemente un módulo desde Azure Portal en el dispositivo. El módulo que se usa en este inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los restantes tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos adicionales que analizan los datos simulados para obtener información empresarial.

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de comenzar.

>[!NOTE]
>IoT Edge para Linux en Windows se encuentra en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Prepare el entorno para la CLI de Azure.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

Recursos en la nube:

* Un grupo de recursos para administrar todos los recursos que se van a usar en esta guía de inicio rápido.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

Dispositivo de IoT Edge:

* El dispositivo debe ser un servidor o PC Windows, versión 1809 o posterior.
* Debe tener al menos 4 GB de memoria, aunque se recomiendan 8 GB.
* 10 GB de espacio libre en disco

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para empezar el inicio rápido, cree un centro de IoT con la CLI de Azure.

![Diagrama: Creación de un centro de IoT en la nube](./media/quickstart/create-iot-hub.png)

El nivel gratuito de IoT Hub funciona para esta guía de inicio rápido. Si ha usado IoT Hub en el pasado y ya tiene un centro creado, puede usarlo.

El código siguiente crea un centro **F1** gratis en el grupo de recursos `IoTEdgeResources`. Reemplace `{hub_name}` por un nombre único para su centro de IoT. El centro de IoT puede tardar algunos minutos en crearse.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Si se produce un error porque ya hay un centro gratis en la suscripción, cambie la SKU a **S1**. Si recibe un error que le indica que el nombre de IoT Hub no está disponible, significa que alguien más ya tiene un centro con ese nombre. Pruebe con uno nuevo.

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.

![Diagrama: Registro de un dispositivo con una identidad de IoT Hub](./media/quickstart/register-device.png)

Cree una identidad para el dispositivo simulado, con el fin de que pueda comunicarse con su centro de IoT. La identidad del dispositivo reside en la nube, y se usa una cadena de conexión de dispositivo única para asociar un dispositivo físico a una identidad de dispositivo.

Dado que los dispositivos de IoT Edge se comportan y se pueden administrar de manera diferente a los dispositivos de IoT típicos, declare que esta identidad es para un dispositivo de IoT Edge con la marca `--edge-enabled`.

1. En Azure Cloud Shell, escriba el comando siguiente para crear un dispositivo llamado **myEdgeDevice** en el centro.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --edge-enabled --hub-name {hub_name}
   ```

   Si recibe un error acerca de las claves de directiva de iothubowner, asegúrese de que Cloud Shell ejecuta la versión más reciente de la extensión azure-iot.

2. Vea la cadena de conexión del dispositivo, que vincula el dispositivo físico con su identidad en IoT Hub. Contiene el nombre del centro de IoT, el nombre del dispositivo y, después, una clave compartida que autentica las conexiones entre los dos.

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Copie el valor de la clave `connectionString` desde la salida JSON y guárdelo. Este valor es la cadena de conexión del dispositivo. Esta cadena de conexión se usará para configurar el entorno de ejecución de IoT Edge en la sección siguiente.

   ![Recuperar la cadena de conexión de la salida de la CLI](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>Instale e inicie el runtime de IoT Edge

Instale Azure IoT Edge para Linux en Windows en el dispositivo y configúrelo con una cadena de conexión del dispositivo.

![Diagrama: Inicio del entorno de ejecución de Azure IoT Edge en el dispositivo](./media/quickstart/start-runtime.png)

1. [Descargue Windows Admin Center](https://aka.ms/WACDownloadEFLOW).

1. Siga el Asistente para la instalación para configurar Windows Admin Center en el dispositivo.

1. Una vez que esté en Windows Admin Center, en la parte superior derecha de la pantalla, seleccione el **icono del engranaje de configuración**

1. En el menú de configuración, en Puerta de enlace, seleccione **Extensiones**.

1. Vaya a la pestaña **Fuentes** y seleccione **Agregar**.

1. Escriba https://aka.ms/wac-insiders-feed en el cuadro de texto y seleccione **Agregar**.

1. Una vez agregada la fuente, vaya a la pestaña **Extensiones disponibles**. La lista de extensiones puede tardar un momento en actualizarse.

1. En la lista **Extensiones disponibles** , seleccione **Azure IoT Edge**.

1. **Instale** la extensión.

1. Una vez instalada la extensión, vaya a la página del panel principal, para lo que debe seleccionar **Windows Admin Center** en la esquina superior izquierda de la pantalla.

1. Verá la conexión de host local, que representa el PC en el que se ejecuta Windows Admin Center.

   :::image type="content" source="media/quickstart/windows-admin-center-start-page.png" alt-text="Captura de pantalla: Página de inicio de Windows Admin Center":::

1. Seleccione **Agregar**.

   :::image type="content" source="media/quickstart/windows-admin-center-start-page-add.png" alt-text="Captura de pantalla: Botón Agregar de la página de inicio de Windows Admin Center":::

1. Busque el icono de Azure IoT Edge y seleccione **Crear nuevo**. Se iniciará el Asistente para la instalación.

    :::image type="content" source="media/quickstart/select-tile-screen.png" alt-text="Captura de pantalla: icono de Azure IoT Edge para Linux en Windows":::

1. Siga los pasos del Asistente para la instalación para aceptar el CLUF y elija **Siguiente**.

    :::image type="content" source="media/quickstart/wizard-welcome-screen.png" alt-text="Captura de pantalla: página de bienvenida del asistente":::

1. Elija **Datos de diagnóstico opcionales** para proporcionar datos de diagnóstico extendidos que ayuden a Microsoft a supervisar y mantener la calidad de servicio y haga clic en **Siguiente: Implementación**

    :::image type="content" source="media/quickstart/diagnostic-data-screen.png" alt-text="Captura de pantalla: Datos de diagnóstico":::

1. En la pantalla **Select target device** (Seleccionar dispositivo de destino), seleccione el dispositivo de destino que desee para comprobar que cumple los requisitos mínimos. En este inicio rápido, vamos a instalar IoT Edge en el dispositivo local, por lo que debe elegir la conexión localhost. Una vez confirmada, elija **Next** (Siguiente) para continuar.

    :::image type="content" source="media/quickstart/wizard-select-target-device-screen.png" alt-text="Captura de pantalla: Seleccionar dispositivo de destino":::

1. Acepte la configuración predeterminada, para lo que debe seleccionar **Next** (Siguiente).

1. En la pantalla de implementación se muestra el proceso de descarga del paquete, la instalación del paquete, la configuración del host y la configuración final de la máquina virtual Linux.  Así será una implementación correcta:

    :::image type="content" source="media/quickstart/wizard-deploy-success-screen.png" alt-text="Captura de pantalla: implementación correcta del asistente":::

1. Haga clic en **Siguiente: Connect** (Siguiente: Conectar) para continuar al paso final para aprovisionar un dispositivo Azure IoT Edge con su identificador de dispositivo desde la instancia de IoT Hub.

1. Copie la cadena de conexión del dispositivo en Azure IoT Hub y péguela en el campo de la cadena de conexión del dispositivo. Luego, elija **Provisioning with the selected method** (Aprovisionamiento con el método seleccionado).

    > [!NOTE]
    > Consulte el paso 3 de la sección anterior, [Registro de un dispositivo IoT Edge](#register-an-iot-edge-device), para recuperar la cadena de conexión.

    :::image type="content" source="media/quickstart/wizard-provision.png" alt-text="Captura de pantalla: Aprovisionamiento del asistente":::

1. Una vez completado el aprovisionamiento, seleccione **Finish** (Finalizar) para completar y volver a la pantalla de inicio de Windows Admin Center. Ya debería poder ver el dispositivo en la lista como un dispositivo IoT Edge.

    :::image type="content" source="media/quickstart/windows-admin-center-device-screen.png" alt-text="Captura de pantalla: Dispositivo Azure IoT Edge de Windows Admin Center":::

1. Seleccione el dispositivo Azure IoT Edge para ver su panel. Debería ver que se han implementado las cargas de trabajo del dispositivo gemelo en Azure IoT Hub. En **IoT Edge Module List** (Lista de módulos de IoT Edge) se debe mostrar un módulo en ejecución, **edgeAgent**, y en **IoT Edge Status** (Estado de IoT Edge) se debe mostrar **active (running)** [activo (en ejecución)].

El dispositivo de IoT Edge está ya configurado. Está preparado para ejecutar módulos implementados en la nube.

## <a name="deploy-a-module"></a>Implementación de un módulo

Administre el dispositivo Azure IoT Edge desde la nube para implementar un módulo que envía datos de telemetría a IoT Hub.

![Diagrama: Implementación del módulo desde la nube al dispositivo](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Visualización de datos generados

En esta guía de inicio rápido, ha creado un nuevo dispositivo de IoT Edge y ha instalado el runtime de IoT Edge en él. Luego, ha usado Azure Portal para implementar un módulo de IoT Edge para que se ejecute en el dispositivo sin tener que realizar cambios en el propio dispositivo.

En este caso, el módulo que ha insertado genera los datos del entorno de ejemplo que puede usar posteriormente para las pruebas. El sensor simulado está supervisando una máquina y el entorno alrededor de esta. Por ejemplo, este sensor podría estar en una sala de servidores, en una fábrica o en un aerogenerador. El mensaje incluye la temperatura y la humedad ambiental, la temperatura y la presión de la máquina, y una marca de tiempo. Los tutoriales de IoT Edge usan los datos creados por este módulo como datos de prueba con fines de análisis.

Confirme que el módulo que se ha implementado desde la nube se ejecuta en el dispositivo IoT Edge; para ello, vaya al shell de comandos de Windows Admin Center.

1. Conéctese al dispositivo IoT Edge recién creado.

   :::image type="content" source="media/quickstart/connect-edge-screen.png" alt-text="Captura de pantalla: Conectar dispositivo":::

2. En la página **Overview** (Información general), verá las secciones **IoT Edge Module List** (Lista de módulos de IoT Edge) y **IoT Edge Status** (Estado de IoT Edge), en las que puede ver los distintos módulos que se han implementado, así como el estado del dispositivo.  

3. En **Tools** (Herramientas) seleccione **Command Shell** (Shell de comandos). El shell de comandos es un terminal con PowerShell que usa ssh (Secure Shell) automáticamente para conectarse a la máquina virtual Linux del dispositivo Azure IoT Edge en un PC Windows.

   :::image type="content" source="media/quickstart/command-shell-screen.png" alt-text="Captura de pantalla: Shell de comandos":::

4. Para comprobar los tres módulos del dispositivo, ejecute el siguiente **comando bash**:

   ```bash
   sudo iotedge list
   ```

   :::image type="content" source="media/quickstart/iotedge-list-screen.png" alt-text="Captura de pantalla: Lista del shell de comandos":::

5. Vea los mensajes que se envían desde el módulo de sensor de temperatura a la nube.

   ```bash
   iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >Los comandos de IoT Edge distinguen mayúsculas de minúsculas en los nombres de los módulos.

   :::image type="content" source="media/quickstart/temperature-sensor-screen.png" alt-text="Captura de pantalla: Sensor de temperatura":::

También puede ver los mensajes que llegan a su centro de IoT mediante la [extensión de Azure IoT Hub para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los tutoriales de IoT Edge, puede usar el dispositivo que ha registrado y configurado en esta guía de inicio rápido. En caso contrario, puede eliminar los recursos de Azure que creó para evitar gastos.

Si ha creado una máquina virtual y un centro de IoT en un nuevo grupo de recursos, puede eliminar dicho grupo y todos los recursos asociados. Vuelva a comprobar el contenido del grupo de recursos para asegurarse de que no haya nada que desee conservar. Si no desea eliminar todo el grupo, puede eliminar recursos individuales en su lugar.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible.

Quite el grupo **IoTEdgeResources**. La eliminación de un grupo de recursos puede tardar unos minutos.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

Puede confirmar que se ha eliminado el grupo de recursos mediante la visualización de la lista de grupos de recursos.

```azurecli-interactive
az group list
```

### <a name="clean-removal-of-azure-iot-edge-for-linux-on-windows"></a>Eliminación limpia de Azure IoT Edge para Linux en Windows

Azure IoT Edge para Linux en Windows se puede desinstalar del dispositivo IoT Edge a través de la extensión del panel en Windows Admin Center.

1. Conéctese al dispositivo con Azure IoT Edge para Linux en Windows en Windows Admin Center. Se cargará la extensión de la herramienta del panel de Azure.
2. Seleccione **Desinstalar**. Una vez que se quita Azure IoT Edge para Linux en Windows, Windows Admin Center irá a la página de inicio y quitará la entrada de conexión del dispositivo Azure IoT Edge de la lista.

Otra forma de quitar Azure IoT Edge del sistema Windows es ir a **Inicio** > **Configuración** > **Aplicaciones** > **Azure IOT Edge** > **Desinstalar** en el dispositivo IoT Edge. Así se eliminará Azure IoT Edge del dispositivo IoT Edge, pero dejará la conexión en Windows Admin Center. Windows Admin Center se puede desinstalar también desde el menú Configuración.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo de prueba que genera datos sin procesar acerca de su entorno.

El siguiente paso es configurar el entorno de desarrollo local para que pueda empezar a crear módulos de IoT Edge que ejecutan la lógica de negocios.

> [!div class="nextstepaction"]
> [Empezar a desarrollar módulos de IoT Edge](tutorial-develop-for-linux.md)
