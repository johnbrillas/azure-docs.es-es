---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/07/2021
ms.author: alkohli
ms.openlocfilehash: c51577882e75facb1d8eb03c7cfab82467c5ec51
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99524967"
---
Para configurar el proceso en Azure Stack Edge Pro, creará un recurso de IoT Hub en Azure Portal.

1. En Azure Portal, en el recurso de Azure Stack Edge, vaya a **Información general** y seleccione **IoT Edge**.

   ![Introducción al proceso](./media/azure-stack-edge-gateway-configure-compute/configure-compute-1.png)

2. En **Enable IoT Edge service** (Habilitar servicio IoT Edge), seleccione **Add** (Agregar).

   ![Configurar el proceso](./media/azure-stack-edge-gateway-configure-compute/configure-compute-2.png)

3. En la hoja **Configure Edge compute** (Configurar el proceso de Edge), escriba la siguiente información:
   
    |Campo  |Value  |
    |---------|---------|
    |Suscripción     |Seleccione una suscripción para el recurso de IoT Hub. Puede emplear la misma suscripción que usa el recurso de Azure Stack Edge.         |
    |Resource group     |Seleccione un grupo de recursos para el recurso de IoT Hub. Puede emplear el mismo grupo de recursos que usa el recurso de Azure Stack Edge.         |
    |IoT Hub     | Elija entre **Nuevo** o **Existente**. <br> De forma predeterminada, se usa un nivel estándar (S1) para crear un recurso de IoT. Para usar un recurso de IoT de nivel gratuito, cree uno y, a continuación, seleccione el recurso existente. <br> En cualquier caso, el recurso de IoT Hub usa la misma suscripción y el mismo grupo de recursos que el recurso de Azure Stack Edge.     |
    |Nombre     |Acepte el nombre predeterminado o escriba un nombre para el recurso de IoT Hub.         |

   ![Introducción al proceso 2](./media/azure-stack-edge-gateway-configure-compute/configure-compute-3.png)

4. Cuando termine la configuración, seleccione **Review + Create** (Revisar y crear). Revise la configuración del recurso de IoT Hub y seleccione **Create** (Crear).

   La creación del recurso de IoT Hub tarda varios minutos. Una vez creado el recurso, la página **Información general** indica que el servicio IoT Edge ahora está en ejecución.

   ![Introducción al proceso 3](./media/azure-stack-edge-gateway-configure-compute/configure-compute-4.png)

5. Para confirmar que se ha configurado el rol de proceso de Edge, vaya a **IoT Edge > Propiedades**.

   ![Introducción al proceso 4](./media/azure-stack-edge-gateway-configure-compute/configure-compute-5.png)

   Cuando el rol de proceso de Edge está configurado en el dispositivo de Edge, este crea dos dispositivos: uno IoT y el otro IoT Edge. Ambos se pueden ver en el recurso de IoT Hub. Un entorno de ejecución de IoT Edge también se ejecuta en este dispositivo de IoT Edge. En este momento, solo está disponible la plataforma Linux para el dispositivo IoT Edge.

Puede tardar de 20 a 30 minutos en configurar el proceso, ya que en segundo plano se crean las máquinas virtuales y el clúster de Kubernetes.

Después de haber configurado correctamente el proceso en Azure Portal, existe un clúster de Kubernetes y un usuario predeterminado asociado al espacio de nombres de IoT (un espacio de nombres del sistema controlado por Azure Stack Edge Pro).
