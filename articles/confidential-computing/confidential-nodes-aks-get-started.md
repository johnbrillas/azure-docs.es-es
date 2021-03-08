---
title: 'Inicio rápido: Implementación de un clúster de Azure Kubernetes Service mediante la CLI de Azure con nodos de computación confidencial'
description: Aprenda a crear un clúster de AKS con nodos confidenciales e implementar una aplicación Hola mundo mediante la CLI de Azure.
author: agowdamsft
ms.service: container-service
ms.topic: quickstart
ms.date: 2/25/2020
ms.author: amgowda
ms.openlocfilehash: 51b0813849236d9335d1482019f740fc8b23749f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703293"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-with-confidential-computing-nodes-dcsv2-using-azure-cli"></a>Inicio rápido: Implementación de un clúster de Azure Kubernetes Service (AKS) con nodos de computación confidencial (DCsv2) mediante la CLI de Azure

Este inicio rápido está destinado a aquellos desarrolladores u operadores de clúster que desean crear rápidamente un clúster de AKS e implementar una aplicación que supervise las aplicaciones mediante el servicio Kubernetes administrado en Azure. También puede aprovisionar el clúster y agregar nodos de computación confidencial desde Azure Portal.

## <a name="overview"></a>Información general

En este inicio rápido, aprenderá a implementar un clúster de Azure Kubernetes Service (AKS) con nodos de computación confidencial mediante la CLI de Azure y ejecutar una aplicación Hola mundo sencilla en un enclave. AKS es un servicio de Kubernetes administrado que permite implementar y administrar clústeres rápidamente. [Aquí](../aks/intro-kubernetes.md) encontrará más información sobre AKS.

> [!NOTE]
> Las máquinas virtuales de la serie DCsv2 de computación confidencial sacan provecho de hardware especializado que está sujeto a una mayor disponibilidad de precios y regiones. Para más información, consulte en la página de máquinas virtuales [las SKU disponibles y las regiones que se admiten](virtual-machine-solutions.md).

### <a name="confidential-computing-node-features-dcxs-v2"></a>Características de los nodos de computación confidencial (DC<x>s-v2)

1. Nodos de trabajo Linux que admiten solo contenedores Linux
1. Máquina virtual de generación 2 con nodos de Virtual Machines de Ubuntu 18.04
1. CPU basada en Intel SGX con memoria caché de páginas cifrada (EPC). Obtenga más información [aquí](./faq.md)
1. Se admite la versión 1.16+ de Kubernetes
1. Controlador Intel SGX DCAP preinstalado previamente en los nodos de AKS Obtenga más información [aquí](./faq.md)

## <a name="deployment-prerequisites"></a>Requisitos previos de implementación
El tutorial de implementación requiere lo siguiente:

1. Una suscripción a Azure activa. Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
1. La versión 2.0.64 de la CLI de Azure, o cualquier versión posterior, instalada y configurada en la máquina de implementación (ejecute `az --version` para encontrar la versión). Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](../container-registry/container-registry-get-started-azure-cli.md)
1. Mínimo de seis núcleos de **DC<x>s-v2** disponibles en su suscripción para su uso. De forma predeterminada, la cuota de núcleos de máquina virtual para la computación confidencial por suscripción de Azure es 8 núcleos. Si planea aprovisionar un clúster que requiera más de 8 núcleos, siga [estas](../azure-portal/supportability/per-vm-quota-requests.md) instrucciones para generar una incidencia de aumento de cuota.

## <a name="creating-new-aks-cluster-with-confidential-computing-nodes-and-add-on"></a>Creación de un clúster de AKS con nodos y complementos de computación confidencial
Siga las instrucciones siguientes para agregar nodos compatibles con la computación confidencial con el complemento.

### <a name="step-1-creating-an-aks-cluster-with-system-node-pool"></a>Paso 1: Creación de un clúster de AKS con un grupo de nodos del sistema

