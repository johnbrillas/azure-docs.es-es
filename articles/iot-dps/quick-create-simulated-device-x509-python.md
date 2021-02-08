---
title: 'Inicio rápido: Aprovisionamiento de un dispositivo X.509 simulado en Azure IoT Hub mediante Python'
description: 'Inicio rápido de Azure: Creación y aprovisionamiento de un dispositivo X.509 simulado mediante el SDK de dispositivos de Python para IoT Hub Device Provisioning Service (DPS). En esta guía de inicio rápido se utilizan inscripciones individuales.'
author: wesmc7777
ms.author: wesmc
ms.date: 01/29/2021
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: python
ms.custom: mvc, devx-track-python
ms.openlocfilehash: c151f78c6164cc62aac618a141a26eb1da574e3c
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99218389"
---
# <a name="quickstart-create-and-provision-a-simulated-x509-device-using-python-device-sdk-for-iot-hub-device-provisioning-service"></a>Inicio rápido: Creación y aprovisionamiento de un dispositivo X.509 simulado mediante el SDK de dispositivos Python para el servicio IoT Hub Device Provisioning

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

En este inicio rápido, aprovisionará una máquina de desarrollo como un dispositivo Python X.509. Se usará el código de dispositivo de ejemplo del [SDK de Python de Azure IoT](https://github.com/Azure/azure-iot-sdk-python) para conectar el dispositivo al centro de IoT. En este ejemplo se usa una inscripción individual con Device Provisioning Service (DPS).

## <a name="prerequisites"></a>Requisitos previos

- Estar familiarizado con los conceptos de [aprovisionamiento](about-iot-dps.md#provisioning-process).
- Finalización de [Configuración de Azure IoT Hub Device Provisioning Service con Azure Portal](./quick-setup-auto-provision.md).
- Una cuenta de Azure con una suscripción activa. [cree una de forma gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Python 3.5.3 o versiones posteriores](https://www.python.org/downloads/)
- [Git](https://git-scm.com/download/).


[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Preparación del entorno 

1. Asegúrese de que `git` está instalado en su máquina y se agrega a las variables de entorno accesibles para la ventana de comandos. Consulte las [herramientas de cliente de Git de Software Freedom Conservancy](https://git-scm.com/download/) para instalar la versión más reciente de las herramientas `git`, lo que incluye **Git Bash**, la aplicación de línea de comandos que puede usar para interactuar con su repositorio de Git local. 

2. Abra un símbolo del sistema de Git Bash. Clone el repositorio de GitHub del [SDK de Azure IoT para Python](https://github.com/Azure/azure-iot-sdk-python).
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python.git --recursive
    ```


## <a name="create-a-self-signed-x509-device-certificate"></a>Creación de un certificado de dispositivo X.509 autofirmado 

En esta sección, creará un certificado X.509 autofirmado. Es importante tener en cuenta los siguientes puntos:

* Los certificados autofirmados son solo para la realización de pruebas, no se deben usar en producción.
* La fecha de expiración predeterminada de un certificado autofirmado es de un año.

Si aún no tiene los certificados de dispositivo para autenticar un dispositivo, puede crear un certificado autofirmado con OpenSSL para realizar pruebas con este artículo.  OpenSSL se incluye con la instalación de Git. 

1. Ejecute el siguiente comando en el símbolo del sistema de Git Bash.

    # <a name="windows"></a>[Windows](#tab/windows)
    
    ```bash
    winpty openssl req -outform PEM -x509 -sha256 -newkey rsa:4096 -keyout ./python-device.key.pem -out ./python-device.pem -days 365 -extensions usr_cert -subj "//CN=Python-device-01"
    ```

    > [!IMPORTANT]
    > La barra diagonal adicional que se proporciona para el nombre de firmante (`//CN=Python-device-01`) solo es necesaria para escapar la cadena con Git en plataformas Windows. 

    # <a name="linux"></a>[Linux](#tab/linux)
    
    ```bash
    openssl req -outform PEM -x509 -sha256 -newkey rsa:4096 -keyout ./python-device.key.pem -out ./python-device.pem -days 365 -extensions usr_cert -subj "/CN=Python-device-01"
    ```
    
    ---
    
2. Cuando se le pida **Enter PEM pass phrase:** (Escriba la frase de contraseña de PEM:), use el valor `1234` para realizar pruebas con este artículo.    

3. Cuando se le pregunte de nuevo **Verifying - Enter PEM pass phrase:** (Verificando: escriba la fase de contraseña de PEM), use de nuevo la frase de contraseña `1234`.    

Se genera un archivo de certificado de prueba (*python-device.pem*) y un archivo de clave privada (*python-device.key.pem*) en el directorio donde ejecutó el comando `openssl`.


## <a name="create-an-individual-enrollment-entry-in-dps"></a>Creación de una entrada de inscripción individual en DPS


Azure IoT Hub Device Provisioning Service admite dos tipos de inscripciones:

- [Grupos de inscripción](concepts-service.md#enrollment-group): usados para inscribir varios dispositivos relacionados.
- [Inscripciones individuales](concepts-service.md#individual-enrollment): usadas para inscribir un solo dispositivo.

En este artículo se muestra una inscripción de un dispositivo que se va a aprovisionar con un centro de IoT.

1. Inicie sesión en Azure Portal, seleccione el botón **Todos los recursos** del menú de la izquierda y abra el servicio de aprovisionamiento.

2. En el menú de Device Provisioning Service, seleccione **Administrar inscripciones**. Seleccione la pestaña **Inscripciones individuales** y seleccione el botón **Agregar inscripción individual** de la parte superior. 

3. En el panel **Agregar inscripción**, escriba la siguiente información:
   - Seleccione **X.509** como *Mecanismo* de atestación de identidad.
   - En el *archivo principal de certificado .pem o .cer*, elija *Seleccione un archivo* para seleccionar el archivo de certificado **python-device.pem** si va a usar el certificado de prueba creado anteriormente.
   - De forma opcional, puede proporcionar la siguiente información:
     - Seleccione un centro de IoT vinculado con el servicio de aprovisionamiento.
     - Actualice el **Estado inicial del dispositivo gemelo** con la configuración inicial deseada para el dispositivo.
   - Una vez completado, presione el botón **Guardar**. 

     [![Agregar inscripción individual para la atestación X.509 en el portal](./media/python-quick-create-simulated-device-x509/device-enrollment.png)](./media/python-quick-create-simulated-device-x509/device-enrollment.png#lightbox)

   Después de una inscripción correcta, el dispositivo X.509 aparece como **Python-device-01** en la columna *Id. de registro* de la pestaña *Inscripciones individuales*. Este valor de registro procede del nombre del firmante en el certificado de dispositivo. 

## <a name="simulate-the-device"></a>Simulación del dispositivo

El ejemplo de aprovisionamiento de Python, [provision_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/provision_x509.py), se encuentra en el directorio `azure-iot-sdk-python/azure-iot-device/samples/async-hub-scenarios`. En este ejemplo se usan seis variables de entorno para autenticar y aprovisionar un dispositivo IoT mediante DPS. Estas variables de entorno son:

| Nombre de la variable              | Descripción                                     |
| :------------------------- | :---------------------------------------------- |
| `PROVISIONING_HOST`        |  Este valor es el punto de conexión global que se usa para conectarse al recurso de DPS |    
| `PROVISIONING_IDSCOPE`     |  Este valor es el ámbito de identificador del recurso de DPS. |    
| `DPS_X509_REGISTRATION_ID` |  Este valor es el identificador del dispositivo. También debe coincidir con el nombre del firmante en el certificado del dispositivo. |    
| `X509_CERT_FILE`           |  El nombre de archivo del certificado del dispositivo. |    
| `X509_KEY_FILE`            |  El nombre de archivo de la clave privada del certificado del dispositivo. |
| `PASS_PHRASE`              |  La frase de contraseña usada para cifrar el certificado y el archivo de clave privada (`1234`). |    

1. En el menú de Device Provisioning Service, seleccione **Información general**. Anote los valores _Ámbito de id._ y _Punto de conexión global del servicio_.

    ![Información del servicio](./media/python-quick-create-simulated-device-x509/extract-dps-endpoints.png)

2. En el símbolo del sistema de Git Bash, use los siguientes comandos para agregar las variables de entorno del punto de conexión global del dispositivo y el ámbito de identificador.

    ```bash
    $export PROVISIONING_HOST=global.azure-devices-provisioning.net
    $export PROVISIONING_IDSCOPE=<ID scope for your DPS resource>
    ```

3. El identificador de registro del dispositivo IoT debe coincidir con el nombre del firmante que aparece en su certificado de dispositivo. Si ha generado un certificado de prueba autofirmado, `Python-device-01` es el nombre del firmante y el identificador de registro del dispositivo. 

    Si ya tiene un certificado de dispositivo, puede usar `certutil` para comprobar el nombre común del firmante que se usa para el dispositivo, como se muestra a continuación para un certificado de prueba autofirmado:

    ```bash
    $ certutil python-device.pem
    X509 Certificate:
    Version: 3
    Serial Number: fa33152fe1140dc8
    Signature Algorithm:
        Algorithm ObjectId: 1.2.840.113549.1.1.11 sha256RSA
        Algorithm Parameters:
        05 00
    Issuer:
        CN=Python-device-01
      Name Hash(sha1): 1dd88de40e9501fb64892b698afe12d027011000
      Name Hash(md5): a62c784820daa931b9d3977739b30d12
    
     NotBefore: 1/29/2021 7:05 PM
     NotAfter: 1/29/2022 7:05 PM
    
    Subject:
        ===> CN=Python-device-01 <===
      Name Hash(sha1): 1dd88de40e9501fb64892b698afe12d027011000
      Name Hash(md5): a62c784820daa931b9d3977739b30d12
    ```

    En el símbolo del sistema de Git Bash, establezca la variable de entorno del identificador de registro como se indica a continuación:

    ```bash
    $export DPS_X509_REGISTRATION_ID=Python-device-01
    ```

4. En el símbolo del sistema de Git Bash, establezca las variables de entorno del archivo de certificado, el archivo de clave privada y la frase de contraseña.

    ```bash
    $export X509_CERT_FILE=./python-device.pem
    $export X509_KEY_FILE=./python-device.key.pem
    $export PASS_PHRASE=1234
    ```

5. Revise el código del archivo [provision_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/provision_x509.py) Si no usa **Python versión 3.7** o posterior, realice el [cambio en el código mencionado aquí](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios#advanced-iot-hub-scenario-samples-for-the-azure-iot-hub-device-sdk) para reemplazar `asyncio.run(main())` y guarde el cambio. 

6. Ejecute el ejemplo. El ejemplo se conectará, se aprovisionará el dispositivo en un centro y se enviarán algunos mensajes de prueba al centro.

    ```bash
    $ winpty python azure-iot-sdk-python/azure-iot-device/samples/async-hub-scenarios/provision_x509.py
    RegistrationStage(RequestAndResponseOperation): Op will transition into polling after interval 2.  Setting timer.
    The complete registration result is
    Python-device-01
    TestHub12345.azure-devices.net
    initialAssignment
    null
    Will send telemetry from the provisioned device
    sending message #4
    sending message #7
    sending message #2
    sending message #8
    sending message #5
    sending message #9
    sending message #1
    sending message #6
    sending message #10
    sending message #3
    done sending message #4
    done sending message #7
    done sending message #2
    done sending message #8
    done sending message #5
    done sending message #9
    done sending message #1
    done sending message #6
    done sending message #10
    done sending message #3
    ```

7. En el portal, vaya al centro de IoT vinculado a su servicio de aprovisionamiento y abra la hoja **Dispositivos de IoT** ubicada en la sección **Exploradores** del menú de la izquierda. En el aprovisionamiento correcto del dispositivo simulado X.509 con el Hub, su identificador de dispositivo aparece en la hoja **Device Explorer** con el *ESTADO* **habilitado**. Es posible que deba presionar el botón **Actualizar** de la parte superior si ha abierto la hoja antes de ejecutar la aplicación de ejemplo del dispositivo. 

    ![El dispositivo se registra con el centro de IoT](./media/python-quick-create-simulated-device-x509/registration.png) 

> [!NOTE]
> Si ha cambiado el valor predeterminado de *Estado inicial del dispositivo gemelo* en la entrada de inscripción para el dispositivo, el dispositivo puede extraer el estado gemelo deseado desde el centro y actuar en consecuencia. Para más información, consulte [Información y uso de dispositivos gemelos en IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).
>

## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea seguir trabajando con el ejemplo de cliente de dispositivo y explorándolo, no limpie los recursos que se crean en este inicio rápido. Si no va a continuar, use el siguiente comando para eliminar todos los recursos que se han creado en este inicio rápido.

1. Cierre la ventana de salida de ejemplo del cliente del dispositivo en su máquina.
2. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y seleccione Device Provisioning Service. Abra la hoja **Administrar inscripciones** de su servicio y, después, seleccione la pestaña **Inscripciones individuales**. Active la casilla situada junto al campo *ID. DE REGISTRO* del dispositivo que ha inscrito en este inicio rápido y presione el botón **Eliminar** situado en la parte superior del panel. 
3. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y después su centro de IoT. Abra la hoja **Dispositivos IoT** de su centro, active la casilla junto al *identificador de dispositivo* del dispositivo que registró en este inicio rápido y, luego, presione el botón **Eliminar** situado en la parte superior del panel.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un dispositivo simulado X.509 en una máquina de desarrollo y lo ha aprovisionado en un centro de IoT mediante Azure IoT Hub Device Provisioning Service en el portal. Para aprender a inscribir un dispositivo X.509 mediante programación, pase al inicio rápido para la inscripción de dispositivos X.509 mediante programación. 

> [!div class="nextstepaction"]
> [Inicio rápido de Azure: Inscripción de dispositivos X.509 en Azure IoT Hub Device Provisioning Service](quick-enroll-device-x509-python.md)
