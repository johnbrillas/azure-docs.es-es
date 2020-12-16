---
title: Instalación de aplicaciones existentes con Helm en AKS
description: Aprenda a usar la herramienta de empaquetado de Helm para implementar contenedores en un clúster de Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
ms.topic: article
ms.date: 12/07/2020
ms.author: zarhoads
ms.openlocfilehash: f12dffe0b538738a8f6dd00cd3d87d44da828f21
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96779174"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Instalación de aplicaciones existentes con Helm en Azure Kubernetes Service (AKS)

[Helm][helm] es una herramienta de empaquetado de código abierto que ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como *APT* y *Yum*, se utiliza para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados.

En este artículo se muestra cómo configurar y usar Helm en un clúster de Kubernetes en AKS.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También necesita tener instalada la CLI de Helm, que es el cliente que se ejecuta en el sistema de desarrollo. Permite iniciar, detener y administrar las aplicaciones con Helm. Si usa Azure Cloud Shell, la CLI de Helm ya está instalada. Para obtener las instrucciones de instalación en su plataforma local, consulte [Instalación de Helm][helm-install].

> [!IMPORTANT]
> Helm está diseñada para ejecutarse en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Helm solo se programan para ejecutarse en nodos de Linux. También deberá asegurarse de que todos los gráficos de Helm que instale están programados también para ejecutarse en los nodos correctos. Los comandos de este artículo utilizan selectores de nodo [k8s-node-selector] para asegurarse de que los pods están programados para los nodos correctos, pero no todos los gráficos de Helm pueden exponer un selector de nodo. También puede considerar otras opciones en el clúster, como los valores [taints][taints].

## <a name="verify-your-version-of-helm"></a>Comprobación de la versión de Helm

Use el comando `helm version` para comprobar que tiene Helm 3 instalado:

```console
helm version
```

En el siguiente ejemplo se muestra la versión 3.0.0 de Helm instalada:

```console
$ helm version

version.BuildInfo{Version:"v3.0.0", GitCommit:"e29ce2a54e96cd02ccfce88bee4f58bb6e2a28b6", GitTreeState:"clean", GoVersion:"go1.13.4"}
```

## <a name="install-an-application-with-helm-v3"></a>Instalación de una aplicación con Helm v3

### <a name="add-helm-repositories"></a>Adición de repositorios de Helm

Use el comando [helm repo][helm-repo-add] para agregar el repositorio *ingress-nginx*.

```console
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

### <a name="find-helm-charts"></a>Búsqueda de gráficos de Helm

Los gráficos de Helm se usan para implementar aplicaciones en un clúster de Kubernetes. Para buscar los gráficos de Helm creados previamente, use el comando [helm search][helm-search]:

```console
helm search repo ingress-nginx
```

En la siguiente salida de ejemplo condensada se muestran algunos de los gráficos de Helm disponibles para usar:

```console
$ helm search repo ingress-nginx

NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                       
ingress-nginx/ingress-nginx     2.12.0          0.34.1          Ingress controller for Kubernetes using NGINX a...
```

Para actualizar la lista de gráficos, use el comando [helm repo update][helm-repo-update].

```console
helm repo update
```

El ejemplo siguiente muestra una actualización de repositorio correcta:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "ingress-nginx" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

### <a name="run-helm-charts"></a>Ejecución de gráficos de Helm

Para instalar los gráficos con Helm, use el comando [helm install][helm-install-command] y especifique un nombre de versión y el nombre del gráfico que se va a instalar. Para ver cómo funciona la instalación de un gráfico de Helm, vamos a instalar una implementación básica de nginx mediante un gráfico de Helm.

```console
helm install my-nginx-ingress ingress-nginx/ingress-nginx \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

En la siguiente salida de ejemplo condensada se muestra el estado de implementación de los recursos de Kubernetes creados mediante el gráfico de Helm:

```console
$ helm install my-nginx-ingress ingress-nginx/ingress-nginx \
>     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
>     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME: my-nginx-ingress
LAST DEPLOYED: Fri Nov 22 10:08:06 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller'
...
```

Use el comando `kubectl get services` para obtener el valor de *EXTERNAL-IP* del servicio.

```console
kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller
```

Por ejemplo, el siguiente comando muestra el valor de *EXTERNAL-IP* para el servicio *my-nginx-ingress-ingress-nginx-controller*:

```console
$ kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller

NAME                                        TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)                      AGE   SELECTOR
my-nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.2.237   <EXTERNAL-IP>    80:31380/TCP,443:32239/TCP   72s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=my-nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

### <a name="list-releases"></a>Lista de versiones

Para ver una lista de las versiones instaladas en el clúster, use el comando `helm list`.

```console
helm list
```

En el siguiente ejemplo se muestra la versión de *my-nginx-ingress* implementada en el paso anterior:

```console
$ helm list

NAME                NAMESPACE   REVISION    UPDATED                                 STATUS      CHART                   APP VERSION
my-nginx-ingress    default     1           2019-11-22 10:08:06.048477 -0600 CST    deployed    nginx-ingress-1.25.0    0.26.1 
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiar estos recursos, use el comando [helm uninstall][helm-cleanup] y especifique el nombre de la versión, tal como aparecía en el comando `helm list` anterior.

```console
helm uninstall my-nginx-ingress
```

En el siguiente ejemplo se muestra que se ha desinstalado la versión denominada *my-nginx-ingress*:

```console
$ helm uninstall my-nginx-ingress

release "my-nginx-ingress" uninstalled
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar las implementaciones de aplicaciones de Kubernetes con Helm, consulte la documentación de Helm.

> [!div class="nextstepaction"]
> [Documentación de Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-cleanup]: https://helm.sh/docs/intro/using_helm/#helm-uninstall-uninstalling-a-release
[helm-documentation]: https://helm.sh/docs/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-command]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-repo-add]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helm-search]: https://helm.sh/docs/intro/using_helm/#helm-search-finding-charts
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
            
<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[taints]: operator-best-practices-advanced-scheduler.md
