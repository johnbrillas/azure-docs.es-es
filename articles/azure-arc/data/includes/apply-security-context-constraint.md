---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 01/15/2021
ms.author: mikeray
ms.openlocfilehash: 6c8dbeea83cba306cfb788cf447236088045ffc9
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99494026"
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

   > [!NOTE]
   > RedHat OpenShift 4.5 o superior, cambia el modo de aplicar el SCC a la cuenta de servicio.
   > Use el mismo espacio de nombres aquí y en el comando `azdata arc dc create` siguiente. Un ejemplo es `arc`. 
   > 
   > Si usa RedHat OpenShift 4.5 o una versión posterior, ejecute: 
   >
   >```console
   >oc create rolebinding arc-data-rbac --clusterrole=system:openshift:scc:arc-data-scc --serviceaccount=arc:default
   >```
