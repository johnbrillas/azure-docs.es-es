---
title: Ejecución de un módulo de GPU en un dispositivo de Microsoft Azure Stack Edge Pro con GPU | Microsoft Docs
description: Describe cómo configurar y ejecutar un módulo en GPU en un dispositivo Azure Stack Edge Pro mediante Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: dfae1a9b02db7e7b9577acdb47a1ba089f1609e8
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102439058"
---
# <a name="configure-and-run-a-module-on-gpu-on-azure-stack-edge-pro-device"></a>Configuración y ejecución de un módulo en GPU en un dispositivo Azure Stack Edge Pro

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

El dispositivo Azure Stack Edge Pro contiene una o varias unidades de procesamiento gráfico (GPU). Las GPU son una opción popular para los cálculos de IA, ya que ofrecen funcionalidades de procesamiento en paralelo y son más rápidas en la representación de imágenes que las unidades de procesamiento central (CPU). Para más información sobre la GPU contenida en un dispositivo Azure Stack Edge Pro, vaya a [Especificaciones técnicas de dispositivos de Azure Stack Edge Pro](azure-stack-edge-gpu-technical-specifications-compliance.md).

En este artículo se describe cómo configurar y ejecutar un módulo en la GPU en un dispositivo Azure Stack Edge Pro. Aquí usará un módulo de contenedor **Digits** disponible públicamente, escrito para GPU de Nvidia T4. Este procedimiento se puede usar para configurar cualquier otro módulo publicado por Nvidia para estas GPU.


## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Tiene acceso a un dispositivo Azure Stack Edge Pro con un nodo habilitado para GPU. Este dispositivo se activa con un recurso en Azure.  

## <a name="configure-module-to-use-gpu"></a>Configuración del módulo para usar la GPU

Para configurar un módulo para que use la GPU en un dispositivo Azure Stack Edge Pro para ejecutar un módulo,<!--Can it be simplified? "To configure a module to be run by the GPU on your Azure Stack Edge Pro device,"?--> siga estos pasos.

1. En Azure Portal, vaya al recurso asociado con el dispositivo.

2. En **Información general**, seleccione **IoT Edge**.

    ![Configuración del módulo para usar la GPU 1](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-1.png)

3. En **Habilitar servicio IoT Edge**, seleccione **Agregar**.

   ![Configuración del módulo para usar la GPU 2](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-2.png)

4. En **Crear servicio IoT Edge**, escriba la configuración del recurso de IoT Hub:

   |Campo   |Valor    |
   |--------|---------|
   |Subscription      | Suscripción utilizada por el recurso de Azure Stack Edge. |
   |Grupo de recursos    | Grupo de recursos utilizado por el recurso de Azure Stack Edge. |
   |IoT Hub           | Elija entre **Crear nuevo** o **Usar existente**. <br> De forma predeterminada, se usa un nivel estándar (S1) para crear un recurso de IoT. Para usar un recurso de IoT de nivel gratuito, cree uno y, a continuación, seleccione el recurso existente. <br> En cualquier caso, el recurso de IoT Hub usa la misma suscripción y el mismo grupo de recursos que el recurso de Azure Stack Edge.     |
   |Nombre              | Si no desea usar el nombre predeterminado proporcionado para el nuevo recurso de IoT Hub, escriba un nombre diferente. |

   Cuando termine la configuración, seleccione **Revisar y crear**. Revise la configuración del recurso de IoT Hub y seleccione **Crear**.

   ![Introducción al proceso 2](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-3.png)

   La creación del recurso de IoT Hub tarda varios minutos. Una vez creado el recurso, la página **Información general** indica que el servicio IoT Edge ahora está en ejecución.

   ![Introducción al proceso 3](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-4.png)

5. Para confirmar que se ha configurado el rol de proceso de Edge, seleccione **Propiedades**.

   ![Introducción al proceso 4](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-5.png)

6. En **Propiedades**, seleccione el vínculo para **dispositivo IoT Edge**.

   ![Configuración del módulo para usar la GPU 6](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-2.png)

   En el panel derecho, se ve el dispositivo IoT Edge asociado al dispositivo Azure Stack Edge Pro. Esto corresponde al dispositivo IoT Edge que creó al crear el recurso de IoT Hub.
 
7. Seleccione este dispositivo de IoT Edge.

   ![Configuración del módulo para usar la GPU 7](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-3.png)

8. Seleccione **Set modules** (Establecer módulos).

   ![Configuración del módulo para usar la GPU 8](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-4.png)

9. En **+ Agregar**, seleccione **+ Módulo de IoT Edge**. 

    ![Configuración del módulo para usar la GPU 9](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-5.png)

10. En la pestaña **Agregar módulo IoT Edge**.

    1. Proporcione el **URI de la imagen**. Aquí usará el módulo **Digits** de Nvidia disponible públicamente. 
    
    2. Establezca **Directiva de reinicio** en **siempre**.
    
    3. Establezca **Estado deseado** en **en ejecución**.
    
    ![Configuración del módulo para usar la GPU 10](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-6.png)

11. En la pestaña **Variables de entorno**, indique el nombre de la variable y el valor correspondiente. 

    1. Para que el módulo actual use una GPU en este dispositivo, use NVIDIA_VISIBLE_DEVICES. 

    2. Define el valor como 0 o 1. Un valor de 0 o 1 garantiza que el dispositivo use al menos una GPU para este módulo. Establecer el valor en 0 o 1 implica que este módulo está usando ambas GPU en el dispositivo.

       ![Configuración del módulo para usar la GPU 11](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-7.png)

       Para obtener más información sobre las variables de entorno que se pueden usar con la GPU de Nvidia, vaya a [nVidia container runtime](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).

    > [!NOTE]
    > Una GPU solo se puede asignar a un módulo. Sin embargo, un módulo puede usar una, ambas o ninguna GPU.

12. Escriba un nombre para el módulo. En este momento, puede elegir proporcionar la opción de creación del contenedor y modificar la configuración del módulo gemelo, o bien, seleccionar **Agregar**. 

    ![Configuración del módulo para usar la GPU 12](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-8.png)

13. Asegúrese de que el módulo esté en ejecución y seleccione **Revisar y crear**.

    ![Configuración del módulo para usar la GPU 13](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-9.png)

14. En la pestaña **Revisar y crear**, se muestran las opciones de implementación que ha seleccionado. Revise las opciones y seleccione **Crear**.
    
    ![Configuración del módulo para usar la GPU 14](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-10.png)

15. Anote el **estado de runtime** del módulo.
    
    ![Configuración del módulo para usar la GPU 15](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-11.png)

    El módulo tarda un par de minutos en implementarse. Seleccione **Actualizar** y debería ver que el **estado de runtime** se actualiza a **en ejecución**.

    ![Configuración del módulo para usar la GPU 16](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-12.png)


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [Variables de entorno de que puede usar con la GPU de Nvidia](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).
