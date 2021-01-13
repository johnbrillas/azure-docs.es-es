---
title: Azure Standard Load Balancer y Virtual Machine Scale Sets
titleSuffix: Azure Standard Load Balancer and Virtual Machine Scale Sets
description: Con esta ruta de aprendizaje, empiece a usar Azure Standard Load Balancer y Virtual Machine Scale Sets.
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: irenehua
ms.openlocfilehash: 7e1df754a4a4ca5878d93d53282fd39191313b54
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97883170"
---
# <a name="azure-load-balancer-with-azure-virtual-machine-scale-sets"></a>Azure Load Balancer con el conjunto de escalado de máquinas virtuales de Azure

Al trabajar con conjuntos de escalado de máquinas virtuales y el equilibrador de carga, debe tener en cuenta las pautas siguientes:

## <a name="port-forwarding-and-inbound-nat-rules"></a>Enrutamiento de puerto y reglas NAT de entrada:
  * Después de crear el conjunto de escalado, no se puede modificar el puerto de back-end para una regla de equilibrio de carga que se usa en un sondeo de estado del equilibrador de carga. Para cambiar el puerto, puede quitar el sondeo de estado mediante la actualización del conjunto de escalado de máquinas virtuales de Azure, actualizar el puerto y, a continuación, volver a configurar el sondeo de estado.
  * Al usar el conjunto de escalado de máquinas virtuales en el grupo de back-end del equilibrador de carga, las reglas NAT de entrada predeterminadas se crean automáticamente.
  
## <a name="inbound-nat-pool"></a>Grupo NAT de entrada:
  * Cada conjunto de escalado de máquinas virtuales debe tener al menos un grupo NAT de entrada. 
  * El grupo NAT de entrada es una colección de reglas NAT de entrada. Un grupo NAT de entrada no puede admitir varios conjuntos de escalado de máquinas virtuales.

## <a name="load-balancing-rules"></a>Reglas de equilibrio de carga:
  * Al usar el conjunto de escalado de máquinas virtuales en el grupo de back-end del equilibrador de carga, la regla de equilibrio de carga predeterminada se crea automáticamente.
  
## <a name="outbound-rules"></a>Reglas de salida:
  *  A fin de crear una regla de salida para un grupo de back-end al que ya se hace referencia mediante una regla de equilibrio de carga, primero debe marcar **"Crear reglas de salida implícitas"** como **No** en el portal cuando se crea la regla de equilibrio de carga de entrada.

  :::image type="content" source="./media/vm-scale-sets/load-balancer-and-vm-scale-sets.png" alt-text="Creación de una regla de equilibrio de carga" border="true":::

Los siguientes métodos se pueden usar para implementar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer.

* [Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante Azure Portal](./configure-vm-scale-set-portal.md)
* [Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante Azure PowerShell](./configure-vm-scale-set-powershell.md).
* [Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante la CLI de Azure](./configure-vm-scale-set-cli.md).
* [Actualización o eliminación de la instancia de Azure Load Balancer existente utilizada por el conjunto de escalado de máquinas virtuales](./update-load-balancer-with-vm-scale-set.md)
