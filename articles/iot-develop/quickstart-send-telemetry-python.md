---
title: Inicio rápido para el envío de datos de telemetría del dispositivo a Azure IoT Central (Python)
description: En esta guía de inicio rápido, usará el SDK de dispositivo de Azure IoT Hub para Python para enviar datos de telemetría desde un dispositivo a IoT Central.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: python
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: d0dcca0c4be801f385a48afcd41b6a547bab3fbe
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654775"
---
# <a name="quickstart-send-telemetry-from-a-device-to-azure-iot-central-python"></a>Inicio rápido: Envío de datos de telemetría desde un dispositivo a Azure IoT Central (Python)

**Se aplica a**: [desarrollo de aplicaciones para dispositivos](about-iot-develop.md#device-application-development)

En este inicio rápido, aprenderá un flujo de trabajo básico de desarrollo de aplicaciones para dispositivos IoT. En primer lugar, se usa Azure IoT Central para crear una aplicación en la nube. Luego, se usa el SDK de Python de Azure IoT para crear un dispositivo simulado, conectarse a IoT Central y enviar datos de telemetría del dispositivo a la nube. 

## <a name="prerequisites"></a>Prerrequisitos
- [Python 3.7+](https://www.python.org/downloads/). Para obtener información acerca de otras versiones de Python compatibles, consulte el tema acerca de las [características de los dispositivos IoT de Azure](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).
    
    Para asegurarse de que la versión de Python esta actualizada, ejecute `python --version`. Si tiene instalado tanto Python 2 como Python 3 y usa un entorno de Python 3, instale todas las bibliotecas con `pip3`. La ejecución de `pip3` garantiza que las bibliotecas se instalan en el entorno de ejecución de Python 3.
    > [!IMPORTANT]
    > En el instalador de Python, seleccione la opción para **agregar Python a la ruta de acceso**. Si ya tiene instalado Python 3.7 o una versión posterior, confirme que ha agregado la carpeta de instalación de Python a la variable de entorno `PATH`.

## <a name="create-an-application"></a>Crear una aplicación
En esta sección se crea una aplicación de IoT Central. IoT Central es una plataforma de aplicaciones IoT basada en portal que ayuda a reducir la complejidad y el costo del desarrollo y administración de soluciones IoT.

Para crear una aplicación de Azure IoT Central:
1. Vaya a [Azure IoT Central](https://apps.azureiotcentral.com/) e inicie sesión con una cuenta personal, profesional o educativa de Microsoft.
1. Vaya a **Compilar** y seleccione **Aplicaciones personalizadas**.
   :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-build.png" alt-text="Página de inicio de IoT Central":::
1. En **Nombre de aplicación**, escriba un nombre único o use el nombre generado.
1. En **URL**, escriba un prefijo de dirección URL de aplicación fácil de recordar o use el prefijo de dirección URL generado.
1. En **Plantilla de aplicación**, deje la opción *Aplicación personalizada*. Es posible que la lista desplegable muestre otras opciones, si ya existen plantillas en su cuenta.
1. Seleccione un opción en **Plan de tarifa**. 
    - Para usar la aplicación de forma gratuita durante siete días, seleccione **Gratis**. Las aplicaciones gratuitas se pueden convertir a los precios estándar antes de que expiren.
    - Opcionalmente, se puede seleccionar un plan de tarifas estándar. Si selecciona los precios estándar, aparecerán más opciones y deberá establecer un **directorio**, una **suscripción de Azure** y una **ubicación**. Para obtener información sobre los precios, consulte [Precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). 
        - **Directorio** es la instancia de Azure Active Directory en que se crea la aplicación. Azure Active Directory contiene identidades de usuario, credenciales y otra información de la organización. Si no tiene Azure Active Directory, se crea automáticamente al crear una suscripción de Azure.
        - Una **suscripción de Azure** permite crear instancias de los servicios de Azure. IoT Central aprovisiona los recursos de su suscripción. Si no tiene una suscripción de Azure, [puede crear una gratis](https://azure.microsoft.com/free/). Después de crear la suscripción, vuelva a la página **Nueva aplicación** de IoT Central. La nueva suscripción aparece en la lista desplegable **Suscripción de Azure**.
        - **Ubicación** es la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) en la que se crea una aplicación. Seleccione una ubicación que se encuentre lo más cercana físicamente posible a los dispositivos a fin de obtener un rendimiento óptimo. Una vez elegida una ubicación, no podrá mover la aplicación a otra.

    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-pricing.png" alt-text="Cuadro de diálogo de una nueva aplicación de IoT Central":::
1. Seleccione **Crear**.
    
    Una vez que IoT Central crea la aplicación, le redirige al panel de la aplicación.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-created.png" alt-text="Panel de aplicación nueva de IoT Central":::

## <a name="add-a-device"></a>Agregar un dispositivo
En esta sección, se agrega un nuevo dispositivo a una aplicación de IoT Central. El dispositivo es una instancia de una plantilla de dispositivo que representa un dispositivo real o simulado que se conectará a la aplicación. 

Para crear un dispositivo:
1. En el panel izquierdo, seleccione **Dispositivos** y, después, seleccione **+ Nuevo**. Se abre el cuadro de diálogo de un dispositivo nuevo.
1. En **Plantilla de dispositivo** deje la opción *Sin asignar*.

    > [!NOTE]
    > En aras de la sencillez, en este inicio rápido, se conecta un dispositivo simulado que usa una plantilla sin asignar. Si sigue usando IoT Central para administrar dispositivos, aprenderá a usar plantillas de dispositivo. Para obtener información general sobre cómo trabajar con plantillas de dispositivo, consulte [Inicio rápido: Adición de un dispositivo simulado a una aplicación de IoT Central](../iot-central/core/quick-create-simulated-device.md).
1. Establezca un **nombre de dispositivo** y un **identificador de dispositivo** descriptivos. Opcionalmente, use los valores generados.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-create-device.png" alt-text="Cuadro de diálogo de nuevo dispositivo de IoT Central":::
1. Seleccione **Crear**.

    El dispositivo creado aparece en la lista **Todos los dispositivos**.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-devices-list.png" alt-text="Lista de todos los dispositivos de IoT Central":::
    
Para recuperar los detalles de conexión del nuevo dispositivo:
1. En la lista **Todos los dispositivos**, haga doble clic en el nombre del dispositivo vinculado para mostrar los detalles. 
1. En el menú superior, seleccione **Conectar**.

    En el cuadro de diálogo **Conexión del dispositivo** se muestran los detalles de la conexión: :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-device-connect.png" alt-text="Detalles de conexión de dispositivo de IoT Central":::
1. Copie los siguientes valores del cuadro de diálogo **Conexión del dispositivo** en una ubicación segura, ya que los usará en la sección siguiente para conectar el dispositivo a IoT Central.
    * `ID scope`
    * `Device ID`
    * `Primary key`

## <a name="send-messages-and-monitor-telemetry"></a>Envío de mensajes y supervisión de los datos de telemetría
En esta sección, usará el SDK de Python para crear un dispositivo simulado y enviar datos de telemetría a una aplicación IoT Central. 

1. Abra un terminal mediante la línea de comandos de Windows, PowerShell o Bash (para Windows o Linux). Usará el terminal para instalar el SDK de Python, actualizar las variables de entorno y ejecutar el ejemplo de código de Python.

1. Copie los [ejemplos para el SDK de dispositivo IoT de Azure para Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) en la máquina local.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Vaya al directorio *azure-iot-sdk-python/azure-iot-device/samples*.

    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples
    ```
1. Instale el SDK de Azure IoT para Python.

    ```console
    pip install azure-iot-device
    ```

1. Establezca las siguientes variables de entorno para que el dispositivo simulado se pueda conectar a IoT Central. Para `ID_SCOPE`, `DEVICE_ID` y `DEVICE_KEY`, use los valores que guardó en el cuadro de diálogo *Conexión del dispositivo* de IoT Central.

    **Línea de comandos de Windows**

    ```console
    set PROVISIONING_HOST=global.azure-devices-provisioning.net
    ```
    ```console
    set ID_SCOPE=<your ID scope>
    ```
    ```console
    set DEVICE_ID=<your device ID>
    ```
    ```console
    set DEVICE_KEY=<your device's primary key>
    ```

    > [!NOTE]
    > En el caso del símbolo de comandos de Windows, no hay comillas alrededor de la cadena de conexión u otros valores variables.

    **PowerShell**

    ```azurepowershell
    $env:PROVISIONING_HOST='global.azure-devices-provisioning.net'
    ```
    ```azurepowershell
    $env:ID_SCOPE='<your ID scope>'
    ```
    ```azurepowershell
    $env:DEVICE_ID='<your device ID>'
    ```
    ```azurepowershell
    $env:DEVICE_KEY='<your device's primary key>'
    ```

    **Bash (Linux o Windows)**

    ```bash
    export PROVISIONING_HOST='global.azure-devices-provisioning.net'
    ```
    ```bash
    export ID_SCOPE='<your ID scope>'
    ```
    ```bash
    export DEVICE_ID='<your device ID>'
    ```
    ```bash
    export DEVICE_KEY='<your device's primary key>'
    ```

1. En el terminal, ejecute el código del archivo de ejemplo *simple_send_temperature. py. Este código accede al dispositivo IoT simulado y envía un mensaje a IoT Central.

    Para ejecutar el ejemplo de Python desde el terminal:
    ```console
    python ./simple_send_temperature.py
    ```

    Opcionalmente, puede ejecutar el código de Python desde el ejemplo en el IDE de Python:
    ```python
    import asyncio
    import os
    from azure.iot.device.aio import ProvisioningDeviceClient
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.iot.device import Message
    import uuid
    import json
    import random

    # ensure environment variables are set for your device and IoT Central application credentials
    provisioning_host = os.getenv("PROVISIONING_HOST")
    id_scope = os.getenv("ID_SCOPE")
    registration_id = os.getenv("DEVICE_ID")
    symmetric_key = os.getenv("DEVICE_KEY")

    # allows the user to quit the program from the terminal
    def stdin_listener():
        """
        Listener for quitting the sample
        """
        while True:
            selection = input("Press Q to quit\n")
            if selection == "Q" or selection == "q":
                print("Quitting...")
                break

    async def main():

        # provisions the device to IoT Central-- this uses the Device Provisioning Service behind the scenes
        provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
            provisioning_host=provisioning_host,
            registration_id=registration_id,
            id_scope=id_scope,
            symmetric_key=symmetric_key,
        )

        registration_result = await provisioning_device_client.register()

        print("The complete registration result is")
        print(registration_result.registration_state)

        if registration_result.status == "assigned":
            print("Your device has been provisioned. It will now begin sending telemetry.")
            device_client = IoTHubDeviceClient.create_from_symmetric_key(
                symmetric_key=symmetric_key,
                hostname=registration_result.registration_state.assigned_hub,
                device_id=registration_result.registration_state.device_id,
            )

            # Connect the client.
            await device_client.connect()

        # Send the current temperature as a telemetry message
        async def send_telemetry():
            print("Sending telemetry for temperature")

            while True:
                current_temp = random.randrange(10, 50)  # Current temperature in Celsius (randomly generated)
                # Send a single temperature report message
                temperature_msg = {"temperature": current_temp}

                msg = Message(json.dumps(temperature_msg))
                msg.content_encoding = "utf-8"
                msg.content_type = "application/json"
                print("Sent message")
                await device_client.send_message(msg)
                await asyncio.sleep(8)

        send_telemetry_task = asyncio.create_task(send_telemetry())

        # Run the stdin listener in the event loop
        loop = asyncio.get_running_loop()
        user_finished = loop.run_in_executor(None, stdin_listener)
        # Wait for user to indicate they are done listening for method calls
        await user_finished

        send_telemetry_task.cancel()
        # Finally, shut down the client
        await device_client.disconnect()

    if __name__ == "__main__":
        asyncio.run(main())

        # If using Python 3.6 or below, use the following code instead of asyncio.run(main()):
        # loop = asyncio.get_event_loop()
        # loop.run_until_complete(main())
        # loop.close()
    ```

Cuando el código de Python envía un mensaje desde el dispositivo a una aplicación de IoT Central, los mensajes aparecen en la pestaña **Datos sin procesar** del dispositivo en IoT Central. Es posible que tenga que actualizar la página para que muestre mensajes recientes.

   :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-telemetry-output.png" alt-text="Captura de pantalla de la salia de datos sin procesar de IoT Central":::

El dispositivo ya está conectado de forma segura y envía datos de telemetría a Azure IoT.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si ya no necesita los recursos de IoT Central que se han creado en este tutorial, puede eliminarlos desde el portal de IoT Central. Opcionalmente, si planea seguir con la documentación de esta guía, puede conservar la aplicación que creó y reutilizarla para otros ejemplos.

Para quitar la aplicación de ejemplo de Azure IoT Central y todos sus dispositivos y recursos:
1. Seleccione **Administración**  > **Su aplicación**.
1. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido un flujo de trabajo básico de una aplicación de Azure IoT para conectar un dispositivo a la nube de forma segura y enviar datos de telemetría del dispositivo a la nube. Ha utilizado la Azure IoT Central para crear una aplicación y un dispositivo y, después, ha utilizado el SDK de Azure IoT para Python para crear un dispositivo simulado y enviar datos de telemetría al centro. También ha usado IoT Central para supervisar la telemetría.

Como paso siguiente, explore el SDK de Azure IoT para Python mediante ejemplos de aplicaciones.

- [Ejemplos asincrónicos](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios): este directorio contiene ejemplos asincrónicos de Python para escenarios de IoT Hub adicionales.
- [Ejemplos sincrónicos](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples): este directorio contiene ejemplos de Python para su uso con Python 2.7 o escenarios de compatibilidad sincrónicos para Python 3.5+.
- [Ejemplos de IoT Edge](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios): este directorio contiene ejemplos de Python para trabajar con módulos IoT Edge y dispositivos de bajada.
