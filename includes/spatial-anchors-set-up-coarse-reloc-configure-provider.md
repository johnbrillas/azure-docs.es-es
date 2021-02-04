---
author: msftradford
ms.author: parkerra
ms.date: 01/28/2021
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 386c2f8a7cc32cf65381d9d62e2e6940754e3606
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99215109"
---
## <a name="configure-the-sensor-fingerprint-provider"></a>Configuración del proveedor de huellas digitales del sensor

Para empezar, es preciso crear y configurar un proveedor de huellas digitales del sensor. Este proveedor de huella digital del sensor se encargará de leer los sensores específicos de la plataforma en el dispositivo y de convertir sus lecturas en una representación común, que es la que usa la sesión del anclaje espacial en la nube.

> [!IMPORTANT]
> [Asegúrese de comprobar aquí](../articles/spatial-anchors/concepts/coarse-reloc.md#platform-availability) si los sensores que está habilitando están disponibles en la plataforma.