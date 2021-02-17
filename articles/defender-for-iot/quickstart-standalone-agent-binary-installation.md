---
title: Instalación del microagente de Defender para IoT (versión preliminar)
titleSuffix: Azure Defender for IoT
description: Aprenda a instalar y autenticar el microagente de Defender.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/18/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 0841bbd8baa524d3eea3afcbffc0aa5ead41409e
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99810086"
---
# <a name="install-defender-for-iot-micro-agent-preview"></a>Instalación del microagente de Defender para IoT (versión preliminar)

En este artículo se explica cómo instalar y autenticar el microagente de Defender.

## <a name="prerequisites"></a>Prerrequisitos

Antes de instalar el módulo Defender para IoT, debe crear una identidad de módulo en IoT Hub. Para aprender a crear una identidad de módulo, consulte [Creación de un módulo gemelo de azureiotsecurity](quickstart-create-security-twin.md).

## <a name="install-the-package"></a>Instalar el paquete

Instale y configure el repositorio de paquetes de Microsoft siguiendo [estas instrucciones](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software). 

Para Debian 9, las instrucciones no incluyen el repositorio que debe agregarse; use los siguientes comandos para agregarlo: 

```azurecli
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add - 

sudo apt-get install software-properties-common

sudo apt-add-repository https://packages.microsoft.com/debian/9/multiarch/prod

sudo apt-get update
```

Para instalar el paquete de microagente de Defender en Debian y las distribuciones de Linux basadas en Ubuntu, use el siguiente comando:

```azurecli
sudo apt-get install defender-iot-micro-agent 
```

## <a name="micro-agent-authentication-methods"></a>Métodos de autenticación del microagente 

Las dos opciones que se usan para autenticar el microagente de Defender para IoT son las siguientes: 

- Cadena de conexión 

- Certificado.

### <a name="authenticate-using-a-connection-string"></a>Autenticación mediante una cadena de conexión.

Para la autenticación mediante una cadena de conexión:

1. Escriba el comando siguiente para colocar un archivo denominado `connection_string.txt` que contenga la cadena de conexión codificada en UTF-8 en la ruta de acceso `/var/defender_iot_micro_agent` del directorio del agente de Defender:

    ```azurecli
    sudo bash -c 'echo "<connection string" > /var/defender_iot_micro_agent/connection_string.txt' 
    ```

    `connection_string.txt` debe encontrarse ahora en la siguiente ubicación de ruta de acceso `/var/defender_iot_micro_agent/connection_string.txt`.

1. Reinicie el servicio con este comando:  

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service 
    ```

### <a name="authenticate-using-a-certificate"></a>Autenticación mediante un certificado

Para la autenticación mediante un certificado:

1. Siga [estas instrucciones](../iot-hub/iot-hub-security-x509-get-started.md) para conseguir un certificado.

1. Coloque la parte pública con codificación PEM del certificado y la clave privada en el directorio del agente de Defender del archivo denominado `certificate_public.pem` y `certificate_private.pem`. 

1. Copie la cadena de conexión adecuada en el archivo `connection_string.txt`. La cadena de conexión debe tener este aspecto: 

    `HostName=<the host name of the iot hub>;DeviceId=<the id of the device>;ModuleId=<the id of the module>;x509=true` 

    Esta cadena indica al agente de Defender que espere a que se proporcione un certificado para la autenticación. 

1. Reinicie el servicio mediante el comando siguiente:  

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service
    ```

### <a name="validate-your-installation"></a>Validación de la instalación

Para la validación de la instalación:

1. Asegurándose de que el microagente se ejecuta correctamente con el siguiente comando:  

    ```azurecli
    systemctl status defender-iot-micro-agent.service
    ```
1. Asegúrese de que el servicio es estable. Para ello, debe asegurarse de que está `active` y de que el tiempo de actividad del proceso es el adecuado.

    :::image type="content" source="media/quickstart-standalone-agent-binary-installation/active-running.png" alt-text="Compruebe que el servicio es estable y está activo.":::
 
## <a name="testing-the-system-end-to-end"></a>Prueba integral del sistema 

Puede probar el sistema de manera integral mediante la creación de un archivo de desencadenador en el dispositivo. El archivo de desencadenador hará que el examen de línea de base del agente detecte el archivo como infracción de línea base. 

Cree un archivo en el sistema de archivos con el comando siguiente:

```azurecli
sudo touch /tmp/DefenderForIoTOSBaselineTrigger.txt 
```
Se producirá una recomendación de error de validación de línea de base en el centro de conectividad, con un `CceId` de CIS-Debian-9-DEFENDER_FOR_IOT_TEST_CHECKS-0.0: 

:::image type="content" source="media/quickstart-standalone-agent-binary-installation/validation-failure.png" alt-text="Recomendación de error de validación de línea de base que se produce en el centro de conectividad." lightbox="media/quickstart-standalone-agent-binary-installation/validation-failure-expanded.png":::

Espere una hora como máximo para que la recomendación aparezca en el centro de conectividad. 

## <a name="micro-agent-versioning"></a>Control de versiones del microagente 

Para instalar una versión específica del microagente de Defender para IoT, ejecute el siguiente comando: 

```azurecli
sudo apt-get install defender-iot-micro-agent=<version>
```

## <a name="next-steps"></a>Pasos siguientes

[Creación del microagente de Defender a partir de código fuente](quickstart-building-the-defender-micro-agent-from-source.md)
