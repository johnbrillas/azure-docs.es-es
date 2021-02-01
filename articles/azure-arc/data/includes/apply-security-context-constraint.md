---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 01/15/2021
ms.author: mikeray
ms.openlocfilehash: 17a8c9580a8e69213c7f34e8ec889f7e46c6d17d
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696078"
---
En esta sección se explica cómo aplicar una restricción de contexto de seguridad (SCC). En el caso de la versión preliminar, se relajan las restricciones de seguridad. 

1. Descargue la restricción de contexto de seguridad (SCC) personalizada. Use uno de los siguientes: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - ([Raw](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml))
   - `curl` El siguiente comando descarga arc-data-scc.yaml:

      ```console
      curl https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml -o arc-data-scc.yaml
      ```

1. Cree la SCC.

   ```console
   oc create -f arc-data-scc.yaml
   ```

1. Aplique la SCC a la cuenta de servicio.

   > [!NOTE]
   > Use el mismo espacio de nombres aquí y en el comando `azdata arc dc create` siguiente. Un ejemplo es `arc`.

   ```console
   oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
   ```