---
title: Configuración de la aplicación de asistente para voz mediante Azure IoT Hub
description: Configuración de la aplicación de asistente para voz mediante Azure IoT Hub
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: b22ef4ee0a8b5978bb2ec1c02fadf368815f3014
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102095789"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Configuración de la aplicación de asistente para voz mediante Azure IoT Hub

En este artículo se describe cómo configurar la aplicación de asistente para voz mediante IoT Hub. Para obtener un tutorial paso a paso que le guíe en el proceso de creación de un asistente para voz mediante una plantilla de demostración, consulte [Creación de un asistente para voz sin código con Azure Percept Studio y Azure Percept Audio](./tutorial-no-code-speech.md).

## <a name="update-your-voice-assistant-configuration"></a>Actualización de la configuración del asistente para voz

1. Abra [Azure Portal](https://portal.azure.com) y escriba **IoT Hub** en la barra de búsqueda. Haga clic en el icono para abrir la página de IoT Hub.

1. En la página de IoT Hub, seleccione la instancia de IoT Hub en la que se aprovisionó el dispositivo.

1. Seleccione **IoT Edge** en **Administración automática de dispositivos** en el menú de navegación izquierdo para ver todos los dispositivos conectados a su instancia de IoT Hub.

1. Seleccione el dispositivo en el que se implementó la aplicación de asistente para voz.

1. Haga clic en **Establecer módulos**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Captura de pantalla de la página de dispositivos con la opción Establecer módulos resaltada.":::

1. Compruebe que la siguiente entrada está presente en la sección **Credenciales de Container Registry**. Agregue credenciales si es necesario.

    |NOMBRE|Dirección|Nombre de usuario|Contraseña|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatepreviewpull|

1. En la sección **Módulos de IoT Edge**, seleccione **azureearspeechclientmodule**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Captura de pantalla que muestra la lista de todos los módulos de IoT Edge del dispositivo.":::

1. Haga clic en la pestaña **Configuración del módulo**. Compruebe la siguiente configuración:

    URI de imagen|Directiva de reinicio|Estado deseado
    ---------|--------------|--------------
    mcr.microsoft.com/azureedgedevices/azureearspeechclientmodule:preload-devkit|Siempre|en ejecución

    Si la configuración no coincide, edítela y haga clic en **Actualizar**.

1. Haga clic en la pestaña **Variables de entorno**. Compruebe que no haya variables de entorno definidas.

1. Haga clic en la pestaña **Configuración de módulos gemelos**. Actualice la sección **speechConfigs** como se indica a continuación:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedsamples.blob.core.windows.net/speech/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > La palabra clave usada anteriormente es una palabra clave predeterminada disponible públicamente. Si desea usar la suya propia, puede agregar su propia palabra clave personalizada cargando un archivo de tabla creado en el almacenamiento de blobs. El almacenamiento de blobs se debe configurar con acceso de contenedor anónimo o con acceso de blobs anónimo.

## <a name="how-to-find-out-appid-key-and-region"></a>Obtención del identificador de la aplicación, la clave y la región

Para obtener el **identificador de la aplicación**, la **clave** y la **región**, vaya a [Speech Studio](https://speech.microsoft.com/):

1. Inicie sesión y seleccione el recurso de voz adecuado.
1. En la página principal de **Speech Studio**, haga clic en **Comandos personalizados** en **Asistentes de voz**.
1. Seleccione el proyecto de destino.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Captura de pantalla de la página del proyecto en Speech Studio.":::

1. En el panel del menú izquierdo, haga clic en **Configuración**.
1. El **identificador de la aplicación** y la **clave** se encuentran en la pestaña de configuración **General**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Captura de pantalla de la configuración general del proyecto de voz.":::

1. Para encontrar la **región**, abra la pestaña **Recursos de LUIS** en la configuración. La selección de **Recurso de creación** contendrá la información de la región.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Captura de pantalla de los recursos de LUIS del proyecto de voz.":::

1. Después de escribir la información de **speechConfigs**, haga clic en **Actualizar**.

1. Haga clic en la pestaña **Rutas** en la parte superior de la página **Establecer módulos**. Asegúrese de tener una ruta con el siguiente valor:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Agregue la ruta si no existe.

1. Haga clic en **Revisar y crear**.

1. Haga clic en **Crear**.


## <a name="next-steps"></a>Pasos siguientes

Después de actualizar la configuración del asistente para voz, vuelva a la demostración de Azure Percept Studio para interactuar con la aplicación.

