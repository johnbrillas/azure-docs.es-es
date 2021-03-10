---
title: Uso de un módulo de IoT Edge para implementar la carga de trabajo de proceso en Azure Stack Edge Pro con GPU | Microsoft Docs
description: Aprenda a ejecutar una carga de trabajo de proceso mediante un módulo de IoT Edge creado previamente en el dispositivo de GPU de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 02/22/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge Pro so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 6c067fb5f39e82bb1601ce7b4d9dc5e2ce4ac624
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102440146"
---
# <a name="tutorial-run-a-compute-workload-with-iot-edge-module-on-azure-stack-edge-pro-gpu"></a>Tutorial: Ejecución de una carga de trabajo de proceso con el módulo de IoT Edge en un dispositivo GPU de Azure Stack Pro

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

En este tutorial se describe cómo ejecutar una carga de trabajo de proceso mediante un módulo de IoT Edge en el dispositivo de GPU de Azure Stack Edge Pro. Una vez configurado el rol de proceso, el dispositivo transformará los datos antes de enviarlos a Azure.

Este procedimiento tarda aproximadamente entre 10 y 15 minutos en completarse.


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar el proceso
> * Agregar recursos compartidos
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

 
## <a name="prerequisites"></a>Requisitos previos

Antes de configurar un rol de proceso en el dispositivo de GPU de Azure Stack Edge Pro, asegúrese de que:

- Ha activado un dispositivo Azure Stack Edge Pro como se describe en [Activación de Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-activate.md).
- Tiene un módulo de IoT Edge que puede ejecutar en los datos. En este tutorial, se usa un módulo `filemove2` que mueve los datos del recurso compartido local de Edge del dispositivo a un recurso compartido de Edge desde donde los datos van a una cuenta de Azure Storage.


## <a name="configure-compute"></a>Configurar el proceso

[!INCLUDE [configure-compute](../../includes/azure-stack-edge-gateway-configure-compute.md)]


## <a name="add-shares"></a>Agregar recursos compartidos

En este tutorial, se realizará una implementación sencilla con dos recursos compartidos: un recurso compartido perimetral y otro recurso compartido local perimetral.

1. Para agregar un recurso compartido de Edge al dispositivo, siga estos pasos:

    1. En el recurso de Azure Stack Edge, vaya a **Puerta de enlace de almacenamiento en la nube > Recursos compartidos**.
    2. En la barra de comandos, seleccione **+ Agregar recurso compartido**.
    3. En la hoja **Agregar recurso compartido**, proporcione el nombre del recurso compartido y seleccione el tipo de recurso compartido.
    4. Para montar el recurso compartido perimetral, active la casilla **Usar el recurso compartido con el proceso perimetral**.
    5. Seleccione la **cuenta de almacenamiento**, el **servicio de almacenamiento**, un usuario existente y, luego, **Crear**.

        ![Agregar un recurso compartido perimetral](./media/azure-stack-edge-gpu-deploy-compute-module-simple/add-edge-share-1.png) 


    > [!NOTE]
    > Para montar el recurso compartido de NFS para un proceso, la red de proceso debe configurarse en la misma subred que la dirección IP virtual de NFS. Para obtener más información sobre cómo configurar la red de proceso, vaya a [Habilitación de la red de proceso en Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).

    Se crea el recurso compartido perimetral y recibirá una notificación para indicar que la creación se realizó correctamente. Puede que la lista de recursos compartidos se actualice, pero debe esperar a que la creación del recurso compartido se complete.

