---
title: Creación de una nube privada de Azure VMware Solution
description: Pasos para crear una nube privada de Azure VMware Solution mediante Azure Portal.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: da79881e609f982960468a8f26c98178f972ad43
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101725411"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Crear un nuevo recurso**. En el cuadro de texto **Buscar en el Marketplace**, escriba `Azure VMware Solution` y seleccione **Azure VMware Solution** en la lista. En la ventana **Azure VMware Solution**, seleccione **Crear**.

1. En la pestaña **Básica**, escriba valores en los campos. En la tabla siguiente se enumeran las propiedades de los campos.

   | Campo   | Value  |
   | ---| --- |
   | **Suscripción** | La suscripción que va a usar para la implementación.|
   | **Grupos de recursos** | El grupo de recursos para los recursos de nube privada. |
   | **Ubicación** | Seleccione una ubicación, como **Este de EE. UU.**|
   | **Nombre del recurso** | Nombre de la nube privada de Azure VMware Solution. |
   | **SKU** | Seleccione el valor de SKU siguiente: AV36 |
   | **Hosts** | Número de hosts que se van a agregar al clúster de nube privada. El valor predeterminado es 3, que se puede aumentar o reducir después de la implementación.  |
   | **Bloque de direcciones** | Escriba un bloque de direcciones IP para la red CIDR para la nube privada; por ejemplo, 10.175.0.0/22. |
   | **Virtual Network** | Seleccione una red virtual o cree una nueva para la nube privada de Azure VMware Solution.  |

   :::image type="content" source="../media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="En la pestaña Básico, escriba los valores de los campos." border="true":::

1. Cuando termine, seleccione **Revisar y crear**. En la siguiente pantalla, compruebe la información especificada. Si toda la información es correcta, seleccione **Crear**.

   > [!NOTE]
   > Este paso tarda aproximadamente dos horas. 

1. Comprobación de que la implementación se ha realizado correctamente. Navegue hasta el grupo de recursos que creó y seleccione la nube privada.  Una vez que se haya completado la implementación, se mostrará el estado **Correcto**. 

   :::image type="content" source="../media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Comprobación de que la implementación se ha realizado correctamente." border="true":::