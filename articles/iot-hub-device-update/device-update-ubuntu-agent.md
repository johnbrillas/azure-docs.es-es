---
title: Tutorial sobre Device Update para Azure IoT Hub mediante el agente de paquetes de Ubuntu Server 18.04 (x64) | Microsoft Docs
description: Introducción a Device Update para Azure IoT Hub mediante el agente de paquetes de Ubuntu Server 18.04 (x64).
author: vimeht
ms.author: vimeht
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: 19be3b141fb663ea0f4f11d811bf6ffc33252504
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101659057"
---
# <a name="device-update-for-azure-iot-hub-tutorial-using-the-ubuntu-server-1804-x64-package-agent"></a>Tutorial sobre Device Update para Azure IoT Hub mediante el agente de paquetes de Ubuntu Server 18.04 (x64)

La actualización de dispositivos para IoT Hub admite dos formas de actualizaciones: basada en imágenes y basada en paquetes.

Las actualizaciones basadas en paquetes son actualizaciones dirigidas que solo modifican una aplicación o un componente específicos en el dispositivo. Esto conduce a un menor consumo de ancho de banda y ayuda a reducir el tiempo necesario para descargar e instalar la actualización. Normalmente, las actualizaciones de paquetes implican menos tiempo de inactividad de los dispositivos al aplicar una actualización y evitan la sobrecarga asociada a la creación de imágenes.

Este tutorial le guiará por los pasos necesarios para completar una actualización basada en paquetes de un extremo a otro mediante la actualización de dispositivos para IoT Hub. En este tutorial, usaremos un agente de paquetes de ejemplo para Ubuntu Server 18.04 (x64). Incluso si planea usar una configuración diferente para la plataforma de sistema operativo, este tutorial sigue siendo útil para obtener información sobre las herramientas y los conceptos asociados a la actualización de dispositivos para IoT Hub. Complete esta introducción al proceso de actualización de un extremo a otro y, a continuación, elija su forma preferida de actualizar una plataforma de sistema operativo para profundizar en los detalles. Puede usar el servicio Device Update para Azure IoT Hub para actualizar un dispositivo de Azure IoT o Azure IoT Edge de Azure mediante este tutorial. 

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Configurar el repositorio de paquetes de actualización de dispositivos
> * Descargar e instalar el agente de actualización de dispositivos y sus dependencias
> * Agregar una etiqueta a su dispositivo IoT
> * Importación de una actualización
> * Creación de un grupo de dispositivos
> * Implementar una actualización del paquete
> * Supervisar la implementación de actualizaciones

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos
* Acceso a IoT Hub. Se recomienda usar un nivel S1 (estándar) o superior.
* Un dispositivo de Azure IoT o Azure IoT Edge que ejecuta Ubuntu Server 18.04 (x64), conectado a IoT Hub.
   * Si usa un dispositivo Azure IoT Edge, asegúrese de que utilice la versión 1.2.0, o posterior, del entorno de ejecución de Edge. 
