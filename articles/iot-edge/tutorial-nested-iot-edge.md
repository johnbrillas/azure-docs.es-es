---
title: 'Tutorial: Creación de una jerarquía de dispositivos IoT Edge con Azure IoT Edge'
description: En este tutorial se muestra cómo crear una estructura jerárquica de dispositivos IoT Edge mediante puertas de enlace.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 2/26/2021
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: f1b1a94dc1d96e625947eef5730c24f080fc155a
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101721419"
---
# <a name="tutorial-create-a-hierarchy-of-iot-edge-devices-preview"></a>Tutorial: Creación de una jerarquía de dispositivos IoT Edge (versión preliminar)

Implemente nodos de Azure IoT Edge en redes organizadas en capas jerárquicas. Cada capa de una jerarquía es un dispositivo de puerta de enlace que controla los mensajes y las solicitudes de los dispositivos de la capa que se encuentra debajo.

>[!NOTE]
>Esta característica requiere IoT Edge versión 1.2, la cual se encuentra en versión preliminar pública, y la ejecución de contenedores Linux.

Puede estructurar una jerarquía de dispositivos para que solo la capa superior tenga conectividad a la nube y las capas inferiores solo puedan comunicarse con las capas superior e inferior adyacentes. Estas capas de red constituyen la base de la mayoría de las redes industriales que siguen la [norma ISA-95](https://en.wikipedia.org/wiki/ANSI/ISA-95).

El objetivo de este tutorial es crear una jerarquía de dispositivos IoT Edge que simula un entorno de producción. Al final, implementará el [módulo del sensor de temperatura simulado](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.simulated-temperature-sensor) en un dispositivo de nivel inferior sin acceso a Internet mediante la descarga de imágenes de contenedor a través de la jerarquía.

Para lograr este objetivo, este tutorial le guiará a través de la creación de una jerarquía de dispositivos IoT Edge, la implementación de contenedores del entorno de ejecución de Azure IoT Edge en los dispositivos y la configuración local de estos. En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Crear y definir las relaciones en una jerarquía de dispositivos IoT Edge.
> * Configurar el entorno de ejecución de Azure IoT Edge en los dispositivos de la jerarquía.
> * Instalar certificados coherentes en la jerarquía de dispositivos.
> * Agregar cargas de trabajo a los dispositivos de la jerarquía.
> * Use el [módulo de proxy de API de IoT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.azureiotedge-api-proxy?tab=Overview) para enrutar de forma segura el tráfico HTTP por un único puerto desde los dispositivos de nivel inferior.

En este tutorial, se definen los siguientes niveles de red:

* **Nivel superior**: Los dispositivos IoT Edge de este nivel se pueden conectar directamente a la nube.

* **Capas inferiores**: los dispositivos IoT Edge en las capas situadas debajo de la superior no se pueden conectar directamente a la nube. Deben pasar por uno o más dispositivos IoT Edge intermediarios para enviar y recibir datos.

En este tutorial se usa una jerarquía de dos dispositivos para simplificar, mostrada a continuación. El **dispositivo de nivel superior** representa un dispositivo en la capa superior de la jerarquía, que se puede conectar directamente a la nube. También se hace referencia a este dispositivo como **dispositivo primario**. El otro dispositivo, el **dispositivo de nivel inferior**, representa un dispositivo en la capa inferior de la jerarquía, que no se puede conectar directamente a la nube. Puede agregar dispositivos de nivel inferior adicionales para representar el entorno de producción. Los dispositivos de las capas inferiores también se denominarán **dispositivos secundarios**. La configuración de cualquier dispositivo de nivel inferior adicional sigue la configuración del **dispositivo de nivel inferior**.

![Estructura de la jerarquía del tutorial, que contiene dos dispositivos: el de nivel superior y el de nivel inferior](./media/tutorial-nested-iot-edge/tutorial-hierarchy-diagram.png)

## <a name="prerequisites"></a>Requisitos previos

Para crear una jerarquía de dispositivos IoT Edge, necesitará:

* Un equipo (Windows o Linux) con conectividad a Internet.
* Una cuenta de Azure con una suscripción válida. Si no tiene una [suscripción a Azure](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing), cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure.
* La versión v2.3.1 de la CLI de Azure con la extensión v0.10.6 de Azure IoT u otra versión posterior. Este tutorial usa [Azure Cloud Shell](../cloud-shell/overview.md). Si no está familiarizado con Azure Cloud Shell, [consulte un inicio rápido para más información](./quickstart-linux.md#prerequisites).
  * Para ver las versiones actuales de los módulos y extensiones de la CLI de Azure, ejecute [az version](/cli/azure/reference-index?#az_version).
* Un dispositivo Linux para configurar como dispositivo IoT Edge para cada dispositivo de la jerarquía. En este tutorial se usan dos dispositivos. Si no tiene dispositivos disponibles, puede crear dos máquinas virtuales de Azure para cada dispositivo; para ello, reemplace el texto de marcador de posición del comando siguiente y ejecútelo:

   ```azurecli-interactive
   az vm create \
    --resource-group <REPLACE_WITH_RESOURCE_GROUP> \
    --name <REPLACE_WITH_UNIQUE_NAMES_FOR_EACH_VM> \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password <REPLACE_WITH_PASSWORD>
   ```

* Asegúrese de que los puertos siguientes están abiertos para el tráfico entrante: 8000, 443, 5671, 8883:
  * 8000: se usa para extraer las imágenes de contenedor de Docker mediante el proxy de API.
  * 443: se usa entre los centros de conectividad perimetrales primario y secundario para las llamadas de API REST.
  * 5671, 8883: se usan para AMQP y MQTT.

  Para obtener más información, vea [Apertura de puertos en una máquina virtual con Azure Portal](../virtual-machines/windows/nsg-quickstart-portal.md).

>[!TIP]
>Si quiere una visión automatizada de la configuración de una jerarquía de dispositivos IoT Edge, puede seguir el [ejemplo Azure IoT Edge para IoT industrial](https://aka.ms/iotedge-nested-sample) con scripts. En este escenario con scripts se implementan máquinas virtuales de Azure como dispositivos preconfigurados para simular el entorno de una fábrica.
>
>Si quiere continuar con la creación de la jerarquía de ejemplo paso a paso, continúe con los pasos del tutorial siguientes.

## <a name="configure-your-iot-edge-device-hierarchy"></a>Configuración de la jerarquía de dispositivos IoT Edge

### <a name="create-a-hierarchy-of-iot-edge-devices"></a>Creación de una jerarquía de dispositivos IoT Edge

Los dispositivos IoT Edge componen las capas de la jerarquía. En este tutorial se creará una jerarquía de dos dispositivos IoT Edge: el **dispositivo de nivel superior** y su secundario, el **dispositivo de nivel inferior**. Puede crear dispositivos secundarios adicionales según sea necesario.

Para crear los dispositivos IoT Edge, puede usar Azure Portal o la CLI de Azure.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://ms.portal.azure.com/), vaya a IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Seleccione **+ Agregar un dispositivo IoT Edge**. Este dispositivo será el dispositivo de nivel superior, por lo que debe especificar un identificador de dispositivo único adecuado. Seleccione **Guardar**.

1. Seleccione **+ Agregar un dispositivo IoT Edge** de nuevo. Este será un dispositivo de nivel inferior, por lo que debe especificar un identificador de dispositivo único adecuado.

1. Elija **Establecer un dispositivo primario**, elija el dispositivo de nivel superior en la lista de dispositivos y seleccione **Aceptar**. Seleccione **Guardar**.

   ![Configuración del elemento primario para el dispositivo de nivel inferior](./media/tutorial-nested-iot-edge/set-parent-device.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un dispositivo IoT Edge en el centro de conectividad. Este dispositivo será el dispositivo de nivel superior, por lo que debe especificar un identificador de dispositivo único adecuado:

   ```azurecli-interactive
   az iot hub device-identity create --device-id {top_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

   La creación correcta de un dispositivo generará la configuración JSON del dispositivo.

   ![Salida JSON de la creación correcta de un dispositivo](./media/tutorial-nested-iot-edge/json-success-output.png)

1. Escriba el comando siguiente para crear un dispositivo IoT Edge de nivel inferior. Puede crear más de un dispositivo de nivel inferior si quiere más capas en la jerarquía. Asegúrese de proporcionar identidades de dispositivo únicas a cada uno.

   ```azurecli-interactive
   az iot hub device-identity create --device-id {lower_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

1. Escriba el comando siguiente para definir la relación de todos los elementos principal y secundario entre el **dispositivo de nivel superior** y cada **dispositivo de nivel inferior**. Asegúrese de ejecutar este comando para cada dispositivo de nivel inferior de la jerarquía.

   ```azurecli-interactive
   az iot hub device-identity parent set --device-id {lower_layer_device_id} --parent-device-id {top_layer_device_id} --hub-name {hub_name}
   ```

   Este comando no tiene una salida explícita.

---

Después, anote la cadena de conexión de cada dispositivo IoT Edge. Se usarán más adelante.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://ms.portal.azure.com/), vaya a la sección **IoT Edge** de IoT Hub.

1. Haga clic en el identificador de dispositivo de uno de los dispositivos de la lista.

1. Seleccione **Copiar** en el campo **Cadena de conexión principal** y guárdelo en un lugar de su elección.

1. Repita el procedimiento con el resto de dispositivos.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), para cada dispositivo, escriba el siguiente comando para recuperar la cadena de conexión del dispositivo y guardarla en un lugar de su elección:

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id {device_id} --hub-name {hub_name}
   ```

---

Si ha completado correctamente los pasos anteriores, puede usar los siguientes para comprobar que las relaciones de elementos principales y secundarios son correctas en Azure Portal o la CLI de Azure.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://ms.portal.azure.com/), vaya a la sección **IoT Edge** de IoT Hub.

1. Haga clic en el id. de dispositivo de un **dispositivo de nivel inferior** en la lista de dispositivos.

1. En la página de detalles del dispositivo, debería ver la identidad del **dispositivo de nivel superior** junto al campo **Dispositivo principal**.

   [Dispositivo principal confirmado por el dispositivo secundario](./media/tutorial-nested-iot-edge/lower-layer-device-parent.png)

También puede vincular la relación de jerarquía mediante el **dispositivo de nivel superior**.

1. Haga clic en el id. de dispositivo de un **dispositivo de nivel superior** en la lista de dispositivos.

1. Seleccione la pestaña **Administrar dispositivos secundarios** en la parte superior.

1. En la lista de dispositivos, debería ver el **dispositivo de nivel inferior**.

   [Dispositivo secundario confirmado por el dispositivo principal](./media/tutorial-nested-iot-edge/top-layer-device-child.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/) puede comprobar que cualquiera de los dispositivos secundarios ha establecido correctamente su relación con el principal si obtiene la identidad del dispositivo principal confirmado del dispositivo secundario. Esto generará la configuración JSON del dispositivo principal, que se muestra arriba:

   ```azurecli-interactive
   az iot hub device-identity parent show --device-id {lower_layer_device_id} --hub-name {hub_name}
   ```

También puede vincular la relación de jerarquía mediante la consulta del **dispositivo de nivel superior**.

1. Enumere los dispositivos secundarios conocidos para el dispositivo principal:

    ```azurecli-interactive
    az iot hub device-identity children list --device-id {top_layer_device_id} --hub-name {hub_name}
    ```

---

Una vez que esté satisfecho con la estructura correcta de la jerarquía, está listo para continuar.

### <a name="create-certificates"></a>Creación de certificados

Todos los dispositivos de un [escenario de puerta de enlace](iot-edge-as-gateway.md) necesitan un certificado compartido para configurar conexiones seguras entre ellos. Use los pasos siguientes para crear certificados de demostración para los dispositivos de este escenario.

Para crear certificados de demostración en un dispositivo Linux, debe clonar los scripts de generación y configurarlos para que se ejecuten localmente en bash.

1. Clone el repositorio de Git de IoT Edge que contiene los scripts para generar los certificados de demostración.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

1. Vaya al directorio en el que quiere trabajar. Todos los archivos de certificados y claves se crearán en este directorio.

1. Copie los archivos de configuración y de script del repositorio de IoT Edge clonado en el directorio de trabajo.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

1. Cree el certificado de entidad de certificación raíz y un certificado intermedio.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Este comando de script crea varios archivos de certificado y clave, pero nosotros vamos a usar el siguiente archivo como **certificado de entidad de certificación raíz** para la jerarquía de la puerta de enlace:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

1. Cree dos conjuntos de certificados y claves privadas de entidad de certificación para los dispositivos IoT Edge con el comando siguiente: un conjunto para el dispositivo del nivel superior y otro para el del inferior. Proporcione nombres fáciles de recordar para que los certificados de la entidad de certificación puedan distinguirlos entre sí.

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "{top_layer_certificate_name}"
   ./certGen.sh create_edge_device_ca_certificate "{lower_layer_certificate_name}"
   ```

   Este comando de script crea varios archivos de certificado y clave, pero se usará el siguiente par de certificado y clave en cada dispositivo IoT Edge, a lo que se hace referencia en el archivo de configuración:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Cada dispositivo necesita una copia del certificado de la entidad de certificación raíz y una copia de su propio certificado y clave privada. Puede usar una unidad USB o una [copia de archivo seguro](https://www.ssh.com/ssh/scp/) para trasladar los certificados a cada dispositivo.

1. Una vez transferidos los certificados, instale la entidad de certificación raíz para cada dispositivo.

   ```bash
   sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```

   La salida de este comando debe indicar que se ha agregado un certificado en `/etc/ssl/certs`.

   [Mensaje de instalación correcta del certificado](./media/tutorial-nested-iot-edge/certificates-success-output.png)

Si ha completado correctamente los pasos anteriores, puede comprobar que los certificados se han instalado en `/etc/ssl/certs`; para ello, vaya a ese directorio y busque los certificados instalados.

1. Vaya a `/etc/ssl/certs`:

   ```bash
   cd /etc/ssl/certs
   ```

1. Enumere los certificados instalados y `grep` para `azure`:

   ```bash
   ll | grep azure
   ```

   Debería ver un hash de certificado vinculado al certificado de CA raíz y el certificado de CA raíz vinculado a la copia en el directorio `usr/local/share`.

   [Resultados de la búsqueda de certificados de Azure](./media/tutorial-nested-iot-edge/certificate-list-results.png)

Una vez que esté satisfecho con los certificados instalados en cada dispositivo, está listo para continuar.

### <a name="install-iot-edge-on-the-devices"></a>Instalación de IoT Edge en los dispositivos

La instalación de imágenes del entorno de ejecución de IoT Edge versión 1.2 proporciona a los dispositivos acceso a las características necesarias para funcionar como una jerarquía de dispositivos.

Para instalar IoT Edge, debe instalar la configuración de repositorios adecuada, los requisitos previos y los recursos de versión necesarios.

1. Instale la configuración del repositorio que coincida con el sistema operativo del dispositivo.

   * **Ubuntu Server 18.04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Raspberry Pi OS Stretch**:

     ```bash
     curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
     ```

1. Copie la lista generada en el directorio sources.list.d.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

1. Instale la clave pública de GPG de Microsoft.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

1. Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

1. Instale el motor de Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

1. Instale IoT Edge y el servicio de identidad de IoT.

   ```bash
   sudo apt-get install aziot-edge
   ```

Si ha completado correctamente los pasos anteriores, ha visto el mensaje de banner de Azure IoT Edge en el que se le solicita que actualice el archivo de configuración de Azure IoT Edge, `/etc/aziot/config.toml`, en todos los dispositivos de la jerarquía. Si es así, está listo para continuar.

### <a name="configure-the-iot-edge-runtime"></a>Configuración del runtime de IoT Edge

Además del aprovisionamiento de los dispositivos, los pasos de configuración establecen una comunicación de confianza entre los dispositivos de la jerarquía mediante los certificados que ha creado antes. Los pasos también comienzan a establecer la estructura de red de la jerarquía. El dispositivo de nivel superior mantendrá la conectividad a Internet, lo que le permitirá extraer imágenes de su entorno de ejecución de la nube, mientras que los dispositivos de nivel inferior se enrutarán mediante el de nivel superior para acceder a estas imágenes.

Para configurar el entorno de ejecución de Azure IoT Edge, debe modificar varios componentes del archivo de configuración. Las configuraciones difieren ligeramente entre el **dispositivo de nivel superior** y un **dispositivo de nivel inferior**, por lo que debe tener en cuenta el archivo de configuración del dispositivo que edite en cada paso. La configuración del entorno de ejecución de Azure IoT Edge para los dispositivos consta de cuatro pasos que se realizan todos mediante la edición del archivo de configuración de IoT Edge:

* Para aprovisionar manualmente cada dispositivo agregue la cadena de conexión del dispositivo al archivo de configuración.

* Termine de configurar los certificados del dispositivo; para ello, apunte el archivo de configuración hacia el certificado, la clave privada y el certificado de la entidad de certificación raíz del dispositivo.

* Arranque el sistema con el agente de IoT Edge.

* Actualice el parámetro **hostname** del dispositivo del **nivel superior** y actualice los parámetros **hostname** y **parent_hostname** de los dispositivos del **nivel inferior**.

Complete estos pasos y reinicie el servicio IoT Edge para configurar los dispositivos.

>[!TIP]
>Al navegar por el archivo de configuración en Nano, puede presionar **Ctrl + W** para buscar palabras clave en el archivo.

1. En cada dispositivo, cree un archivo de configuración en función de la plantilla incluida.

   ```bash
   sudo cp /etc/aziot/config.toml.edge.template /etc/aziot/config.toml

1. On each device, open the IoT Edge configuration file.

   ```bash
   sudo nano /etc/aziot/config.toml
   ```

1. Para el dispositivo de **nivel superior**, busque la sección **Nombre de host**. Quite la marca de comentario de la línea con el parámetro `hostname` y actualice el valor para que sea el nombre de dominio completo (FQDN) o la dirección IP del dispositivo IoT Edge. Use el valor que elija de forma coherente en los dispositivos de la jerarquía.

   ```toml
   hostname = "<device fqdn or IP>"
   ```

   >[!TIP]
   >Para pegar el contenido del portapapeles en Nano: `Shift+Right Click` o presione `Shift+Insert`.

1. Para cualquier dispositivo IoT Edge en **capas inferiores**, busque la sección **Parent hostname** (Nombre de host principal). Quite la marca de comentario de la línea con el `parent_hostname` parámetro y actualice el valor para que apunte al FQDN o la dirección IP del dispositivo principal. Use el valor exacto que haya colocado en el campo **hostname** del dispositivo principal. Para el dispositivo IoT Edge del **nivel superior**, deje este parámetro como comentario.

   ```toml
   parent_hostname = "<parent device fqdn or IP>"
   ```

   > [!NOTE]
   > En el caso de las jerarquías con más de un nivel inferior, actualice el campo *parent_hostname* con el FQDN del dispositivo del nivel inmediatamente inferior.

1. Busque la sección **Trust bundle cert** (Confiar en el certificado de la agrupación) del archivo. Quite la marca de comentario de la línea con el `trust_bundle_cert` parámetro y actualice el valor con la ruta de acceso del URI del archivo para el certificado de CA raíz compartido por todos los dispositivos de la jerarquía de puerta de enlace.

   ```toml
   trust_bundle_cert = "<root CA certificate>"
   ```

1. Busque la sección **Provisioning** (Aprovisionamiento) del archivo. Quite la marca de comentario de las líneas **Manual provisioning with connection string** (Aprovisionamiento manual con cadena de conexión). En cada dispositivo de la jerarquía, actualice el valor de **connection_string** con la cadena de conexión del dispositivo IoT Edge.

   ```toml
   # Manual provisioning with connection string
   [provisioning]
   source = "manual"
   connection_string: "<Device connection string>"
   ```

1. Busque la sección **Default Edge Agent** (Agente perimetral predeterminado).

   * Para el dispositivo de **nivel superior**, actualice el valor de imagen edgeAgent a la versión preliminar pública del módulo en Azure Container Registry.
   
     ```toml
     [agent.config]
     image = "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4"
     ```

   * Para cada dispositivo IoT Edge en **niveles inferiores**, actualice la imagen de edgeAgent para que apunte al dispositivo principal seguido del puerto en el que escucha el proxy de API. En la sección siguiente implementará el módulo de proxy de API en el dispositivo principal.
   
     ```toml
     [agent.config]
     image = "<parent hostname value>:8000/azureiotedge-agent:1.2.0-rc4"
     ```

1. Busque la sección **Edge CA certificate** (Certificado de CA perimetral). Quite los comentarios de las tres primeras líneas de esta sección. Después, actualice los dos parámetros para que apunten a los archivos de clave privada de CA de dispositivo y de certificado de la CA de dispositivo que ha creado en la sección anterior y que se han movido al dispositivo IoT Edge. Proporcione las rutas de acceso del URI del archivo, que adoptan el formato `file:///<path>/<filename>`, como `file:///certs/iot-edge-device-ca-top-layer-device.key.pem`.

   ```yml
   [edge_ca]
   cert = "<File URI path to the device full chain CA certificate unique to this device.>"
   pk = "<File URI path to the device CA private key unique to this device.>"
   ```

   >[!NOTE]
   >Asegúrese de usar la ruta de acceso del certificado de CA de **cadena completa** y el nombre de archivo para rellenar el campo `device_ca_cert`.

1. Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

1. Después de escribir la información de aprovisionamiento en el archivo de configuración, aplique los cambios:

   ```bash
   sudo iotedge config apply
   ```

Antes de continuar, asegúrese de que ha actualizado el archivo de configuración de cada uno de los dispositivos de la jerarquía. En función de la estructura de la jerarquía, ha configurado un **dispositivo de nivel superior** y uno o varios **dispositivos de nivel inferior**.

Si ha completado correctamente los pasos anteriores, puede comprobar que los dispositivos estén bien configurados.

1. Ejecute las comprobaciones de configuración y conectividad en los dispositivos:

   ```bash
   sudo iotedge check
   ```

En el **dispositivo de nivel superior**, espere ver una salida con varias evaluaciones que se superan y al menos una advertencia. La comprobación de `latest security daemon` le advertirá de que otra versión de IoT Edge es la versión estable más reciente, ya que la versión 1.2 de IoT Edge está en versión preliminar pública. Es posible que vea advertencias adicionales sobre directivas de registro y, en función de la red, directivas de DNS.

En un **dispositivo de nivel inferior**, espere ver una salida similar a la del dispositivo de nivel superior, pero con una advertencia adicional que indica que el módulo EdgeAgent no se puede extraer del flujo ascendente. Esto es aceptable, ya que el módulo de proxy de API de IoT Edge y el de Docker Container Registry, en el que los dispositivos de nivel inferior extraerán las imágenes, todavía no se han implementado en el **dispositivo de nivel superior**.

A continuación se muestra una salida de ejemplo de `iotedge check`:

[Resultados de configuración y conectividad de ejemplo](./media/tutorial-nested-iot-edge/configuration-and-connectivity-check-results.png)

Cuando esté satisfecho con las configuraciones correctas en cada dispositivo, ya está listo para continuar.

## <a name="deploy-modules-to-the-top-layer-device"></a>Implementación de módulos en el dispositivo de nivel superior

Los módulos sirven para completar la implementación y el entorno de ejecución de IoT Edge en los dispositivos y definir la estructura de la jerarquía. El módulo de proxy de API de IoT Edge enruta de forma segura el tráfico HTTP por un único puerto desde los dispositivos de nivel inferior. El módulo Docker Registry permite un repositorio de imágenes de Docker al que pueden acceder los dispositivos de nivel inferior mediante el enrutamiento de extracciones de imágenes al dispositivo de nivel superior.

Para implementar módulos en el dispositivo de nivel superior, puede usar Azure Portal o la CLI de Azure.

>[!NOTE]
>Los pasos restantes para completar la configuración del entorno de ejecución de Azure IoT Edge e implementar cargas de trabajo no se realizan en los dispositivos IoT Edge.

# <a name="portal"></a>[Portal](#tab/azure-portal)

En el [Portal de Azure](https://ms.portal.azure.com/):

1. Vaya a su instancia de IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Haga clic en el id. de dispositivo del dispositivo perimetral del **nivel superior** en la lista de dispositivos.

1. En la barra superior, seleccione **Establecer módulos**.

1. Seleccione la opción **Configuración del entorno de ejecución**, situada junto al icono de engranaje.

1. En **Centro de Microsoft Edge**, en el campo de imagen, escriba `mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4`.

   ![Edición de la imagen del Centro de Microsoft Edge](./media/tutorial-nested-iot-edge/edge-hub-image.png)

1. Agregue las siguientes variables de entorno al módulo del Centro de Microsoft Edge:

    | Nombre | Value |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

   ![Edición de las variables de entorno del Centro de Microsoft Edge](./media/tutorial-nested-iot-edge/edge-hub-environment-variables.png)

1. En **Agente de Edge**, en el campo de imagen, escriba `mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4`. Seleccione **Guardar**.

1. Agregue el módulo del registro de Docker al dispositivo de nivel superior. Seleccione **+ Agregar** y elija **Módulo IoT Edge** en la lista desplegable. Proporcione el nombre `registry` para el módulo del registro de Docker y escriba `registry:latest` para el URI de la imagen. A continuación, agregue variables de entorno y cree opciones que apunten el módulo del registro local hacia el registro de contenedor de Microsoft desde el que descargar las imágenes de contenedor y proporcionar estas imágenes en el registro: 5000.

1. En la pestaña Variables de entorno, escriba el siguiente par nombre-valor de la variable de entorno:

    | Nombre | Value |
    | - | - |
    | `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

1. En la pestaña Opciones de creación del contenedor, escriba el siguiente JSON:

   ```json
   {
    "HostConfig": {
        "PortBindings": {
            "5000/tcp": [
                {
                    "HostPort": "5000"
                }
            ]
         }
      }
   }
   ```

1. A continuación, agregue el módulo de proxy de API al dispositivo del nivel superior. Seleccione **+ Agregar** y elija **Módulo de Marketplace** en la lista desplegable. Busque `IoT Edge API Proxy` y seleccione el módulo. El proxy de API de IoT Edge usa el puerto 8000 y está configurado para usar el módulo del registro denominado `registry` en el puerto 5000 de forma predeterminada.

1. Seleccione **Revisar y crear** y, luego, **Crear** para completar la implementación. El entorno de ejecución de Azure IoT Edge del dispositivo del nivel superior, que tiene acceso a Internet, extraerá y ejecutará las configuraciones de la **versión preliminar pública** para el centro y el agente de IoT Edge.

   ![Implementación finalizada que contiene el centro de Edge, el agente de Edge, el módulo del registro y el módulo de proxy de API](./media/tutorial-nested-iot-edge/complete-top-layer-deployment.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un archivo deployment.json:

   ```azurecli-interactive
   code deploymentTopLayer.json
   ```

1. Copie el contenido del JSON siguiente en el archivo deployment.json, guárdelo y ciérrelo.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "dockerContainerRegistry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "https://mcr.microsoft.com"
                               } 
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       },
                       "IoTEdgeAPIProxy": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-api-proxy",
                               "createOptions": "{\"HostConfig\": {\"PortBindings\": {\"8000/tcp\": [{\"HostPort\":\"8000\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "NGINX_DEFAULT_PORT": {
                                   "value": "8000"
                               },
                               "DOCKER_REQUEST_ROUTE_ADDRESS": {
                                   "value": "registry:5000"
                               },
                               "BLOB_UPLOAD_ROUTE_ADDRESS": {
                                   "value": "AzureBlobStorageonIoTEdge:11002"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Escriba el siguiente comando para crear una implementación en el dispositivo perimetral del nivel superior:

   ```azurecli-interactive
   az iot edge set-modules --device-id <top_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentTopLayer.json
   ```

---

Si ha completado correctamente los pasos anteriores, el **dispositivo de nivel superior** debe informar de los cuatro módulos: el módulo de proxy de API de IoT Edge, el módulo Docker Container Registry y los módulos del sistema, como se **especifica en implementación**. Es posible que el dispositivo tarde unos minutos en recibir su nueva implementación e iniciar los módulos. Actualice la página hasta que vea el módulo de sensor de temperatura mostrado como **Notificado por el dispositivo**. Una vez que el dispositivo notifique los módulos, estará listo para continuar.

## <a name="deploy-modules-to-the-lower-layer-device"></a>Implementación de módulos en el dispositivo de nivel inferior

Los módulos también sirven como cargas de trabajo de los dispositivos de nivel inferior. El módulo del sensor de temperatura simulado crea datos de telemetría de ejemplo para proporcionar un flujo de datos funcional mediante la jerarquía de dispositivos.

Para implementar módulos en el dispositivo de nivel inferior, puede usar Azure Portal o la CLI de Azure.

# <a name="portal"></a>[Portal](#tab/azure-portal)

En el [Portal de Azure](https://ms.portal.azure.com/):

1. Vaya a su instancia de IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Haga clic en el id. del dispositivo del nivel inferior en la lista de dispositivos IoT Edge.

1. En la barra superior, seleccione **Establecer módulos**.

1. Seleccione la opción **Configuración del entorno de ejecución**, situada junto al icono de engranaje.

1. En **Centro de Microsoft Edge**, en el campo de imagen, escriba `$upstream:8000/azureiotedge-hub:1.2.0-rc4`.

1. Agregue las siguientes variables de entorno al módulo del Centro de Microsoft Edge:

    | Nombre | Value |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

1. En **Agente de Edge**, en el campo de imagen, escriba `$upstream:8000/azureiotedge-agent:1.2.0-rc4`. Seleccione **Guardar**.

1. Agregue el módulo del sensor de temperatura. Seleccione **+ Agregar** y elija **Módulo de Marketplace** en la lista desplegable. Busque `Simulated Temperature Sensor` y seleccione el módulo.

1. En **Módulos de IoT Edge**, seleccione el módulo `Simulated Temperature Sensor` que acaba de agregar y actualice su URI de imagen para que apunte a `$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0`.

1. Seleccione **Guardar**, **Revisar y crear**, y **Crear** para completar la implementación.

   ![Implementación finalizada que contiene el centro de Edge, el agente de Edge y el sensor de temperatura simulado](./media/tutorial-nested-iot-edge/complete-lower-layer-deployment.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un archivo deployment.json:

   ```azurecli-interactive
   code deploymentLowerLayer.json
   ```

1. Copie el contenido del JSON siguiente en el archivo deployment.json, guárdelo y ciérrelo.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "simulatedTemperatureSensor": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0",
                               "createOptions": ""
                           },
                           "type": "docker",
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-hub:1.2.0-rc4",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Escriba el siguiente comando para crear una implementación de módulos establecida en el dispositivo del nivel inferior:

   ```azurecli-interactive
   az iot edge set-modules --device-id <lower_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentLowerLayer.json

---

Notice that the image URI that we used for the simulated temperature sensor module pointed to `$upstream:8000` instead of to a container registry. We configured this device to not have direct connections to the cloud, because it's in a lower layer. To pull container images, this device requests the image from its parent device instead. At the top layer, the API proxy module routes this container request to the registry module, which handles the image pull.

If you completed the above steps correctly, your **lower layer device** should report three modules: the temperature sensor module and the system modules, as **Specified in Deployment**. It may take a few minutes for the device to receive its new deployment, request the container image, and start the module. Refresh the page until you see the temperature sensor module listed as **Reported by Device**. Once the modules are reported by the device, you are ready to continue.

## Troubleshooting

Run the `iotedge check` command to verify the configuration and to troubleshoot errors.

You can run `iotedge check` in a nested hierarchy, even if the child machines don't have direct internet access.

When you run `iotedge check` from the lower layer, the program tries to pull the image from the parent through port 443.

In this tutorial, we use port 8000, so we need to specify it:

```bash
sudo iotedge check --diagnostics-image-name <parent_device_fqdn_or_ip>:8000/azureiotedge-diagnostics:1.2.0-rc4
```

El valor de `azureiotedge-diagnostics` se extrae del registro de contenedor vinculado al módulo del registro. En este tutorial se ha establecido de forma predeterminada en https://mcr.microsoft.com:.

| Nombre | Valor |
| - | - |
| `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

Si utiliza un registro de contenedor privado, asegúrese de que todas las imágenes (como IoTEdgeAPIProxy, edgeAgent, edgeHub y diagnostics) estén presentes en él.

## <a name="view-generated-data"></a>Visualización de datos generados

El módulo **Simulated Temperature Sensor** que insertó genera datos de entorno de ejemplo. Envía mensajes con la temperatura y la humedad ambiental, la temperatura y la presión de la máquina, y una marca de tiempo.

Puede ver los mensajes que llegan a su centro de IoT mediante la [extensión de Azure IoT Hub para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

También puede ver estos mensajes mediante [Azure Cloud Shell](https://shell.azure.com/):

   ```azurecli-interactive
   az iot hub monitor-events -n <iothub_name> -d <lower-layer-device-name>
   ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo.

Para eliminar los recursos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Grupos de recursos**.

2. Seleccione el nombre del grupo de recursos que contiene los recursos de prueba de IoT Edge. 

3. Revise la lista de los recursos contenidos en el grupo de recursos. Si desea eliminar todos ellos, puede seleccionar **Eliminar grupo de recursos**. Si desea eliminar solo algunos de ellos, puede hacer clic en cada recurso para eliminarlos individualmente. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configuró dos dispositivos IoT Edge como puertas de enlace y estableció uno como el dispositivo primario del otro. Después, ha mostrado la extracción de una imagen de contenedor en el dispositivo secundario a través de una puerta de enlace.

Para ver cómo Azure IoT Edge puede crear más soluciones para la empresa, siga con los otros tutoriales.

> [!div class="nextstepaction"]
> [Implementación de un modelo de Azure Machine Learning como módulo](tutorial-deploy-machine-learning.md)
