---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/12/2021
ms.author: alkohli
ms.openlocfilehash: 0c7e011cf8445164e0931f71e390813c9134dd89
ms.sourcegitcommit: 5f32f03eeb892bf0d023b23bd709e642d1812696
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2021
ms.locfileid: "103200956"
---
1. En [Azure Portal](https://portal.azure.com/), seleccione el recurso de Azure Stack Edge y, después, vaya a **Información general**. El dispositivo debe estar en línea. Vaya a **Cloud storage gateway > Storage accounts** (Puerta de enlace de almacenamiento en la nube > Cuentas de almacenamiento).

2. Seleccione **+ Agregar cuenta de almacenamiento** en la barra de comandos del dispositivo. 

   ![Agregar una cuenta de almacenamiento](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. En el panel **Agregar cuenta de almacenamiento de Edge**, especifique la siguiente configuración:

    1. Especifique un nombre único para la cuenta de almacenamiento de Edge en el dispositivo. Los nombres de las cuentas de almacenamiento solo pueden contener números y letras en minúsculas. No se aceptan caracteres especiales. El nombre de la cuenta de almacenamiento debe ser único en el dispositivo (no en todos los dispositivos).

    2. Proporcione una descripción opcional de la información sobre los datos que contiene la cuenta de almacenamiento.  
    
    3. De manera predeterminada, la cuenta de almacenamiento de Edge está asignada a una cuenta de Azure Storage en la nube y los datos de la cuenta de almacenamiento se insertan automáticamente en la nube. Especifique la cuenta de Azure Storage a la que está asignada la cuenta de almacenamiento de Edge.

    4. Cree un nuevo contenedor o seleccione un contenedor existente en la cuenta de almacenamiento de Azure. Los datos del dispositivo que se escriben en la cuenta de almacenamiento de Edge se cargan automáticamente en el contenedor de almacenamiento seleccionado en la cuenta de Azure Storage asignada.

    5. Después de especificar todas las opciones de cuenta de almacenamiento, seleccione **Agregar** para crear la cuenta de almacenamiento de Edge. Recibirá una notificación cuando la cuenta de almacenamiento de Edge se haya creado correctamente. La nueva cuenta de almacenamiento de Edge se muestra en la lista de cuentas de almacenamiento en Azure Portal.

    <!--[Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->
    
4. Si selecciona esta nueva cuenta de almacenamiento y va a **Claves de acceso**, puede encontrar el punto de conexión de Blob Service y el nombre de la cuenta de almacenamiento correspondiente. Copie esta información, ya que estos valores junto con las claves de acceso le ayudarán a conectarse a la cuenta de almacenamiento de Edge.

    ![Adición de una cuenta de almacenamiento 2](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    Puede obtener las claves de acceso mediante la [conexión a las API locales del dispositivo con Azure Resource Manager](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md). 
