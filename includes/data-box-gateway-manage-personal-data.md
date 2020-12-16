---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: ebd0e48c9f197439e838efbc936537ca5da3520f
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581388"
---
- **Detalles de pedido**. Una vez creado el pedido, la dirección de envío, la dirección de correo electrónico y la información de contacto del usuario se almacenan en Azure Portal. Entre la información guardada se incluyen los siguientes datos:
  - Nombre de contacto
  - Número de teléfono
  - Dirección de correo electrónico
  - Dirección
  - City
  - Código postal
  - State
  - País/provincia/región
  - Número de seguimiento del envío

    Los detalles del pedido se cifran y almacenan en el servicio. El servicio conserva la información hasta que elimine explícitamente el recurso o el pedido. La eliminación del recurso y el pedido correspondiente se bloquea desde el momento en que se envía el dispositivo hasta que el dispositivo vuelve a Microsoft.

- **Dirección de envío**. Después de realizar el pedido, el servicio Data Box proporciona la dirección de envío a compañías de transporte como UPS.

- **Usuarios de los recursos compartidos**. Los usuarios en el dispositivo también pueden acceder a los datos que se encuentran en los recursos compartidos. Se puede ver una lista de los usuarios que pueden acceder a los datos del recurso compartido. Cuando los recursos compartidos se eliminan, esta lista también se elimina.