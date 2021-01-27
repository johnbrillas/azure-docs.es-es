---
title: Administración de IoT Hub Device Provisioning Service mediante la CLI de Azure y la extensión de IoT
description: Aprenda a usar la CLI de Azure y la extensión de IoT para administrar IoT Hub Device Provisioning Service (DPS)
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
ms.custom: devx-track-azurecli
services: iot-dps
ms.openlocfilehash: dd0564fbb23a0695d849852fd464308cd1b5fac9
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678969"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>Uso de la CLI de Azure y de la extensión de IoT para administrar IoT Hub Device Provisioning Service

La [CLI de Azure](/cli/azure) es una herramienta de la línea de comandos multiplataforma y de código abierto para la administración de recursos de Azure, como IoT Edge. La CLI de Azure está disponible en Windows, Linux y macOS. La CLI de Azure permite administrar recursos de Azure IoT Hub, instancias de servicio de aprovisionamiento de dispositivos y centros vinculados listos para usar.

La extensión de IoT enriquece la CLI de Azure con características como administración de dispositivos y funcionalidad completa de IoT Edge.

En este tutorial, primero realizará los pasos para configurar la CLI de Azure y la extensión de IoT. A continuación, aprenderá a ejecutar comandos de la CLI para realizar operaciones básicas del servicio de aprovisionamiento de dispositivos. 

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Requisitos previos

- Se requiere [Python 2.7x o Python 3.x](https://www.python.org/downloads/).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0.70 de la CLI de Azure, o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="basic-device-provisioning-service-operations"></a>Operaciones básicas del servicio de aprovisionamiento de dispositivos

En el ejemplo se muestra cómo iniciar sesión en una cuenta de Azure, crear un grupo de recursos de Azure (un contenedor que alberga los recurso relacionados de una solución de Azure), crear una instancia de IoT Hub, crear un servicio de aprovisionamiento de dispositivos, enumerar los servicios de aprovisionamiento de dispositivos existentes y crear un centro de IoT vinculado con comandos de la CLI. 

Antes de comenzar, realice los pasos de instalación descritos anteriormente. Si no tiene una cuenta de Azure aún, también puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Inicie sesión en la cuenta de Azure
  
```azurecli
az login
```

![Captura de pantalla que muestra una ventana del símbolo del sistema que ejecuta el comando az login.](./media/how-to-manage-dps-with-cli/login.jpg)

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Cree un grupo de recursos IoTHubBlogDemo en eastus

```azurecli
az group create -l eastus -n IoTHubBlogDemo
```

![Creación de un grupo de recursos](./media/how-to-manage-dps-with-cli/create-resource-group.jpg)


### <a name="3-create-two-device-provisioning-services"></a>3. Creación de dos servicios de aprovisionamiento de dispositivos

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps
```

![Creación de un servicio de aprovisionamiento de dispositivos](./media/how-to-manage-dps-with-cli/create-dps.jpg)

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps2
```

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Enumeración de todos los servicios de aprovisionamiento de dispositivos existentes en este grupo de recursos

```azurecli
az iot dps list --resource-group IoTHubBlogDemo
```

![Lista de los servicios de aprovisionamiento de dispositivos](./media/how-to-manage-dps-with-cli/list-dps.jpg)


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Cree una instancia de IoT Hub blogDemoHub en el grupo de recursos recién creado

```azurecli
az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo
```

![Crear IoT Hub](./media/how-to-manage-dps-with-cli/create-hub.jpg)

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Vinculación de una instancia de IoT Hub existente con un servicio de aprovisionamiento de dispositivos

```azurecli
az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus
```

![Vincular centro](./media/how-to-manage-dps-with-cli/create-hub.jpg)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Inscribir el dispositivo
> * Iniciar el dispositivo
> * Comprobar que el dispositivo está registrado

Pase al siguiente tutorial para aprender a aprovisionar varios dispositivos en centros con equilibrio de carga. 

> [!div class="nextstepaction"]
> [Aprovisionamiento de dispositivos en instancias de IoT Hub con equilibrio de carga](./tutorial-provision-multiple-hubs.md)