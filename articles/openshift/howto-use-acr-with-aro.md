---
title: Uso de Azure Container Registry con Red Hat OpenShift en Azure
description: Aprenda a extraer y ejecutar un contenedor de Azure Container Registry en un clúster de Red Hat OpenShift en Azure.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 01/10/2021
ms.openlocfilehash: 651b73db084e8090f59faeffa9991c2ac468ca08
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100634420"
---
# <a name="use-azure-container-registry-with-azure-red-hat-openshift-aro"></a>Uso de Azure Container Registry con Red Hat OpenShift en Azure (ARO)

Azure Container Registry (ACR) es un servicio de registro de contenedor administrado que puede usar para almacenar imágenes de contenedor de Docker privadas con funcionalidades empresariales como la replicación geográfica. Para acceder al ACR desde un clúster de ARO, el clúster se puede autenticar con ACR mediante el almacenamiento de las credenciales de inicio de sesión de Docker en un secreto de Kubernetes.  Del mismo modo, un clúster de ARO puede usar un elemento imagePullSecret en la especificación de pod para autenticarse en el registro al extraer la imagen. En este artículo, aprenderá a configurar una instancia de Azure Container Registry con un clúster de Red Hat OpenShift en Azure para almacenar y extraer imágenes de contenedor de Docker privadas.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que tiene una instancia de Azure Container Registry. Si no se así, use Azure Portal o las [instrucciones para la CLI de Azure](../container-registry/container-registry-get-started-azure-cli.md) para crear un registro de contenedor.

En este artículo también se presupone que tiene un clúster de Red Hat OpenShift en Azure y que tiene instalada la CLI de `oc`. Si no es así, siga las instrucciones del [tutorial para crear un clúster de ARO](tutorial-create-cluster.md).

## <a name="get-a-pull-secret"></a>Obtención de un secreto de extracción

Necesitará un secreto de extracción de ACR para acceder al registro desde el clúster de ARO.

Para obtener las credenciales del secreto de extracción, puede usar Azure Portal o la CLI de Azure.

Si usa Azure Portal, navegue a su instancia de ACR y seleccione **Claves de acceso**.  `docker-username` es el nombre del registro de contenedor. Para `docker-password`, use password o password2.

![Claves de acceso](./media/acr-access-keys.png)

En su lugar, puede usar la CLI de Azure para obtener estas credenciales:
```azurecli
az acr credential show -n <your registry name>
```

## <a name="create-the-kubernetes-secret"></a>Creación del secreto de Kubernetes

Ahora usaremos estas credenciales para crear un secreto de Kubernetes. Ejecute el comando siguiente con sus credenciales de ACR:

```
oc create secret docker-registry \
    --docker-server=<your registry name>.azurecr.io \
    --docker-username=<your registry name> \
    --docker-password=******** \
    --docker-email=unused \
    acr-secret
```

>[!NOTE]
>Este secreto se almacenará en el proyecto de OpenShift actual (espacio de nombres de Kubernetes) y solo podrán hacer referencia a él los pods creados en ese proyecto.  Para más instrucciones sobre cómo crear un secreto de extracción para todo el clúster, consulte este [documento](https://docs.openshift.com/container-platform/4.4/openshift_images/managing_images/using-image-pull-secrets.html).

## <a name="create-a-pod-using-a-private-registry-image"></a>Creación de un pod con una imagen de registro privada

Ahora que conectamos el clúster de ARO a la instancia de ACR, vamos a extraer una imagen de ACR para crear un pod.

Comience con podSpec y especifique el secreto que creó como imagePullSecret:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world
spec:
  containers:
  - name: hello-world
    image: <your registry name>.azurecr.io/hello-world:v1
  imagePullSecrets:
  - name: acr-secret
```

Para comprobar que el pod está en funcionamiento, ejecute este comando y espere hasta que el estado sea **En ejecución**:

```bash
$ oc get pods --watch
NAME         READY   STATUS    RESTARTS   AGE
hello-world  1/1     Running   0          30s
```

## <a name="next-steps"></a>Pasos siguientes

* [Azure Container Registry](../container-registry/container-registry-concepts.md)
* [Inicio rápido: Creación de un registro de contenedor privado con la CLI de Azure](../container-registry/container-registry-get-started-azure-cli.md)
