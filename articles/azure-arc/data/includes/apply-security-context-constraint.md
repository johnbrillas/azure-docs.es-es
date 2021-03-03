---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 03/02/2021
ms.author: mikeray
ms.openlocfilehash: 0fca43f76b24a08ca96be749f7f2a822b0be2418
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101687601"
---
En esta sección se explica cómo aplicar una restricción de contexto de seguridad (SCC). En el caso de la versión preliminar, se relajan las restricciones de seguridad. 

1. Descargue la restricción de contexto de seguridad (SCC) personalizada. Use uno de los siguientes: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - [Sin formato](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml)
   - `curl`
   
       El siguiente comando descarga arc-data-scc.yaml:

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

   ```console
   oc create rolebinding arc-data-rbac --clusterrole=system:openshift:scc:arc-data-scc --serviceaccount=arc:default
   ```
