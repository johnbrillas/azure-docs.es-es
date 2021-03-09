---
title: Instalación de Azure Percept DK
description: Conexión del kit de desarrollo a Azure e implementación del primer modelo de inteligencia artificial
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/15/2021
ms.custom: template-quickstart
ms.openlocfilehash: 49bf89d38edef6a9186cbdb5bb89a763339385b4
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102175828"
---
# <a name="set-up-your-azure-percept-dk-and-deploy-your-first-ai-model"></a>Instalación de Azure Percept DK e implementación del primer modelo de inteligencia artificial

Comience a usar Azure Percept DK y Azure Percept Studio con la experiencia de instalación de Azure Percept DK para conectar el dispositivo a Azure e implementar su primer modelo de inteligencia artificial. Después de comprobar que la cuenta de Azure sea compatible con Azure Percept Studio, debe completar la experiencia de instalación para asegurarse de que Azure Percept DK esté instalado para crear una prueba de concepto de inteligencia artificial.

Si experimenta algún problema durante este inicio rápido, consulte la guía de [solución de problemas](./troubleshoot-dev-kit.md) para encontrar posibles soluciones.

## <a name="prerequisites"></a>Prerrequisitos

- Una instancia de Azure Percept DK.
- Un equipo host basado en Windows, Linux u OS X con funcionalidad Wi-Fi y un explorador web.
- Una cuenta de Azure con una suscripción activa. [Cree su cuenta de forma gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- La cuenta de Azure debe tener el rol "Propietario" o "Colaborador" en la suscripción. Obtenga más información sobre las [definiciones de roles de Azure](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-roles).

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

Siga estos pasos para comprobar si la cuenta de Azure tiene los roles "Propietario" o "Colaborador" en la suscripción.

1. Vaya a Azure Portal e inicie sesión con la misma cuenta de Azure que tiene pensado usar con Azure Percept Studio. 

    > [!NOTE]
    > Si tiene varias cuentas de Azure, el explorador puede almacenar en caché las credenciales de otra cuenta. Consulte la guía de solución de problemas para más información sobre cómo asegurarse de que ha iniciado sesión con la cuenta correcta.

1. Expanda el menú principal en la esquina superior izquierda de la pantalla y haga clic en "Suscripciones" o seleccione "Suscripciones" en el menú de iconos de la página principal. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-01-subscription.png" alt-text="supscription icon in Azure portal.":::
    --->
1. Seleccione la suscripción en la lista. Si no ve su suscripción en la lista, asegúrese de que ha iniciado sesión con la cuenta de Azure correcta. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-02-sub-list.png" alt-text="supscription list in Azure portal.":::
    --->
Si desea crear una suscripción, siga [estos pasos](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription).

1. En el menú Suscripción, seleccione "Control de acceso (IAM)".
1. Haga clic en el botón "View my access" (Ver mi acceso).
1. Compruebe el rol.
    - Si muestra el rol de "Lector" o si recibe un mensaje que indica que no tiene permisos para ver los roles, deberá seguir el proceso pertinente de su organización para obtener la elevación del rol de su cuenta.
    - Si muestra el rol "Propietario" o "Colaborador", la cuenta funcionará con Azure Percept Studio. 

## <a name="launch-the-azure-percept-dk-setup-experience"></a>Inicio de la experiencia de instalación de Azure Percept DK

<!---
> [!NOTE]
> Connecting over ethernet? See [this how-to guide](<link needed>) for detailed instructions.
--->
1. Conecte el equipo host directamente al punto de acceso Wi-Fi del kit de desarrollo. Esto se realiza de la misma forma que la conexión a cualquier otra red Wi-Fi.
    - **nombre de red**: scz-xxxx (donde "xxxx" son los últimos cuatro dígitos de la dirección de red MAC del kit de desarrollo)
    - **contraseña**: puede encontrarla en la tarjeta de bienvenida que se incluye en el kit de desarrollo.

    > [!WARNING]
    > Mientras está conectado al punto de acceso Wi-Fi de Azure Percept DK, el equipo host perderá temporalmente su conexión a Internet. Las llamadas de videoconferencias activas, el streaming web o cualquier otra experiencia basada en red se interrumpirán hasta que se complete el paso 3 de la experiencia de instalación de Azure Percept DK.

1. Una vez conectado al punto de acceso Wi-Fi del kit de desarrollo, el dispositivo host iniciará automáticamente la experiencia de instalación de Azure Percept DK en una nueva ventana del explorador. Si no se abre automáticamente, abra una ventana del explorador y vaya a [http://10.1.1.1](http://10.1.1.1) para iniciarla manualmente. 

1. Ahora que ha iniciado la experiencia de instalación de Azure Percept, está listo para continuar. 

    > [!NOTE]
    > El servicio web de la experiencia de instalación de Azure Percept DK se apagará después de 30 minutos sin usarse o al finalizar la experiencia de instalación. Cuando esto ocurra, se recomienda reiniciar el kit de desarrollo para evitar problemas de conectividad con el punto de acceso Wi-Fi del kit de desarrollo.

## <a name="complete-the-azure-percept-dk-setup-experience"></a>Finalización de la experiencia de instalación de Azure Percept DK

1. Primeros pasos: en la pantalla de bienvenida, haga clic en **Siguiente**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-01-welcome.png" alt-text="Página de bienvenida.":::

1. Conexión a Wi-Fi: en la página Conexión de red, haga clic en **Connect to a new WiFi network** (Conectarse a una nueva red Wi-Fi) para conectar el kit de desarrollo a la red Wi-Fi.

    Si ha conectado previamente este kit de desarrollo a la red Wi-Fi o si ya está conectado a la experiencia de instalación de Azure Percept DK a través de la red Wi-Fi, haga clic en el vínculo **Skip** (Omitir).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-02-connect-to-wi-fi.png" alt-text="Conexión a Wi-Fi.":::

1. Seleccione la red Wi-Fi entre las conexiones disponibles y conéctese. (Necesitará la contraseña de Wi-Fi local)

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-03-select-wi-fi.png" alt-text="Selección de la red Wi-Fi."::: 

1. Copie la dirección IP: una vez que el kit de desarrollo se haya conectado correctamente a la red de su elección, anote la **dirección IPv4** que aparece en la página. **Necesitará esta dirección IP más adelante en esta guía de inicio rápido**. 

    > [!NOTE]
    > La dirección IP puede cambiar con cada arranque del dispositivo.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-04-success-wi-fi.png" alt-text="Copia de la dirección IP.":::

1. Revise y acepte el contrato de licencia: lea el contrato de licencia, seleccione **I have read and agree to the License Agreement** (He leído y acepto el contrato de licencia) en la parte inferior del contrato y haga clic en **Next** (Siguiente).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-05-eula.png" alt-text="Aceptación del CLUF.":::

1. Cree su cuenta de inicio de sesión de SSH: configure SSH para el acceso remoto en el kit de desarrollo. Cree un nombre de usuario y contraseña de SSH. 

    > [!NOTE]
    > SSH (Secure Shell) es un protocolo de red que se usa para proteger la comunicación entre el dispositivo host y el kit de desarrollo. Para más información sobre SSH, consulte [este artículo](https://en.wikipedia.org/wiki/SSH_(Secure_Shell)).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-06-ssh-login.png" alt-text="Creación de una cuenta SSH."::: 

1. Comience el proceso de conexión del kit de desarrollo: en la siguiente pantalla, haga clic en **Connect with a new device** (Conectarse con un nuevo dispositivo) para empezar el proceso de conexión del kit de desarrollo a Azure IoT Hub. 

    <!---
    Connecting with an existing IoT Edge device connection string? See this [how-to guide](<link needed>) for reference.
    --->
    :::image type="content" source="./media/quickstart-percept-dk-setup/main-07-connect-device.png" alt-text="Conexión a Azure."::: 

1. Copie el código del dispositivo: haga clic en el vínculo **Copy** (Copiar) para copiar el código del dispositivo. A continuación, haga clic en **Login to Azure** (Iniciar sesión en Azure). 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-08-copy-code.png" alt-text="Copia del código del dispositivo."::: 

1. Pegue el código del dispositivo: se abrirá una nueva pestaña del explorador con una ventana que solicita el código del dispositivo copiado. Pegue el código en la ventana y haga clic en **Next** (Siguiente).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-09-enter-code.png" alt-text="Escritura del código del dispositivo."::: 

1. Inicie sesión en Azure: la siguiente ventana requiere que inicie sesión con la cuenta de Azure que ha comprobado al principio del inicio rápido. Escriba las credenciales de inicio de sesión y haga clic en **Next** (Siguiente). 

    > [!NOTE]
    > El explorador puede almacenar en caché automáticamente otras credenciales. Compruebe que está iniciando sesión con la cuenta correcta.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-10-azure-sign-in.png" alt-text="Inicio de sesión en Azure.":::
 
1. No cierre la pestaña del explorador de la experiencia de instalación en este paso: después de iniciar sesión, se le mostrará una pantalla en la que se confirma que ha iniciado sesión. Aunque le indica que puede cerrar la ventana, **se recomienda no cerrar ninguna de las ventanas**. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-11-sign-in-success.png" alt-text="Inicio de sesión correcto."::: 

1. Vuelva a la pestaña del explorador que hospeda la experiencia de instalación.
1. Seleccione su opción de IoT Hub.
    - Si ya tiene una instancia de IoT Hub y aparece en esta página, puede seleccionarla y **pasar al paso 17**.
    - Si no tiene ninguna o desea crear una nueva, vaya a la parte inferior de la lista y haga clic en **Create a new Azure IOT Hub** (Crear una nueva instancia de Azure IoT Hub).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-13-iot-hub-select.png" alt-text="Selección de una instancia de IoT Hub."::: 

1. Cree la nueva instancia de IoT Hub: rellene todos los campos de esta página. 
    - Seleccione la suscripción de Azure que va a usar con Azure Percept Studio.
    - Seleccione un grupo de recursos existente. Si no existe ninguno, haga clic en "Create new" (Crear nuevo) y siga las indicaciones. 
    - Seleccione la región de Azure. 
    - Asigne un nombre a la nueva instancia de IoT Hub.
    - Seleccione el plan de tarifa (normalmente, coincidirá con el de la suscripción).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-14-create-iot-hub.png" alt-text="Creación de una nueva instancia de IoT Hub."::: 

1. Espere a que se implemente la instancia de IoT Hub: esto puede tardar unos minutos.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-15-iot-hub-deploy.png" alt-text="Implementación de IoT Hub."::: 

1. Registre el kit de desarrollo: una vez completada la implementación, haga clic en el botón **Register** (Registrar).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-16-iot-hub-success.png" alt-text="Instancia de IoT Hub implementada correctamente."::: 

1. Asigne un nombre al kit de desarrollo: escriba un nombre de dispositivo para el kit de desarrollo y haga clic en **Next** (Siguiente).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-17-device-name.png" alt-text="Asignación de un nombre al dispositivo."::: 

1. Espere a que se descarguen los modelos de inteligencia artificial predeterminados; esto tardará unos minutos.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-18-finalize.png" alt-text="Finalización de la instalación."::: 

1. Vea Vision AI en acción: el kit de desarrollo se ha vinculado correctamente a Azure IoT Hub y ha recibido el modelo de detección de objetos de Vision AI predeterminado. La cámara de Azure Percept Vision ahora puede realizar la inferencia de detección de objetos sin una conexión a la nube. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-2-preview-video-output.png" alt-text="Hacer clic en la vista previa de la salida de vídeo."::: 
       
    > [!NOTE]
    > El proceso de incorporación y conexión al acceso Wi-Fi del dispositivo del equipo host se apaga en este punto, pero el kit de desarrollo permanecerá conectado a Internet.   Puede reiniciar la experiencia de incorporación con un reinicio del kit de desarrollo, lo que le permitirá volver a la incorporación y volver a conectar el dispositivo a un centro de conectividad de IoT diferente asociado a la misma suscripción de Azure o a otra.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-0-warning.png" alt-text="Advertencia de desconexión de la experiencia de instalación."::: 

1. Continúe en Azure Portal: vuelva a la ventana de la experiencia de instalación y haga clic en el botón **Continue to the Azure portal** (Continuar en Azure Portal) para empezar a crear los modelos de inteligencia artificial personalizados en Azure Percept Studio.

    > [!NOTE]
    > Compruebe que el equipo host ya no está conectado al punto de acceso del kit de desarrollo en la configuración de Wi-Fi y ahora se conecta de nuevo a la red Wi-Fi local.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-20-azure-portal-continue.png" alt-text="Ir a Azure Percept Studio."::: 

## <a name="view-your-device-in-the-azure-percept-studio-and-deploy-common-prebuilt-sample-apps"></a>Vea el dispositivo en Azure Percept Studio e implemente las aplicaciones de ejemplo habituales creadas previamente.

1. Consulte la lista de dispositivos en la página de información general de [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819). La página de información general de Azure Percept es el punto de inicio para acceder a muchos flujos de trabajo diferentes tanto para el modelado de inteligencia artificial perimetral inicial y avanzado, como para el desarrollo de soluciones.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-01-get-device-list.png" alt-text="Visualización de la lista de dispositivos.":::
    
1. Compruebe que el dispositivo que acaba de crear está conectado y haga clic en él.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-02-select-device.png" alt-text="Seleccione el dispositivo.":::

1. Vea el flujo de dispositivo para ver lo que está viendo la cámara del kit de desarrollo. Un modelo de detección de objetos predeterminado se implementa de forma inmediata y detecta una serie de objetos comunes.

    > [!NOTE]
    > La primera vez que lo haga en un nuevo dispositivo, recibirá una notificación de que se está implementando un nuevo módulo en la esquina superior derecha.  Una vez completada la implementación, podrá iniciar la ventana con la secuencia de vídeo de la cámara. 
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-1-start-video-stream.png" alt-text="Ver la secuencia de vídeo.":::
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-2-object-detection.png" alt-text="Ver la detección de objetos.":::

1. Explore los modelos de ejemplo de inteligencia artificial creados previamente.   Azure Percept Studio tiene una serie de ejemplos comunes pregenerados que se pueden implementar con un solo clic.   Seleccione "Deploy a sample model" (Implementar un modelo de ejemplo) para verlos e implementarlos.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-04-explore-prebuilt.png" alt-text="Exploración de modelos pregenerados.":::
    
1. Implemente una nuevo ejemplo pregenerado en el dispositivo conectado. Seleccione un ejemplo de la biblioteca y haga clic en "Deploy to Device" (Implementar en el dispositivo).

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-2-select-journey.png" alt-text="Ver detección de objetos en acción.":::

## <a name="next-steps"></a>Pasos siguientes

Puede seguir un flujo similar para probar [modelos de voz pregenerados](./tutorial-no-code-speech.md).