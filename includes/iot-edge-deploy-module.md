---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/30/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 6b9ec2017ffa5d4e4148b441aa23ed2eca6ee8b8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99628874"
---
Una de las funcionalidades clave de Azure IoT Edge es la implementación de código en dispositivos de IoT Edge desde la nube. Los *módulos de IoT Edge* son paquetes ejecutables que se implementan como contenedores. En esta sección, va a implementar un módulo pregenerado desde la [sección de módulos de IoT Edge de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) directamente desde la instancia de Azure IoT Hub.

El módulo que se implementa en esta sección simula un sensor y envía los datos generados. Este módulo es un fragmento de código útil para empezar a trabajar con IoT Edge porque se pueden usar los datos simulados para desarrollo y pruebas. Si desea ver exactamente lo que hace este módulo, puede ver el [código fuente del sensor de temperatura simulado](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs).

Siga estos pasos para implementar el primer módulo desde Azure Marketplace.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al centro de IoT.

1. En el menú de la izquierda, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Seleccione el identificador del dispositivo de destino en la lista de dispositivos.

1. En la barra superior, seleccione **Establecer módulos**.

   ![Captura de pantalla que muestra la selección de Establecer módulos.](./media/iot-edge-deploy-module/select-set-modules.png)

1. En **Módulos de IoT Edge**, abra el menú desplegable **Agregar** y, a continuación, seleccione **Módulo de Marketplace**.

   ![Captura de pantalla que muestra el menú desplegable Agregar.](./media/iot-edge-deploy-module/add-marketplace-module.png)

1. En **Marketplace de módulos IoT Edge**, busque y seleccione el módulo `Simulated Temperature Sensor`.

   El módulo se agrega a la sección Módulos de IoT Edge con el estado **en ejecución** deseado.

1. Seleccione **Siguiente: Rutas** para continuar con el paso siguiente del asistente.

   ![Captura de pantalla que muestra cómo continuar con el paso siguiente después de agregar el módulo.](./media/iot-edge-deploy-module/view-temperature-sensor-next-routes.png)

1. En la pestaña **Rutas**, quite la ruta predeterminada (**route**) y, a continuación, seleccione **Siguiente: Revisar y crear** para continuar al próximo paso del asistente.

   >[!Note]
   >Las rutas se construyen mediante pares de nombre-valor. Debería ver dos rutas en esta página. La ruta predeterminada (**route**) envía todos los mensajes a IoT Hub (que se denomina `$upstream`). Una segunda ruta (**SimulatedTemperatureSensorToIoTHub**) se creó automáticamente al agregar el módulo desde Azure Marketplace. Esta ruta envía todos los mensajes desde el módulo de temperatura simulado a IoT Hub. Puede eliminar la ruta predeterminada porque es redundante en este caso.

   ![Captura de pantalla que muestra cómo quitar la ruta predeterminada y pasar al paso siguiente.](./media/iot-edge-deploy-module/delete-route-next-review-create.png)

1. Revise el archivo JSON y luego seleccione **Crear**. El archivo JSON define todos los módulos que se implementan en el dispositivo IoT Edge. Verá el módulo **SimulatedTemperatureSensor** y los dos módulos en tiempo de ejecución, **edgeAgent** y **edgeHub**.

   >[!Note]
   >Cuando se envía una implementación nueva a un dispositivo IoT Edge, no se inserta nada en el dispositivo. En lugar de eso, el dispositivo consulta a IoT Hub de manera periódica para comprobar cualquier instrucción nueva. Si el dispositivo encuentra un manifiesto de implementación actualizado, usa la información sobre la nueva implementación para extraer las imágenes del módulo de la nube y, después, comienza a ejecutar localmente los módulos. Este proceso puede tardar unos minutos.

1. Después de crear los detalles de la implementación del módulo, el asistente lo lleva nuevamente a la página de detalles del dispositivo. Vea el estado de implementación en la pestaña **Módulos**.

   Deben aparecer tres módulos: **$edgeAgent**, **$edgeHub** y **SimulatedTemperatureSensor**. Si se muestra **SÍ** para uno o varios de los módulos en **ESPECIFICADO EN LA IMPLEMENTACIÓN** pero no en **NOTIFICADO POR EL DISPOSITIVO**, significa que el dispositivo IoT Edge todavía los está iniciando. Espere unos minutos y actualice la página.

   ![Captura de pantalla que muestra el sensor de temperatura simulado en la lista de módulos implementados.](./media/iot-edge-deploy-module/view-deployed-modules.png)
