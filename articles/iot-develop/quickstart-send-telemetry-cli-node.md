---
title: Guía de inicio rápido para el envío de datos de telemetría del dispositivo a Azure IoT Hub (Node.js)
description: En esta guía de inicio rápido, usará el SDK de dispositivo de Azure IoT Hub para Node.js para enviar datos de telemetría desde un dispositivo a un centro de IoT.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: node
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: 97fcff4706d6da968c93426f85569fed9af95aac
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197816"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-nodejs"></a>Inicio rápido: Envío de datos de telemetría desde un dispositivo a un centro de IoT (Node.js)

**Se aplica a**: [desarrollo de aplicaciones para dispositivos](about-iot-develop.md#device-application-development)

En esta guía de inicio rápido, aprenderá un flujo de trabajo básico de desarrollo de aplicaciones para dispositivos IoT. Use la CLI de Azure para crear una instancia de Azure IoT Hub y un dispositivo simulado y, después, use el SDK de Azure IoT para Node.js para acceder al dispositivo y enviar datos de telemetría al centro.

## <a name="prerequisites"></a>Prerrequisitos
- Si no tiene una suscripción de Azure, [cree una gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.
- CLI de Azure. Puede ejecutar todos los comandos de esta guía de inicio rápido mediante el Azure Cloud Shell, un shell interactivo de la CLI que se ejecuta en el explorador. Si usa el Cloud Shell, no necesita instalar nada. Si prefiere usar la CLI de forma local, esta guía de inicio rápido requiere CLI de Azure versión 2.0.76 o posterior. Para saber qué versión tiene, ejecute el comando az --version. Para la instalación o la actualización, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).
- [Node.js 10+](https://nodejs.org). Si usa Azure Cloud Shell, no actualice la versión instalada de Node.js. Azure Cloud Shell ya tiene la versión más reciente de Node.js.

    Compruebe la versión actual de Node.js en la máquina de desarrollo con el comando siguiente:

    ```cmd/sh
        node --version
    ```

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-nodejs-sdk-to-send-messages"></a>Uso del SDK para Node.js para enviar mensajes
En esta sección, usará el SDK para Node.js para enviar mensajes desde el dispositivo simulado al centro de IoT. 

1. Abra una nueva ventana de terminal. Usará este terminal para instalar el SDK para Node.js y trabajar con el código de ejemplo de Node.js. Ahora debería tener dos terminales abiertos: el que acaba de abrir para trabajar con Node.js y el shell de la CLI que usó en las secciones anteriores para escribir los comandos de la CLI de Azure. 

1. Copie los [ejemplos para el SDK de dispositivo IoT de Azure para Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) en la máquina local:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-node
    ```

1. Vaya al directorio *azure-iot-sdk-node/device/samples*:

    ```console
    cd azure-iot-sdk-node/device/samples
    ```
1. Instale el SDK de Azure IoT para Node.js y las dependencias necesarias:

    ```console
    npm install
    ```
    Este comando instala las dependencias adecuadas, como se especifica en el archivo *package.json*, en el directorio de ejemplos de dispositivos.

1. Establezca la cadena de conexión del dispositivo como una variable de entorno llamada `DEVICE_CONNECTION_STRING`. El valor de cadena que se va a usar es la cadena obtenida en la sección anterior después de crear el dispositivo Node.js simulado. 

    **Windows (cmd)**

    ```console
    set DEVICE_CONNECTION_STRING=<your connection string here>
    ```

    > [!NOTE]
    > En el caso del símbolo de comandos de Windows, no hay comillas alrededor de la cadena de conexión.

    **Linux (bash)**

    ```bash
    export DEVICE_CONNECTION_STRING="<your connection string here>"
    ```

1. En el shell de la CLI abierto, ejecute el comando [az iot hub monitor-events](/cli/azure/ext/azure-iot/iot/hub#ext-azure-iot-az-iot-hub-monitor-events) para empezar a supervisar eventos en el dispositivo IoT simulado.  Los mensajes de los eventos se imprimirán en el terminal cuando lleguen.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. En el terminal de Node.js, ejecute el código del archivo de ejemplo instalado *simple_sample_device.js*. Este código accede al dispositivo IoT simulado y envía un mensaje al centro de IoT.

    Para ejecutar el ejemplo de Node.js desde el terminal:
    ```console
    node ./simple_sample_device.js
    ```

    Opcionalmente, puede ejecutar el código de Node.js del ejemplo en el IDE de JavaScript:
    ```javascript
    'use strict';

    const Protocol = require('azure-iot-device-mqtt').Mqtt;
    // Uncomment one of these transports and then change it in fromConnectionString to test other transports
    // const Protocol = require('azure-iot-device-amqp').AmqpWs;
    // const Protocol = require('azure-iot-device-http').Http;
    // const Protocol = require('azure-iot-device-amqp').Amqp;
    // const Protocol = require('azure-iot-device-mqtt').MqttWs;
    const Client = require('azure-iot-device').Client;
    const Message = require('azure-iot-device').Message;

    // String containing Hostname, Device Id & Device Key in the following formats:
    //  "HostName=<iothub_host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>"
    const deviceConnectionString = process.env.DEVICE_CONNECTION_STRING;
    let sendInterval;

    function disconnectHandler () {
    clearInterval(sendInterval);
    client.open().catch((err) => {
        console.error(err.message);
    });
    }

    // The AMQP and HTTP transports have the notion of completing, rejecting or abandoning the message.
    // For example, this is only functional in AMQP and HTTP:
    // client.complete(msg, printResultFor('completed'));
    // If using MQTT calls to complete, reject, or abandon are no-ops.
    // When completing a message, the service that sent the C2D message is notified that the message has been processed.
    // When rejecting a message, the service that sent the C2D message is notified that the message won't be processed by the device. the method to use is client.reject(msg, callback).
    // When abandoning the message, IoT Hub will immediately try to resend it. The method to use is client.abandon(msg, callback).
    // MQTT is simpler: it accepts the message by default, and doesn't support rejecting or abandoning a message.
    function messageHandler (msg) {
    console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
    client.complete(msg, printResultFor('completed'));
    }

    function generateMessage () {
    const windSpeed = 10 + (Math.random() * 4); // range: [10, 14]
    const temperature = 20 + (Math.random() * 10); // range: [20, 30]
    const humidity = 60 + (Math.random() * 20); // range: [60, 80]
    const data = JSON.stringify({ deviceId: 'myFirstDevice', windSpeed: windSpeed, temperature: temperature, humidity: humidity });
    const message = new Message(data);
    message.properties.add('temperatureAlert', (temperature > 28) ? 'true' : 'false');
    return message;
    }

    function errorCallback (err) {
    console.error(err.message);
    }

    function connectCallback () {
    console.log('Client connected');
    // Create a message and send it to the IoT Hub every two seconds
    sendInterval = setInterval(() => {
        const message = generateMessage();
        console.log('Sending message: ' + message.getData());
        client.sendEvent(message, printResultFor('send'));
    }, 2000);

    }

    // fromConnectionString must specify a transport constructor, coming from any transport package.
    let client = Client.fromConnectionString(deviceConnectionString, Protocol);

    client.on('connect', connectCallback);
    client.on('error', errorCallback);
    client.on('disconnect', disconnectHandler);
    client.on('message', messageHandler);

    client.open()
    .catch(err => {
    console.error('Could not connect: ' + err.message);
    });

    // Helper function to print results in the console
    function printResultFor(op) {
    return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
    };
    }
    ```

Dado que el código de Node.js envía un mensaje de telemetría simulado desde el dispositivo al centro de IoT, el mensaje aparece en el shell de la CLI que supervisa los eventos:

```output
event:
  component: ''
  interface: ''
  module: ''
  origin: <your device name>
  payload: '{"deviceId":"myFirstDevice","windSpeed":11.853592092144627,"temperature":22.62484121157508,"humidity":66.17960805575937}'
```

El dispositivo ya está conectado de forma segura y envía datos de telemetría a Azure IoT Hub.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si ya no necesita los recursos de Azure creados en esta guía de inicio rápido, puede usar la CLI de Azure para eliminarlos.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

Para eliminar un grupo de recursos por el nombre:
1. Ejecute el comando [az group delete](/cli/azure/group#az-group-delete). Este comando quita el grupo de recursos, el centro de IoT y el registro del dispositivo que ha creado.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Ejecute el comando [az group list](/cli/azure/group#az-group-list) para confirmar que se ha eliminado el grupo de recursos.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido un flujo de trabajo básico de una aplicación de Azure IoT para conectar un dispositivo a la nube de forma segura y enviar datos de telemetría del dispositivo a la nube. Ha utilizado la CLI de Azure para crear un centro de IoT y un dispositivo simulado y, después, ha utilizado el SDK de Azure IoT para Node.js para acceder al dispositivo y enviar datos de telemetría al centro. 

Como paso siguiente, explore el SDK de Azure IoT para Node.js mediante ejemplos de aplicaciones.

- [Más ejemplos de Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples): este directorio contiene más ejemplos del repositorio del SDK para Node.js que muestran escenarios de IoT Hub.