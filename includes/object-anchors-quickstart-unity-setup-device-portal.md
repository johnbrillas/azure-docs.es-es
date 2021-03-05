---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 44fd3b2bc89ac53e7a7c0293961098509d3f5c76
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "102044693"
---
### <a name="set-up-the-windows-device-portal"></a>Configuración del Portal de dispositivos Windows

Para conectarse al dispositivo HoloLens a través de Wi-Fi, siga estos pasos:

1. En primer lugar, [conecte el dispositivo HoloLens a la red Wi-Fi](https://docs.microsoft.com/hololens/hololens-network).

2. Busque ahora la dirección IP del dispositivo en **Configuración -> Red e Internet -> Wi-Fi -> Opciones avanzadas**.

3. Desde un explorador web de tu equipo, ve a `https://<YOUR_HOLOLENS_IP_ADDRESS>`. Se mostrará el siguiente mensaje en el explorador: "Hay un problema con el certificado de seguridad de este sitio web". Esto ocurre porque el certificado emitido en Device Portal es un certificado autofirmado. Puede ignorar este error de certificado y continuar.

Consulte [más información sobre cómo configurar el Portal de dispositivos Windows](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal).
