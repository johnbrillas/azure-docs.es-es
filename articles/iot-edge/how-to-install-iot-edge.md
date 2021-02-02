---
title: Instalación de Azure IoT Edge | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en dispositivos Windows o Linux
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/20/2021
ms.author: kgremban
ms.openlocfilehash: ab783d6cb20f1c2fe31e8556dc57999df20d5637
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629817"
---
# <a name="install-or-uninstall-azure-iot-edge-for-linux"></a>Instalación o desinstalación de Azure IoT Edge para Linux

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. Para obtener más información, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se indican los pasos para instalar el entorno de ejecución de Azure IoT Edge en dispositivos Linux.

## <a name="prerequisites"></a>Requisitos previos

* Un [identificador de dispositivo registrado](how-to-register-device.md)

  Si registró el dispositivo con la autenticación de clave simétrica, tenga lista la cadena de conexión del dispositivo.

  Si registró el dispositivo con la autenticación de certificado autofirmado X.509, debe tener al menos uno de los certificados de identidad que usó para registrar el dispositivo y su clave privada correspondiente en el dispositivo.

* Un dispositivo Linux

  Debe tener un dispositivo Linux X64, ARM32 o ARM64. Microsoft ofrece paquetes de instalación para los sistemas operativos Ubuntu Server 16.04, Ubuntu Server 18.04 y Raspberry Pi OS Stretch.

  Para obtener la información más reciente sobre qué sistemas operativos se admiten actualmente en los escenarios de producción, consulte [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems).

  >[!NOTE]
  >La compatibilidad con dispositivos ARM64 está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

* Prepare el dispositivo para acceder a los paquetes de instalación de Microsoft.

  Instale la configuración del repositorio que coincida con el sistema operativo del dispositivo.

  * **Ubuntu Server 16.04**:

    ```bash
    curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
    ```

  * **Ubuntu Server 18.04**:

    ```bash
    curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
    ```

  * **Raspberry Pi OS Stretch**:

    ```bash
    curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
    ```

  Copie la lista generada en el directorio sources.list.d.

  ```bash
  sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
  ```

  Instale la clave pública de GPG de Microsoft.

  ```bash
  curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
  sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
  ```

Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia que se encuentran cada paquete (`usr/share/doc/{package-name}` o el directorio `LICENSE`). Lea los términos de licencia antes de usar el paquete. La instalación y el uso de un paquete constituyen la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-a-container-engine"></a>Instalación de un motor del contenedor

Azure IoT Edge utiliza un runtime de contenedor compatible con OCI. En los escenarios de producción, se recomienda utilizar el motor de Moby. El motor Moby es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de contenedor de Docker CE/EE son totalmente compatibles con el entorno de ejecución de Moby.

Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

Instale el motor de Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

