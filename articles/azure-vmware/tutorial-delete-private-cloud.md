---
title: 'Tutorial: Eliminación de una nube privada de Azure VMware Solution'
description: Aprenda a eliminar una nube privada de Azure VMware Solution que ya no se necesita.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 453e7a3316c342cd724a951eafea0ae9fa045506
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103462106"
---
# <a name="tutorial-delete-an-azure-vmware-solution-private-cloud"></a>Tutorial: Eliminación de una nube privada de Azure VMware Solution

Si tiene una nube privada de Azure VMware Solution que ya no necesita, puede eliminarla. Una nube privada incluye un dominio de red aislado, uno o varios clústeres de vSphere aprovisionados en hosts de servidor dedicados y varias máquinas virtuales. Cuando se elimina una nube privada, se eliminan todas las máquinas virtuales, sus datos y los clústeres. Los hosts de Azure VMware Solution dedicados se borran de forma segura y se devuelven al grupo libre. También se elimina el espacio de direcciones de red aprovisionado.  

> [!CAUTION]
> La eliminación de la nube privada es una operación irreversible. Una vez eliminada la nube privada, no se pueden recuperar los datos, ya que se finalizan todas las cargas de trabajo en ejecución, se eliminan los componentes y se destruyen todos los datos y valores de configuración de la nube privada, incluidas las direcciones IP públicas.

## <a name="prerequisites"></a>Requisitos previos

Si necesita las máquinas virtuales y sus datos más adelante, asegúrese de hacer una copia de seguridad de los datos antes de eliminar la nube privada.  No hay ninguna manera de recuperar las máquinas virtuales y sus datos.


## <a name="delete-the-private-cloud"></a>Eliminación de la nube privada

1. Acceda a la consola de Azure VMware Solution en [Azure Portal](https://portal.azure.com).

2. Seleccione la nube privada que se va a eliminar.
 
3. Escriba el nombre de la nube privada y seleccione **Sí**. 

>[!NOTE]
>El proceso de eliminación tarda unas horas en completarse.  
