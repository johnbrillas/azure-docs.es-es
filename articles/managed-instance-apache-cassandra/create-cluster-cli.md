---
title: 'Inicio rápido: uso de la CLI para crear un clúster de Azure Managed Instance for Apache Cassandra'
description: Use este inicio rápido para crear un clúster de Azure Managed Instance for Apache Cassandra mediante CLI de Azure.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: quickstart
ms.date: 03/02/2021
ms.openlocfilehash: 86fa7e2e45dacb86b6601b699dca46b1b909fd08
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102424706"
---
# <a name="quickstart-create-an-azure-managed-instance-for-apache-cassandra-cluster-using-azure-cli-preview"></a>Inicio rápido: creación de un clúster de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure (versión preliminar)

Azure Managed Instance for Apache Cassandra proporciona operaciones de implementación y escalado automatizadas para los centros de datos administrados de código abierto de Apache Cassandra. Este servicio le ayuda a acelerar los escenarios híbridos y a reducir el mantenimiento continuo.

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este inicio rápido se muestra cómo usar los comandos de la CLI de Azure para crear un clúster con Azure Managed Instance for Apache Cassandra. También se muestra cómo crear un centro de recursos y escalar o reducir verticalmente los nodos en el centro de seguridad.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* Este artículo requiere la CLI de Azure 2.12.1 o una versión posterior. Si usa Azure Cloud Shell, la versión más reciente ya está instalada.

* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) con conectividad a su entorno autohospedado o local. Para obtener más información sobre cómo conectar entornos locales a Azure, consulte el artículo [Conexión de una red local a Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/).

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


## <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>Creación de un clúster de instancia administrada

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

1. Establezca el identificador de la suscripción en la CLI de Azure:

   ```azurecli-interactive
   az account set -s <Subscription_ID>
   ```

1. A continuación, cree una red virtual con una subred dedicada en el grupo de recursos:

   ```azurecli-interactive
   az network vnet create -n <VNet_Name> -l eastus2 -g <Resource_Group_Name> --subnet-name <Subnet Name>
   ```

1. Aplique algunos permisos especiales a la red virtual y a la subred, ya que son necesarios para la instancia administrada. Utilice el comando `az role assignment create`, reemplazando `<subscription ID>`, `<resource group name>`, `<VNet name>` y `<subnet name>` con los valores adecuados:

   ```azurecli-interactive
   az role assignment create --assignee e5007d2c-4b13-4a74-9b6a-605d99f03501 --role 4d97b98b-1d4f-4787-a291-c67834d212e7 --scope /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/<VNet name>/subnets/<subnet name>
   ```

   > [!NOTE]
   > Los valores `assignee` y `role` del comando anterior son los identificadores de rol y principio de servicio fijos, respectivamente.

1. A continuación, cree el clúster en la red virtual recién creada. Ejecute el siguiente comando y asegúrese de usar el valor `Resource ID` obtenido en el comando anterior como valor de la variable `delegatedManagementSubnetId`:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster_Name>'
   location='eastus2'
   delegatedManagementSubnetId='<Resource_ID>'
   initialCassandraAdminPassword='myPassword'
    
   az managed-cassandra cluster create \
      --cluster-name $clusterName \
      --resource-group $resourceGroupName \
      --location $location \
      --delegated-management-subnet-id $delegatedManagementSubnetId \
      --initial-cassandra-admin-password $initialCassandraAdminPassword \
      --debug
   ```

1. Por último, cree un centro de datos para el clúster, con tres nodos:

   ```azurecli-interactive
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
   delegatedSubnetId='<Resource_ID>'
    
   az managed-cassandra datacenter create \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --data-center-location $dataCenterLocation \
      --delegated-subnet-id $delegatedSubnetId \
      --node-count 3 
   ```

1. Una vez creado el centro de datos, si desea escalar o reducir verticalmente los nodos del centro de datos, ejecute el siguiente comando. Cambie el valor del parámetro `node-count` al valor deseado:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster Name>'
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
   delegatedSubnetId= '<Resource_ID>'
    
   az managed-cassandra datacenter update \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --node-count 9 
   ```

## <a name="connect-to-your-cluster"></a>Conexión al clúster

Azure Managed Instance for Apache Cassandra no crea nodos con direcciones IP públicas. Para conectarse al clúster de Cassandra recién creado, tiene que crear otro recurso dentro de la red virtual. Este recurso puede ser una aplicación o una máquina virtual que tenga instalada la herramienta de consulta de código abierto de Apache [CQLSH](https://cassandra.apache.org/doc/latest/tools/cqlsh.html). Puede usar una [plantilla de Resource Manager](https://azure.microsoft.com/resources/templates/101-vm-simple-linux/) para implementar una máquina virtual de Ubuntu. Tras la implementación, use SSH para conectarse a la máquina e instalar CQLSH, tal y como se muestra en los siguientes comandos:

```bash
# Install default-jre and default-jdk
sudo apt update
sudo apt install openjdk-8-jdk openjdk-8-jre

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to CQLSH (replace <IP> with the private IP addresses of the nodes in your Datacenter):
host=("<IP>" "<IP>" "<IP>")
cqlsh $host 9042 -u cassandra -p cassandra --ssl
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el comando `az group delete` para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados cuando ya no se necesiten:

```azurecli-interactive
az group delete --name <Resource_Group_Name>
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a crear un clúster de Azure Managed Instance for Apache Cassandra mediante la CLI de Azure. Ahora puede empezar a trabajar con el clúster:

> [!div class="nextstepaction"]
> [Implementación de un clúster de Apache Spark administrado con Azure Databricks](deploy-cluster-databricks.md)