Si ya tiene un clúster de AKS que cumpla los requisitos anteriores, [vaya a la sección de clúster existente](#existing-cluster) para agregar un nuevo grupo de nodos de computación confidencial.

Primero, cree un grupo de recursos para el clúster con el comando az group create. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región *westus2*:

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Ahora cree un clúster de AKS con el comando az aks create.

```azurecli-interactive
# Create a new AKS cluster with system node pool with Confidential Computing addon enabled
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addon confcom
```
La operación anterior crea un clúster de AKS con un grupo de nodos del sistema con el complemento habilitado. Ahora, agregue un nodo de usuario del tipo grupo de nodos de computación confidencial en AKS (DCsv2)

### <a name="step-2-adding-confidential-computing-node-pool-to-aks-cluster"></a>Paso 2: Adición de un grupo de nodos de computación confidencial al clúster de AKS 

Ejecute el comando siguiente en un grupo de nodos de usuario de un tamaño `Standard_DC2s_v2` con 3 nodos. [Aquí](../virtual-machines/dcv2-series.md) puede elegir otra lista admitida de SKU y regiones de DCsv2:

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-vm-size Standard_DC2s_v2
```
El comando anterior es un grupo de nodos nuevo con **DC<x>s-v2** que debe estar visible con los daemonsets del complemento de computación confidencial ([complemento de dispositivo SGX](confidential-nodes-aks-overview.md#sgx-plugin)).
 
### <a name="step-3-verify-the-node-pool-and-add-on"></a>Paso 3: Comprobación del complemento y del grupo de nodos
Obtenga las credenciales para el clúster de AKS mediante el comando az aks get-credentials:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```
Compruebe que los nodos se crean correctamente y que los daemonsets relacionados con SGX se ejecutan en grupos de nodos de **DC<x>s-v2** mediante el comando kubectl get pods & nodes, como se muestra a continuación:

```console
$ kubectl get pods --all-namespaces

output
kube-system     sgx-device-plugin-xxxx     1/1     Running
```
Si el resultado es como el anterior, el clúster de AKS ya está listo para ejecutar aplicaciones confidenciales.

Vaya a la sección de implementación de [Hola mundo desde enclave](#hello-world) para probar una aplicación en un enclave. O bien, siga las instrucciones que encontrará a continuación si desea agregar grupos de nodos adicionales en AKS (AKS admite la mezcla de grupos de nodos de SGX y grupos de nodos que no son de SGX).

## <a name="adding-confidential-computing-node-pool-to-existing-aks-cluster"></a>Adición de un grupo de nodos de computación confidencial a un clúster de AKS existente<a id="existing-cluster"></a>

En esta sección se supone que ya tiene un clúster de AKS que cumple con los criterios que se enumeran en la sección de requisitos previos (se aplica al complemento).

### <a name="step-1-enabling-the-confidential-computing-aks-add-on-on-the-existing-cluster"></a>Paso 1: Habilitación del complemento de AKS de computación confidencial en el clúster existente

Ejecute el siguiente comando para habilitar el complemento de computación confidencial.

```azurecli-interactive
az aks enable-addons --addons confcom --name MyManagedCluster --resource-group MyResourceGroup 
```
### <a name="step-2-add-dcxs-v2-user-node-pool-to-the-cluster"></a>Paso 2: Adición del grupo de nodos de usuario **DC<x>s-v2** al clúster
    
> [!NOTE]
> Para usar la funcionalidad de computación confidencial, el clúster de AKS existente debe tener al menos un grupo de nodos basado en una SKU de máquina virtual **DC<x>s-v2**. Obtenga más información sobre la computación confidencial de las SKU de las máquinas virtuales DCsv2 aquí [SKU disponibles y regiones admitidas](virtual-machine-solutions.md).
    
  ```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-count 1 --node-vm-size Standard_DC4s_v2

output node pool added

Verify

az aks nodepool list --cluster-name myAKSCluster --resource-group myResourceGroup
```
El comando anterior debe mostrar el grupo de nodos reciente que agregó con el nombre confcompool1.

### <a name="step-3-verify-that-daemonsets-are-running-on-confidential-node-pools"></a>Paso 3: Comprobación de que los daemonsets se ejecutan en grupos de nodos confidenciales

Inicie sesión en el clúster de AKS existente para realizar la siguiente comprobación. 

```console
kubectl get nodes
```
La salida debe mostrar el elemento confcompool1 recién agregado en el clúster de AKS.

```console
$ kubectl get pods --all-namespaces

output (you may also see other daemonsets along SGX daemonsets as below)
kube-system     sgx-device-plugin-xxxx     1/1     Running
```
Si el resultado es como el anterior, el clúster de AKS ya está listo para ejecutar aplicaciones confidenciales. Realice la siguiente implementación de prueba de la aplicación.

## <a name="hello-world-from-isolated-enclave-application"></a>Hola mundo desde una aplicación de enclave aislada <a id="hello-world"></a>
Cree un archivo denominado *hello-world-enclave.yaml* y pegue el siguiente manifiesto YAML. Este código de aplicación de ejemplo basado en Open Enclave se puede encontrar en el [proyecto de Open Enclave](https://github.com/openenclave/openenclave/tree/master/samples/helloworld). En la siguiente implementación se supone que ha implementado el complemento "confcom".

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 5 # This limit will automatically place the job into confidential computing node. Alternatively you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
  ```

Ahora, use el comando kubectl apply para crear un trabajo de ejemplo que se iniciará en un enclave seguro, como se muestra en la siguiente salida de ejemplo:

```console
$ kubectl apply -f hello-world-enclave.yaml

job "sgx-test" created
```

Puede confirmar que la carga de trabajo ha creado correctamente un entorno de ejecución de confianza (enclave) mediante la ejecución de los siguientes comandos:

```console
$ kubectl get jobs -l app=sgx-test
```

```console
$ kubectl get jobs -l app=sgx-test
NAME       COMPLETIONS   DURATION   AGE
sgx-test   1/1           1s         23s
```

```console
$ kubectl get pods -l app=sgx-test
```

```console
$ kubectl get pods -l app=sgx-test
NAME             READY   STATUS      RESTARTS   AGE
sgx-test-rchvg   0/1     Completed   0          25s
```

```console
$ kubectl logs -l app=sgx-test
```

```console
$ kubectl logs -l app=sgx-test
Hello world from the enclave
Enclave called into host to print: Hello World!
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar los grupos de nodos asociados o eliminar el clúster de AKS, use los siguientes comandos:

Eliminación del clúster de AKS
``````azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```
Removing the confidential computing node pool

``````azurecli-interactive
az aks nodepool delete --cluster-name myAKSCluster --name myNodePoolName --resource-group myResourceGroup
``````

## <a name="next-steps"></a>Pasos siguientes

Ejecute las aplicaciones Python, Node etc. de forma confidencial mediante contenedores confidenciales visitando [ejemplos de contenedores confidenciales](https://github.com/Azure-Samples/confidential-container-samples).

Para ejecutar aplicaciones compatibles con enclave, visite [ejemplos de contenedores de Azure compatibles con enclave](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/).