Si recibe algún error al instalar el motor del contenedor Moby, compruebe la compatibilidad con Moby del kernel de Linux. Algunos fabricantes de dispositivos incrustados distribuyen imágenes de dispositivos que contienen kernels de Linux personalizados sin las características necesarias para la compatibilidad del motor del contenedor. Ejecute el siguiente comando, que usa el [script check-config](https://github.com/moby/moby/blob/master/contrib/check-config.sh) suministrado por Moby, para comprobar la configuración del kernel:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

En la salida del script, compruebe que todos los elementos que figuran en `Generally Necessary` y `Network Drivers` estén habilitados. Si faltan características, puede habilitarlas volviendo a generar el kernel a partir del origen y seleccionando los módulos asociados para incluirlos en el archivo .config del kernel adecuado. Igualmente, si usa un generador de configuración de kernel como `defconfig` o `menuconfig`, busque y habilite las características correspondientes y vuelva a generar el kernel como corresponda. Una vez que haya implementado el kernel recién modificado, vuelva a ejecutar el script check-config para comprobar que se han habilitado correctamente todas las características necesarias.

## <a name="install-the-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de IoT Edge

El demonio de seguridad de IoT Edge proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo.

Los pasos de esta sección representan el proceso habitual para instalar la versión más reciente en un dispositivo que tenga conexión a Internet. Si tiene que instalar una versión específica, como una versión preliminar, o tiene que realizar la instalación mientras está sin conexión, siga los pasos de [Instalación sin conexión o de una versión específica](#offline-or-specific-version-installation-optional), en la sección siguiente.

Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

Compruebe qué versiones de IoT Edge están disponibles.

   ```bash
   apt list -a iotedge
   ```

Si quiere instalar la versión más reciente del demonio de seguridad, use el siguiente comando, que también instala la versión más reciente del paquete **libiothsm-std**:

   ```bash
   sudo apt-get install iotedge
   ```

O bien, si quiere instalar una versión específica del demonio de seguridad, especifique la versión en la salida de apt list. Especifique también la misma versión para el paquete **libiothsm-std**, que de lo contrario instalaría su versión más reciente. Por ejemplo, el siguiente comando instala la versión más reciente de la versión 1.0.10:

   ```bash
   sudo apt-get install iotedge=1.0.10* libiothsm-std=1.0.10*
   ```

Si la versión que quiere instalar no aparece en la lista, siga los pasos que se describen en [Instalación sin conexión o de una versión específica](#offline-or-specific-version-installation-optional), más adelante en este artículo. En esta sección se muestra cómo seleccionar cualquier versión anterior del demonio de seguridad de IoT Edge o versiones candidatas para lanzamiento.

## <a name="provision-the-device-with-its-cloud-identity"></a>Aprovisionamiento del dispositivo con su identidad de nube

Ahora que el motor del contenedor y el entorno de ejecución de IoT Edge están instalados en el dispositivo, está preparado para el paso siguiente, que consiste en configurar el dispositivo con su información de identidad de nube y autenticación.

Elija la siguiente sección en función del tipo de autenticación que quiera usar:

* [Opción 1: Autenticación con claves simétricas](#option-1-authenticate-with-symmetric-keys)
* [Opción 2: Autenticación con certificados X.509](#option-2-authenticate-with-x509-certificates)

### <a name="option-1-authenticate-with-symmetric-keys"></a>Opción 1: Autenticación con claves simétricas

En este momento, el entorno de ejecución de IoT Edge se instala en el dispositivo Linux y debe aprovisionar el dispositivo con su información de identidad de nube y autenticación.

En esta sección se explican los pasos necesarios para aprovisionar un dispositivo con la autenticación de clave simétrica. Debe haber registrado el dispositivo en IoT Hub y recuperar la cadena de conexión de la información del dispositivo. Si no es así, siga los pasos que se describen en [Registro de un dispositivo IoT Edge en IoT Hub](how-to-register-device.md).

Abra el archivo de configuración en el dispositivo IoT Edge.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Busque las configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección **Manual provisioning configuration using a connection string** (Configuración del aprovisionamiento manual mediante una cadena de conexión).

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
     dynamic_reprovisioning: false
   ```

Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge. Asegúrese de que las demás secciones de aprovisionamiento estén comentadas. Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

Para pegar el contenido del portapapeles en Nano: `Shift+Right Click` o presione `Shift+Insert`.

Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

   ```bash
   sudo systemctl restart iotedge
   ```

### <a name="option-2-authenticate-with-x509-certificates"></a>Opción 2: Autenticación con certificados X.509

En este momento, el entorno de ejecución de IoT Edge se instala en el dispositivo Linux y debe aprovisionar el dispositivo con su información de identidad de nube y autenticación.

En esta sección se explican los pasos necesarios para aprovisionar un dispositivo con la autenticación de certificados X.509. Debe haber registrado el dispositivo en IoT Hub y proporcionado huellas digitales que coincidan con el certificado y la clave privada que se encuentran en el dispositivo IoT Edge. Si no es así, siga los pasos que se describen en [Registro de un dispositivo IoT Edge en IoT Hub](how-to-register-device.md).

Abra el archivo de configuración en el dispositivo IoT Edge.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Busque la sección de configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección **Manual provisioning configuration using an X.509 identity certificate** (Configuración de aprovisionamiento manual mediante un certificado de identidad X.509). Asegúrese de que las demás secciones de aprovisionamiento estén comentadas. Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     authentication:
       method: "x509"
       iothub_hostname: "<REQUIRED IOTHUB HOSTNAME>"
       device_id: "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
       identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
       identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
     dynamic_reprovisioning: false
   ```

Actualice los campos siguientes:

* **iothub_hostname**: nombre de host del centro de IoT al que se conectará el dispositivo. Por ejemplo, `{IoT hub name}.azure-devices.net`.
* **device_id**: el id. que proporcionó al registrar el dispositivo.
* **identity_cert**: URI a un certificado de identidad en el dispositivo. Por ejemplo, `file:///path/identity_certificate.pem`.
* **identity_pk**: URI al archivo de clave privada para el certificado de identidad proporcionado. Por ejemplo, `file:///path/identity_key.pem`.

Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

   ```bash
   sudo systemctl restart iotedge
   ```

## <a name="verify-successful-configuration"></a>Comprobación de configuración correcta

Compruebe que el runtime se ha instalado y configurado correctamente en el dispositivo de IoT Edge.

1. Compruebe que el demonio de seguridad de IoT Edge se ejecuta como un servicio del sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   >[!TIP]
   >Necesita privilegios elevados para ejecutar comandos `iotedge`. Cuando cierre la sesión en su máquina y la inicie de nuevo por primera vez después de instalar el entorno de ejecución de IoT Edge, sus permisos se actualizarán automáticamente. Hasta entonces, use `sudo` delante de los comandos.

2. Si necesita solucionar problemas del servicio, recupere los registros del servicio.

   ```bash
   journalctl -u iotedge
   ```

3. Use la herramienta `check` para comprobar el estado de configuración y conexión del dispositivo.

   ```bash
   sudo iotedge check
   ```

   >[!TIP]
   >Use siempre `sudo` para ejecutar la herramienta de comprobación, incluso después de que se actualicen los permisos. Para comprobar el estado de la configuración, la herramienta necesita privilegios elevados para acceder al archivo **config.yaml**.

4. Vea todos los módulos que se ejecutan en el dispositivo IoT Edge. Cuando el servicio se inicia por primera vez, solo verá la ejecución del módulo **edgeAgent**. El módulo edgeAgent se ejecuta de forma predeterminada y le ayuda a instalar e iniciar todos los módulos adicionales que implemente en el dispositivo.

   ```bash
   sudo iotedge list
   ```

## <a name="offline-or-specific-version-installation-optional"></a>Instalación sin conexión o de una versión específica (opcional)

Los pasos de esta sección corresponden a escenarios que no se tratan en los pasos de instalación estándar. Estos pueden incluir lo siguiente:

* Instalación de IoT Edge sin conexión
* Instalación de una versión candidata para lanzamiento

Siga los pasos de esta sección si quiere instalar una versión específica del entorno de ejecución de Azure IoT Edge que no esté disponible mediante `apt-get install`. La lista de paquetes de Microsoft solo contiene un conjunto limitado de versiones recientes y sus subversiones, por lo que estos pasos son para cualquiera que quiera instalar una versión anterior o una versión candidata para lanzamiento.

Con los comandos curl, puede dirigirse a los archivos de componentes directamente desde el repositorio de GitHub de IoT Edge.

1. Vaya a [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) y busque la versión de lanzamiento que desea tener como destino.

2. Expanda la sección **Recursos** para esa versión.

3. Cada versión debe tener archivos nuevos para el demonio de seguridad de IoT Edge y hsmlib. Use los comandos siguientes para actualizar esos componentes.

   1. Busque el archivo **libiothsm-std** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo.

   2. Use el vínculo copiado en el comando siguiente para instalar esa versión de hsmlib:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```

   3. Busque el archivo **iotedge** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo.

   4. Use el vínculo copiado en el comando siguiente para instalar esa versión del demonio de seguridad de IoT Edge.

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Ahora que el motor del contenedor y el entorno de ejecución de IoT Edge están instalados en el dispositivo, está preparado para el paso siguiente, que consiste en [aprovisionar el dispositivo con su identidad de nube](#provision-the-device-with-its-cloud-identity).

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge de su dispositivo, use los siguientes comandos.

Quite el entorno de ejecución de Azure IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

Al quitar el entorno de ejecución de IoT Edge, los contenedores que se hayan creado en él se detendrán, pero seguirán existiendo en el dispositivo. Observe todos los contenedores para ver cuáles permanecen.

```bash
sudo docker ps -a
```

Elimine los contenedores del dispositivo, incluidos los dos contenedores del entorno en tiempo de ejecución.

```bash
sudo docker rm -f <container name>
```

Por último, quite el entorno en tiempo de ejecución del contenedor del dispositivo.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Pasos siguientes

Siga [implementando módulos de IoT Edge](how-to-deploy-modules-portal.md) para información sobre cómo implementar módulos en el dispositivo.