* Si no usa un dispositivo Azure IoT Edge, [instale el paquete `aziot-identity-service` (versión preliminar) más reciente en el dispositivo IoT](https://github.com/Azure/iot-identity-service/actions/runs/575919358). 
* [La instancia y la cuenta de actualización de dispositivos que están vinculadas a la instancia de IoT Hub anterior.](create-device-update-account.md)

## <a name="configure-device-update-package-repository"></a>Configurar el repositorio de paquetes de actualización de dispositivos

1. Instale la configuración del repositorio que coincida con el sistema operativo del dispositivo. En este tutorial, será Ubuntu Server 18.04. 
   
   ```shell
      curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

2. Copie la lista generada en el directorio sources.list.d.

   ```shell
      sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```
   
3. Instale la clave pública de GPG de Microsoft.

   ```shell
      curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
      sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-device-update-deb-agent-packages"></a>Instalación de paquetes del agente .deb de actualización de dispositivos

1. Actualización de las listas de paquetes en el dispositivo

   ```shell
      sudo apt-get update
   ```

2. Instalación del paquete deviceupdate-agent y sus dependencias

   ```shell
      sudo apt-get install deviceupdate-agent deliveryoptimization-plugin-apt
   ```

Los paquetes de software de Device Update para Azure IoT Hub están sujetos a los siguientes términos de licencia:
   * [Licencia de actualización de dispositivos para IoT Hub](https://github.com/Azure/iot-hub-device-update/blob/main/LICENSE.md)
   * [Licencia de cliente de optimización de distribución](https://github.com/microsoft/do-client/blob/main/LICENSE.md)
   
Lea los términos de licencia antes de usar el paquete. La instalación y el uso de un paquete constituyen la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="configure-device-update-agent-using-azure-iot-identity-service-preview"></a>Configuración de Device Update Agent mediante el servicio Azure IoT Identity Service (versión preliminar)

Una vez que tenga instalados los paquetes necesarios, debe aprovisionar el dispositivo con su identidad de nube e información de autenticación.

1. Apertura del archivo de configuración

   ```shell
      sudo nano /etc/aziot/config.toml
   ```

2. Busque la sección de configuración de aprovisionamiento del archivo. Quite la marca de comentario de la sección de "aprovisionamiento manual con cadena de conexión". Actualice el valor de connection_string con la cadena de conexión del dispositivo IoT (Edge). Asegúrese de que todas las demás secciones de aprovisionamiento estén comentadas.


   ```toml
      # Manual provisioning configuration using a connection string
      [provisioning]
      source = "manual"
      iothub_hostname = "<REQUIRED IOTHUB HOSTNAME>"
      device_id = "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
      dynamic_reprovisioning = false 
   ```

3. Guardado y cierre del archivo mediante Ctrl+X, Y

4. Aplique la configuración. 

   Si usa un dispositivo IoT Edge, ejecute el siguiente comando. 
   
   ```shell
      sudo iotedge config apply
   ```
   
   Si usa un dispositivo IoT, con el paquete `aziot-identity-service` instalado, ejecute el siguiente comando. 
      
   ```shell
      sudo aziotctl config apply
   ```

5. Como alternativa, compruebe que los servicios se están ejecutando

    ```shell
       sudo systemctl list-units --type=service | grep 'adu-agent\.service\|deliveryoptimization-agent\.service'
    ```

    La salida debería indicar:

    ```markdown
       adu-agent.service                   loaded active running Device Update for IoT Hub Agent daemon.

       deliveryoptimization-agent.service               loaded active running deliveryoptimization-agent.service: Performs content delivery optimization tasks   `
    ```

## <a name="add-a-tag-to-your-device"></a>Incorporación de una etiqueta al dispositivo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al centro de IoT.

2. En "Dispositivos de IoT" o "IoT Edge" en el panel de navegación izquierdo, busque el dispositivo de IoT y vaya a hasta el dispositivo gemelo.

3. En Dispositivo gemelo, elimine cualquier valor de etiqueta de Device Update existente. Para ello, establézcalo en NULL.

4. Agregue un nuevo valor de etiqueta de Device Update como se muestra a continuación.

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

## <a name="import-update"></a>Importación de una actualización

1. Descargue el [archivo de manifiesto APT](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-apt-manifest.json) y el [archivo de manifiesto de importación](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.1-importManifest.json) siguientes. Este archivo de manifiesto APT instalará la versión más reciente de `libcurl4-doc package` en el dispositivo ioT. 

   Como alternativa, descargue el [archivo de manifiesto APT](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-7.58-apt-manifest.json) y el [archivo de manifiesto de importación](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-2-2.0.1-importManifest.json). De este modo, se instalará la versión 7.58.0 específica de `libcurl4-doc package` en el dispositivo IoT. 

2. En Azure Portal, seleccione la opción Actualizaciones del dispositivo en Administración de dispositivos automática, en la barra de navegación izquierda de IoT Hub.

3. Seleccione la pestaña Actualizaciones.

4. Seleccione "+ Import New Update" (Importar nueva actualización).

5. Seleccione el icono de la carpeta o el cuadro de texto en "Select an Import Manifest File" (Seleccionar un archivo de manifiesto de importación). Verá un cuadro de diálogo para seleccionar archivos. Seleccione el manifiesto de importación que descargó anteriormente. Seleccione el icono de la carpeta o el cuadro de texto en "Select one or more update files" (Seleccionar uno o varios archivos de importación). Verá un cuadro de diálogo para seleccionar archivos. Seleccione el archivo de actualización del manifiesto APT que descargó anteriormente.
   
   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Captura de pantalla que muestra la selección del archivo de actualización." lightbox="media/import-update/select-update-files.png":::

6. Seleccione el icono de la carpeta o el cuadro de texto en "Select a storage container" (Seleccionar un contenedor de almacenamiento). Luego, seleccione la cuenta de almacenamiento adecuada.

7. Si ya ha creado un contenedor, puede volver a usarlo (de lo contrario, seleccione "+ Contenedor" para crear contenedor de almacenamiento para las actualizaciones).  Seleccione el contenedor que desee usar y haga clic en "Seleccionar".

   :::image type="content" source="media/import-update/container.png" alt-text="Captura de pantalla que muestra la selección del contenedor." lightbox="media/import-update/container.png":::

8. Seleccione "Enviar" para iniciar el proceso de importación.

9. Se inicia el proceso de importación y la pantalla cambia a la sección "Historial de importación". Seleccione "Actualizar" para ver el progreso hasta que se complete el proceso de importación. En función del tamaño de la actualización, puede completarse en unos minutos, pero puede tardar más tiempo.
   
   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Captura de pantalla que muestra la secuencia de importación de la actualización." lightbox="media/import-update/update-publishing-sequence-2.png":::

10. Cuando la columna Estado indique que la importación se ha realizado correctamente, seleccione el encabezado "Ready to Deploy" (Listo para implementar). Debería ver la actualización importada en la lista.

[Más información](import-update.md) sobre la importación de actualizaciones.

## <a name="create-update-group"></a>Creación de un grupo de actualización

1. Vaya a la instancia de IoT Hub que conectó anteriormente a la instancia de Device Update.

2. Seleccione la opción Actualizaciones del dispositivo en Administración de dispositivos automática en la barra de navegación izquierda.

3. Seleccione la pestaña Groups (Grupos) en la parte superior de la página. 

4. Seleccione el botón Add (Agregar) para crear un grupo.

5. Seleccione en la lista la etiqueta IoT Hub que ha creado en el paso anterior. Seleccione Create update group (Crear grupo de actualización).

   :::image type="content" source="media/create-update-group/select-tag.PNG" alt-text="Captura de pantalla que muestra la selección de etiquetas." lightbox="media/create-update-group/select-tag.PNG":::

[Más información](create-update-group.md) sobre cómo agregar etiquetas y crear grupos de actualización


## <a name="deploy-update"></a>Implementación de la actualización

1. Una vez creado el grupo, debería ver que hay una actualización nueva disponible para el grupo de dispositivos, con un vínculo a la actualización en Actualizaciones pendientes. Es posible que tenga que actualizar una vez. 

2. Haga clic en la actualización disponible.

3. Confirme que se ha seleccionado el grupo correcto como grupo de destino. Programe la implementación y, a continuación, seleccione Deploy update (Implementar actualización).

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Select update" lightbox="media/deploy-update/select-update.png"::: (Seleccionar actualización)

4. Consulte el gráfico de compatibilidad. Debería ver que la actualización está en curso. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Actualización en curso" lightbox="media/deploy-update/update-in-progress.png":::

5. Una vez que el dispositivo se haya actualizado correctamente, debería ver que el gráfico de compatibilidad y la actualización de los detalles de implementación reflejan lo mismo. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Actualización correcta" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Supervisión de una implementación de actualizaciones

1. Seleccione la pestaña Implementaciones en la parte superior de la página.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Pestaña Implementaciones" lightbox="media/deploy-update/deployments-tab.png":::

2. Seleccione la implementación que creó para ver los detalles de la implementación.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Detalles de implementación" lightbox="media/deploy-update/deployment-details.png":::

3. Seleccione Actualizar para ver los detalles de estado más recientes. Continúe con este proceso hasta que el estado cambie a Correcto.

Ha completado correctamente una actualización de la imagen de un extremo a otro mediante Device Update para IoT Hub en un dispositivo Ubuntu Server 18.04 (x64). 

## <a name="bonus-steps"></a>Pasos adicionales

1. Descargue el [archivo de manifiesto APT](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-remove-apt-manifest.json) y el [archivo de manifiesto de importación](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.2-importManifest.json) siguientes. Este archivos de manifiesto APT quitará la versión instalada de `libcurl4-doc package` del dispositivo IoT. 

2. Repita los pasos de las secciones "Importación de una actualización" e "Implementación de una actualización".

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, limpie la cuenta y la instancia de Device Update, el centro de IoT y el dispositivo IoT. Para ello, vaya a cada recurso individual y seleccione "Delete" (Eliminar). Tenga en cuenta que antes de limpiar la cuenta de actualización del dispositivo, debe limpiar la instancia de actualización del dispositivo. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de actualización de imágenes en Raspberry PI 3 B+](device-update-raspberry-pi.md)

