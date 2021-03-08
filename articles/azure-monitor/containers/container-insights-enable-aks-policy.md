---
title: Habilitación del complemento de supervisión de AKS con Azure Policy
description: Describe cómo habilitar el complemento de supervisión de AKS mediante una directiva personalizada de Azure.
ms.topic: conceptual
ms.date: 02/04/2021
ms.openlocfilehash: 2163527cc83e70913e9a6e11bf2e22f9ed9c6690
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713905"
---
# <a name="enable-aks-monitoring-addon-using-azure-policy"></a>Habilitación del complemento de supervisión de AKS con Azure Policy
En esta artículo se describe cómo habilitar el complemento de supervisión de AKS mediante una directiva personalizada de Azure. La directiva personalizada del complemento de supervisión se puede asignar en el ámbito de la suscripción o del grupo de recursos. Si el área de trabajo de Azure Log Analytics y el clúster de AKS se encuentran en distintas suscripciones, la identidad administrada que use la asignación de directiva debe tener los permisos de rol necesarios en ambas suscripciones, o como mínimo en el recurso del área de trabajo de Log Analytics. Del mismo modo, si la directiva está en el ámbito del grupo de recursos, la identidad administrada debe tener los permisos de rol necesarios en el área de trabajo de Log Analytics si dicha área no se encuentra en el ámbito del grupo de recursos seleccionado.

El complemento de supervisión requiere los siguientes roles en la identidad administrada que usa Azure Policy:

 - [azure-kubernetes-service-contributor-role](../../role-based-access-control/built-in-roles.md#azure-kubernetes-service-contributor-role)
 - [log-analytics-contributor](../../role-based-access-control/built-in-roles.md#log-analytics-contributor)

## <a name="create-and-assign-policy-definition-using-azure-portal"></a>Creación y asignación de una definición de directiva mediante Azure Portal

### <a name="create-policy-definition"></a>Creación de definición de directiva

1. Descargue la definición de directiva personalizada de Azure para habilitar el complemento de supervisión de AKS.
 
    ``` sh
    curl -o azurepolicy.json -L https://aka.ms/aks-enable-monitoring-custom-policy
    ```

3. Vaya a https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions y cree la definición de directiva con los siguientes datos en el cuadro de diálogo para crear la definición de directiva.
 
    - **Ubicación de definición**: elija la suscripción a Azure en la que se debe almacenar la definición de directiva.
    - **Nombre**: *(Preview)AKS-Monitoring-Addon*
    - **Description** (Descripción): *Directiva personalizada de Azure para habilitar el complemento de supervisión en los clústeres de Azure Kubernetes en el ámbito especificado*
    - **Categoría**: elija *usar existentes* y seleccione *Kubernetes* de la lista desplegable.
    - **Regla de directiva**: quite las reglas de ejemplo existentes y copie el contenido del archivo *azurepolicy.json* que descargó en el paso n.º 1 anterior.

### <a name="assign-policy-definition-to-specified-scope"></a>Asignación de la definición de directiva al ámbito especificado

> [!NOTE]
>  La identidad administrada se creará automáticamente y se asignarán los roles especificados en la definición de la directiva.

1. Seleccione la definición de directiva *(Versión preliminar) complemento de supervisión de AKS* que acaba de crear.
4. Haga clic en *Asignar*** y especifique el **Ámbito** en el que se debe asignar la directiva. 
5. Haga clic en **Siguiente** y especifique el id. de recurso del área de trabajo de Azure Log Analytics. El id. de recurso debe tener este formato: `/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>`.
6. Cree una tarea de corrección en caso de que quiera aplicar la directiva a los clústeres de AKS existentes en el ámbito seleccionado.
7. Haga clic en la opción **Revisar y crear** para crear la asignación de directiva.
   
## <a name="create-and-assign-policy-definition-using-azure-cli"></a>Creación y asignación de una definición de directiva mediante la CLI de Azure

### <a name="create-policy-definition"></a>Creación de definición de directiva

1. Descargue los archivos de reglas y parámetros de la definición de directivas personalizadas de Azure con los siguientes comandos:

    ``` sh
    curl -o azurepolicy.rules.json -L https://aka.ms/aks-enable-monitoring-custom-policy-rules
    curl -o azurepolicy.parameters.json -L https://aka.ms/aks-enable-monitoring-custom-policy-parameters
    ```

2. Cree la definición de directiva con el comando siguiente:

    ``` sh
    az cloud set -n <AzureCloud | AzureChinaCloud | AzureUSGovernment> # set the Azure cloud
    az login # login to cloud environment 
    az account set -s <subscriptionId>
    az policy definition create --name "(Preview)AKS-Monitoring-Addon" --display-name "(Preview)AKS-Monitoring-Addon" --mode Indexed --metadata version=1.0.0 category=Kubernetes --rules azurepolicy.rules.json --params azurepolicy.parameters.json
    ```

### <a name="assign-policy-definition-to-specified-scope"></a>Asignación de la definición de directiva al ámbito especificado

- Cree la asignación de directiva con el comando siguiente:

    ``` sh
    az policy assignment create --name aks-monitoring-addon --policy "(Preview)AKS-Monitoring-Addon" --assign-identity --identity-scope /subscriptions/<subscriptionId> --role Contributor --scope /subscriptions/<subscriptionId> --location <locatio> --role Contributor --scope /subscriptions/<subscriptionId> -p "{ \"workspaceResourceId\": { \"value\":  \"/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/microsoft.operationalinsights/workspaces/<workspaceName>\" } }"
    ```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información acerca de [Azure Policy](../../governance/policy/overview.md).
- Obtenga más información acerca de cómo [funciona la seguridad de corrección](../../governance/policy/how-to/remediate-resources.md#how-remediation-security-works).
- Obtenga más información sobre [Container Insights](./container-insights-overview.md).
- Instale la [CLI de Azure](/cli/azure/install-azure-cli).