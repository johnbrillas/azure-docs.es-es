---
title: Administración de recursos de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure
description: Obtenga información sobre los comandos comunes para automatizar la administración de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure.
author: TheovanKraay
ms.service: managed-instance-apache-cassandra
ms.topic: how-to
ms.date: 03/02/2021
ms.author: thvankra
ms.openlocfilehash: 3cd5fdbf6cdc504a1290c8fbd80cf89cf85ce714
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743858"
---
# <a name="manage-azure-managed-instance-for-apache-cassandra-resources-using-azure-cli-preview"></a>Administración de recursos de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure (versión preliminar)

En este artículo se describen los comandos comunes para automatizar la administración de Azure Managed Instance para clústeres de Apache Cassandra mediante la CLI de Azure.

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* Este artículo requiere la CLI de Azure 2.12.1 o una versión posterior. Si usa Azure Cloud Shell, la versión más reciente ya está instalada.

> [!IMPORTANT]
> La administración de Azure Managed Instance para recursos de Apache Cassandra no se puede cambiar de nombre, ya que esto infringe el funcionamiento de Azure Resource Manager con los URI de recursos.

## <a name="azure-managed-instance-for-apache-cassandra-clusters"></a>Azure Managed Instance para clústeres de Apache Cassandra

En las secciones siguientes se muestra cómo administrar clústeres de Azure Managed Instance for Apache Cassandra:

* [Creación de un clúster de instancia administrada](#create-cluster)
* [Eliminación de un clúster de instancia administrada](#delete-cluster)
* [Obtención de los detalles del clúster](#get-cluster-details)
* [Obtención del estado del nodo de clúster](#get-cluster-status)
* [Listado de clústeres por grupo de recursos](#list-clusters-resource-group)
* [Listado de clústeres por identificador de suscripción](#list-clusters-subscription)

### <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>Creación de un clúster de instancia administrada

Cree un clúster de Azure Managed Instance for Apache Cassandra:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
location='West US'
delegatedManagementSubnetId='/subscriptions/<subscription id>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/management'
initialCassandraAdminPassword='myPassword'

# You can override the cluster name if the original name is not legal for an Azure resource.
# overrideClusterName='ClusterNameIllegalForAzureResource'
# the default Cassandra version is v3.11

az managed-cassandra cluster create \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName \
    --location $location \
    --delegated-management-subnet-id $delegatedManagementSubnetId \
    --initial-cassandra-admin-password $initialCassandraAdminPassword \
```

### <a name="delete-a-managed-instance-cluster"></a><a id="delete-cluster"></a>Eliminación de un clúster de instancia administrada

Elimine un clúster:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster delete \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-details"></a><a id="get-cluster-details"></a>Obtención de los detalles del clúster

Obtenga los detalles del clúster:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster show \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-node-status"></a><a id="get-cluster-status"></a>Obtención del estado del nodo de clúster

Obtenga los detalles del clúster:

```azurecli-interactive
clusterName='cassandra-hybrid-cluster'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster node-status \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="list-the-clusters-by-resource-group"></a><a id="list-clusters-resource-group"></a>Listado de clústeres por grupo de recursos

Enumere los clústeres por grupo de recursos:

```azurecli-interactive
subscriptionId='MySubscriptionId'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster list\
    --resource-group $resourceGroupName
```

### <a name="list-clusters-by-subscription-id"></a><a id="list-clusters-subscription"></a>Listado de clústeres por identificador de suscripción

Enumere los clústeres por identificador de suscripción:

```azurecli-interactive
# set your subscription id
az account set -s <subscription id>

az managed-cassandra cluster list
```

## <a name="the-managed-instance-datacenters"></a><a id="managed-instance-datacenter"></a>Centros de datos de instancia administrada

En las secciones siguientes se muestra cómo administrar centro de datos de Azure Managed Instance for Apache Cassandra:

* [Creación de un centro de datos](#create-datacenter)
* [Eliminación de un centro de datos](#delete-datacenter)
* [Obtención de los detalles del centro de datos](#get-datacenter-details)
* [Actualización o escalabilidad de un centro de datos](#update-datacenter)
* [Obtención de centro de datos en un clúster](#get-datacenters-cluster)

### <a name="create-a-datacenter"></a><a id="create-datacenter"></a>Creación de un centro de datos

Cree un centro de datos:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus2'
delegatedSubnetId='/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter create \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --data-center-location $dataCenterLocation \
    --delegated-subnet-id $delegatedSubnetId \
    --node-count 3 
```

### <a name="delete-a-datacenter"></a><a id="delete-datacenter"></a>Eliminación de un centro de datos

Elimine un centro de datos:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter delete \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="get-datacenter-details"></a><a id="get-datacenter-details"></a>Obtención de detalles del centro de datos

Obtenga detalles del centro de datos:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter show \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="update-or-scale-a-datacenter"></a><a id="update-datacenter"></a>Actualización o escalabilidad de un centro de datos

Actualice o escale un centro de datos (para la escalabilidad, cambie el valor de nodeCount):

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus'
delegatedSubnetId= '/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter update \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --node-count 13 
```

### <a name="get-the-datacenters-in-a-cluster"></a><a id="get-datacenters-cluster"></a>Obtención de los centro de datos de un clúster

Obtenga centros de datos en un clúster:

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra datacenter list \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName
```

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un clúster de instancia administrada desde Azure Portal](create-cluster-portal.md)
* [Implementación de un clúster de Apache Spark administrado con Azure Databricks](deploy-cluster-databricks.md)