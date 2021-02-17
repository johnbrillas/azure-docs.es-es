---
title: Conexión de un clúster de Kubernetes habilitado para Azure Arc (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Conexión de un clúster de Kubernetes habilitado para Azure Arc
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: b4ab84153eaaf81c668d8589fec7516853aca5f9
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "100008118"
---
# <a name="connect-an-azure-arc-enabled-kubernetes-cluster-preview"></a>Conexión de un clúster de Kubernetes habilitado para Azure Arc (versión preliminar)

En este artículo se describe el proceso de conexión de cualquier clúster de Kubernetes certificado de Cloud Native Computing Foundation (CNCF), como AKS-Engine en Azure, AKS-Engine en Azure Stack Hub, GKE, EKS y clúster de VMware vSphere, con Azure Arc.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que ha preparado los siguientes requisitos previos:

* Un clúster de Kubernetes en funcionamiento. Si no tiene un clúster de Kubernetes existente, puede usar una de las siguientes guías para crear un clúster de prueba:
  * Creación de un clúster de Kubernetes mediante [Kubernetes en Docker (Kind)](https://kind.sigs.k8s.io/).
  * Creación de un clúster de Kubernetes mediante Docker para [Mac](https://docs.docker.com/docker-for-mac/#kubernetes) o [Windows](https://docs.docker.com/docker-for-windows/#kubernetes).
* Un archivo kubeconfig para acceder al clúster y al rol Administrador del clúster en el clúster para la implementación de los agentes de Kubernetes habilitados para Arc.
* El usuario o la entidad de servicio que se usa con los comandos `az login` y `az connectedk8s connect` debe tener los permisos de "Lectura" y "Escritura" en el tipo de recurso "Microsoft.Kubernetes/connectedclusters". El rol "Kubernetes Cluster - Azure Arc Onboarding" (Clúster de Kubernetes - Incorporación de Azure Arc) tiene estos permisos y se puede usar para las asignaciones de roles en el usuario o la entidad de servicio.
* Helm 3 para la incorporación del clúster mediante una extensión connectedk8s. [Instale la última versión de Helm 3](https://helm.sh/docs/intro/install) para cumplir este requisito.
* La versión 2.15, o cualquier versión posterior, de la CLI de Azure para instalar las extensiones de la CLI de Kubernetes habilitado para Azure Arc. [Instalación la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) o realice la actualización a la versión más reciente.
* Instale las extensiones de la CLI de Kubernetes habilitado para Arc:
  
  * Instale la extensión `connectedk8s`, que ayuda a conectar clústeres de Kubernetes a Azure:
  
  ```azurecli
  az extension add --name connectedk8s
  ```
  
   * Instale la extensión `k8sconfiguration`:
  
  ```azurecli
  az extension add --name k8sconfiguration
  ```

  * Si quiere actualizar estas extensiones más adelante, ejecute los siguientes comandos:
  
  ```azurecli
  az extension update --name connectedk8s
  az extension update --name k8sconfiguration
  ```

## <a name="supported-regions"></a>Regiones admitidas

* Este de EE. UU.
* Oeste de Europa

## <a name="network-requirements"></a>Requisitos de red

Los agentes de Azure Arc requieren que funcionen los siguientes protocolos, puertos y direcciones URL de salida:

* TCP en el puerto 443: `https://:443`
* TCP en el puerto 9418: `git://:9418`

| Punto de conexión (DNS)                                                                                               | Descripción                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `https://management.azure.com`                                                                                 | Necesario para que el agente se conecte a Azure y registre el clúster.                                                        |
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com` | Punto de conexión de plano de datos para que el agente inserte información de la configuración de estado y recuperación de cambios.                                      |
| `https://login.microsoftonline.com`                                                                            | Necesario para capturar y actualizar tokens de Azure Resource Manager.                                                                                    |
| `https://mcr.microsoft.com`                                                                            | Necesario para extraer imágenes de contenedor para agentes de Azure Arc.                                                                  |
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`                                                                            |  Necesario para extraer certificados de identidad administrados que haya asignado el sistema.                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Registre los dos proveedores en Kubernetes habilitado para Azure Arc:

```console
az provider register --namespace Microsoft.Kubernetes

az provider register --namespace Microsoft.KubernetesConfiguration
```

El registro es un proceso asincrónico que puede tardar aproximadamente 10 minutos. Puede supervisar el proceso de registro con los comandos siguientes:

```console
az provider show -n Microsoft.Kubernetes -o table
```

```console
az provider show -n Microsoft.KubernetesConfiguration -o table
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Use un grupo de recursos para almacenar los metadatos del clúster.

En primer lugar, cree un grupo de recursos para almacenar el recurso de clúster conectado.

```console
az group create --name AzureArcTest -l EastUS -o table
```

**Salida:**

```console
Location    Name
----------  ------------
eastus      AzureArcTest
```

## <a name="connect-a-cluster"></a>Conexión de un clúster

A continuación, conectaremos el clúster de Kubernetes a Azure mediante `az connectedk8s connect`:

1. Compruebe la conectividad con el clúster de Kubernetes a través de:
   1. `KUBECONFIG`
   1. `~/.kube/config`
   1. `--kube-config`
1. Implemente agentes de Azure Arc para Kubernetes mediante Helm 3 en el espacio de nombres `azure-arc`:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
```

**Salida:**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Helm release deployment succeeded

{
  "aadProfile": {
    "clientAppId": "",
    "serverAppId": "",
    "tenantId": ""
  },
  "agentPublicKeyCertificate": "...",
  "agentVersion": "0.1.0",
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1",
  "identity": {
    "principalId": null,
    "tenantId": null,
    "type": "None"
  },
  "kubernetesVersion": "v1.15.0",
  "location": "eastus",
  "name": "AzureArcTest1",
  "resourceGroup": "AzureArcTest",
  "tags": {},
  "totalNodeCount": 1,
  "type": "Microsoft.Kubernetes/connectedClusters"
}
```

## <a name="verify-connected-cluster"></a>Comprobación del clúster conectado

Use el comando siguiente para enumerar los clústeres conectados:

```console
az connectedk8s list -g AzureArcTest -o table
```

**Salida:**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Name           Location    ResourceGroup
-------------  ----------  ---------------
AzureArcTest1  eastus      AzureArcTest
```

También puede ver este recurso en [Azure Portal](https://portal.azure.com/). Abra el portal en el explorador y vaya al grupo de recursos y al recurso de Kubernetes habilitado para Azure Arc, según las entradas de nombre de recurso y nombre del grupo de recursos que se usaron anteriormente en el comando `az connectedk8s connect`.

> [!NOTE]
> Después de la incorporación del clúster, los metadatos del mismo (versión del clúster, versión del agente, número de nodos, etc.) tardan entre 5 y 10 minutos en aparecer en la página de información general del recurso de Kubernetes compatible con Azure Arc en Azure Portal.

## <a name="connect-using-an-outbound-proxy-server"></a>Conexión mediante un servidor proxy de salida

Si el clúster se encuentra detrás de un servidor proxy de salida, la CLI de Azure y los agentes de Kubernetes habilitado para Arc deben enrutar sus solicitudes a través del servidor proxy de salida:

1. Compruebe la versión de la extensión `connectedk8s` instalada en la máquina:

    ```console
    az -v
    ```

    Necesita la versión 0.2.5, o cualquier versión superior, de la extensión de `connectedk8s` para configurar los agentes con el proxy de salida. Si tiene la versión 0.2.3, o cualquier versión anterior, en la máquina, siga los [pasos de actualización](#before-you-begin) para obtener la versión más reciente de la extensión en la máquina.

2. Establezca las variables de entorno necesarias para que CLI de Azure use el servidor proxy de salida:

    * Si usa bash, ejecute el siguiente comando con los valores adecuados:

        ```bash
        export HTTP_PROXY=<proxy-server-ip-address>:<port>
        export HTTPS_PROXY=<proxy-server-ip-address>:<port>
        export NO_PROXY=<cluster-apiserver-ip-address>:<port>
        ```

    * Si usa PowerShell, ejecute el siguiente comando con los valores adecuados:

        ```powershell
        $Env:HTTP_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:HTTPS_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:NO_PROXY = "<cluster-apiserver-ip-address>:<port>"
        ```

3. Ejecute el comando connect con los parámetros de proxy especificados:

    ```console
    az connectedk8s connect -n <cluster-name> -g <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port> --proxy-skip-range <excludedIP>,<excludedCIDR> --proxy-cert <path-to-cert-file>
    ```

> [!NOTE]
> 1. Si especifica `excludedCIDR` en `--proxy-skip-range` es importante asegurarse de que la comunicación interna del clúster no se interrumpe para los agentes.
> 2. Aunque `--proxy-http`, `--proxy-https` y `--proxy-skip-range` se esperan para la mayoría de los entornos de proxy de salida, `--proxy-cert` solo es obligatorio si es necesario insertar certificados de confianza del proxy en el almacén de certificados de confianza de pods de agente.
> 3. Actualmente, la especificación de proxy anterior solo se aplica a los agentes de Arc y no a los pods de flujo utilizados en sourceControlConfiguration. El equipo de Kubernetes habilitado para Arc trabaja activamente en esta característica y pronto estará disponible.

## <a name="azure-arc-agents-for-kubernetes"></a>Agentes de Azure Arc para Kubernetes

Kubernetes habilitado para Azure Arc implementa varios operadores en el espacio de nombres `azure-arc`. Para ver estas implementaciones y pods, utilice:

```console
kubectl -n azure-arc get deployments,pods
```

**Salida:**

```console
NAME                                        READY      UP-TO-DATE  AVAILABLE  AGE
deployment.apps/cluster-metadata-operator     1/1             1        1      16h
deployment.apps/clusteridentityoperator       1/1             1        1      16h
deployment.apps/config-agent                  1/1             1        1      16h
deployment.apps/controller-manager            1/1             1        1      16h
deployment.apps/flux-logs-agent               1/1             1        1      16h
deployment.apps/metrics-agent                 1/1             1        1      16h
deployment.apps/resource-sync-agent           1/1             1        1      16h

NAME                                           READY    STATUS   RESTART AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

Kubernetes habilitado para Azure Arc consta de varios agentes (operadores) que se ejecutan en el clúster implementado en el espacio de nombres `azure-arc`.

| Agentes (operadores)                                                                                               | Descripción                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `deployment.apps/config-agent`                                                                                 | Busca en el clúster conectado los recursos de configuración de control de código fuente aplicados en el clúster y actualiza el estado de cumplimiento.                                                        |
| `deployment.apps/controller-manager` | Operador de operadores que organiza las interacciones entre los componentes de Azure Arc.                                      |
| `deployment.apps/metrics-agent`                                                                            | Recopila las métricas de rendimiento de otros agentes de Arc.                                                                                    |
| `deployment.apps/cluster-metadata-operator`                                                                            | Recopila metadatos del clúster, como la versión del clúster, el número de nodos y la versión del agente de Azure Arc.                                                                  |
| `deployment.apps/resource-sync-agent`                                                                            |  Sincroniza con Azure los metadatos de clúster mencionados anteriormente.                                                                  |
| `deployment.apps/clusteridentityoperator`                                                                            |  Kubernetes habilitado para Azure Arc admite actualmente la identidad asignada por el sistema. `clusteridentityoperator` mantiene el certificado de Managed Service Identity (MSI) que usan otros agentes para comunicarse con Azure.                                                                  |
| `deployment.apps/flux-logs-agent`                                                                            |  Recopila registros de los operadores de Flux implementados como parte de la configuración de control de código fuente.                                                                  |

## <a name="delete-a-connected-cluster"></a>Eliminación de un clúster conectado

Puede eliminar un recurso `Microsoft.Kubernetes/connectedcluster` mediante la CLI de Azure o Azure Portal.


* **Eliminación con la CLI de Azure**: Use el siguiente comando de la CLI de Azure para iniciar la eliminación del recurso Kubernetes habilitado para Azure Arc.
  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```
  Este comando quita el recurso `Microsoft.Kubernetes/connectedCluster` y los recursos `sourcecontrolconfiguration` asociados en Azure. La CLI de Azure usa `helm uninstall` para quitar los agentes que se ejecutan en el clúster también.

* **Eliminación en Azure Portal**: si se elimina el recurso Kubernetes habilitado para Azure Arc en Azure Portal, se elimina también el recurso `Microsoft.Kubernetes/connectedcluster` y todos los recursos `sourcecontrolconfiguration` asociados en Azure, pero *no* se eliminan los agentes que se ejecutan en el clúster. 

  Para quitar los agentes que se ejecutan en el clúster, ejecute el siguiente comando:

  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de GitOps en un clúster conectado](./use-gitops-connected-cluster.md)
* [Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)