2. Para agregar un recurso compartido local de Edge al dispositivo, repita todas las indicaciones del paso anterior y active la casilla **Configurar como recurso compartido local de Edge**. Los datos en el recurso compartido local permanecen en el dispositivo.

    ![Agregar un recurso compartido local perimetral](./media/azure-stack-edge-gpu-deploy-compute-module-simple/add-edge-share-2.png)

    Si ha creado un recurso compartido NFS local, use la siguiente opción de comando de sincronización remota (rsync) para copiar archivos en él:

    `rsync <source file path> < destination file path>`

    Para más información sobre el comando `rsync`, consulte la [documentación sobre `Rsync`](https://www.computerhope.com/unix/rsync.htm).
 
3. Vaya a **Puerta de enlace de almacenamiento en la nube > Recursos Compartidos** para ver la lista actualizada de recursos compartidos.

    ![Lista de recursos compartidos actualizada](./media/azure-stack-edge-gpu-deploy-compute-module-simple/add-edge-share-3.png) 
 

## <a name="add-a-module"></a>Agregar un módulo

Puede agregar un módulo personalizado o uno creado previamente. El dispositivo no incluye módulos predefinidos o personalizados. Para aprender a crear un módulo personalizado, vaya a [Desarrollo de un módulo C# para un dispositivo de Azure Stack Edge Pro](azure-stack-edge-j-series-create-iot-edge-module.md).

En esta sección, agregará un módulo personalizado al dispositivo de IoT Edge que creó en [Desarrollo de un módulo C# para Azure Stack Edge Pro](azure-stack-edge-j-series-create-iot-edge-module.md). Este módulo personalizado toma los archivos de un recurso compartido local perimetral del dispositivo perimetral y los mueve a un recurso compartido perimetral (nube) del dispositivo. A continuación, el recurso compartido en la nube inserta los archivos en la cuenta de Azure Storage asociada con este.

Para agregar un módulo, haga lo siguiente:

1. Vaya a **IoT Edge > Módulos**. En la barra de comandos, seleccione **+ Agregar módulo**. 

2. En la hoja **Agregar módulo**, introduzca los siguientes valores:

    
    |Campo  |Valor  |
    |---------|---------|
    |Nombre     | Un nombre único para el módulo. Este módulo es un contenedor de Docker que puede implementar en el dispositivo de IoT Edge asociado a Azure Stack Edge Pro.        |
    |URI de imagen     | El URI de la imagen de contenedor correspondiente del módulo.        |
    |Credenciales necesarias     | Si está activada, se usa el nombre de usuario y la contraseña para recuperar los módulos con una dirección URL coincidente.        |
    |Recurso compartido de entrada     | Seleccione un recurso compartido de entrada. En este caso, el recurso compartido local perimetral es el recurso compartido de entrada. El módulo que se usa aquí mueve los archivos desde el recurso compartido local perimetral hasta un recurso compartido perimetral donde se cargan en la nube.        |
    |Recurso compartido de salida     | Seleccione un recurso compartido de salida. En este caso, el recurso compartido perimetral es el recurso compartido de salida.        |
    |Tipo de desencadenador     | Seleccione **Archivo** o **Programación**. Un desencadenador de archivo se activa cada vez que se produce un evento de archivo, por ejemplo, cuando se escribe un archivo en el recurso compartido de entrada. Un desencadenador programado se activa según una programación definida por el usuario.         |
    |Nombre del desencadenador     | Un nombre exclusivo para el concentrador.         |
    |Variables de entorno| Información opcional que le ayudará a definir el entorno en que se ejecutará su módulo.   |

    ![Agregar y configurar el módulo](./media/azure-stack-edge-gpu-deploy-compute-module-simple/add-module-1.png)

3. Seleccione **Agregar**. Se agrega el módulo. La página **IoT Edge > Módulos** se actualiza para indicar que el módulo se ha implementado. El estado del entorno de ejecución del módulo que se agrega es *En ejecución*.

    ![Módulo implementado](./media/azure-stack-edge-gpu-deploy-compute-module-simple/add-module-2.png)

### <a name="verify-data-transform-and-transfer"></a>Comprobar la transformación y la transferencia de los datos

El último paso es asegurarse de que el módulo se esté ejecutando y que procese los datos según lo previsto. El estado del entorno de ejecución del módulo debe ser "en ejecución" para el dispositivo IoT Edge del recurso de IoT Hub.

Para comprobar que el módulo se está ejecutando y que procesa los datos según lo previsto, haga lo siguiente:


1. En el Explorador de archivos, conéctese a los recursos compartidos local perimetral y perimetral que creó anteriormente. Consultar los pasos 

    ![Conexión con los recursos compartidos locales y en la nube de Edge](./media/azure-stack-edge-gpu-deploy-compute-module-simple/verify-data-1.png) 
 
1. Agregue datos al recurso compartido local.

    ![Archivo copiado en el recurso compartido local de Edge](./media/azure-stack-edge-gpu-deploy-compute-module-simple/verify-data-2.png) 
 
   Los datos se mueven al recurso compartido en la nube.

    ![Archivo trasladado al recurso compartido en la nube de Edge](./media/azure-stack-edge-gpu-deploy-compute-module-simple/verify-data-3.png)  

   A continuación, los datos se trasladan del recurso compartido en la nube a la cuenta de almacenamiento. Para ver los datos puede usar el Explorador de Storage o Azure Storage en el portal.

    ![Comprobación de los datos de la cuenta de almacenamiento](./media/azure-stack-edge-gpu-deploy-compute-module-simple/verify-data-4.png)
 
Ha completado el proceso de validación.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Configurar el proceso
> * Agregar recursos compartidos
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

Para aprender a administrar el dispositivo de Azure Stack Edge Pro, consulte:

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar Azure Stack Edge Pro](azure-stack-edge-gpu-manage-access-power-connectivity-mode.md).
