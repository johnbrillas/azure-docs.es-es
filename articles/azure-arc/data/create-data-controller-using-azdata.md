---
title: Creación de un controlador de datos con la CLI de datos de Azure (azdata)
description: Cree un controlador de datos de Azure Arc en un típico clúster de Kubernetes de varios nodos que ya haya creado mediante la CLI de datos de Azure (azdata).
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 3d2652d2f6c1bb56dd009a9e4de375c42786986d
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735006"
---
# <a name="create-azure-arc-data-controller-using-the-azure-data-cli-azdata"></a>Creación de un controlador de datos de Azure Arc mediante [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

Revise el tema [Creación del controlador de datos de Azure Arc](create-data-controller.md) para obtener información general.

Para crear el controlador de datos de Azure Arc mediante [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)], deberá tener [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] instalado.

   [Instale [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]](install-client-tools.md).

Independientemente de la plataforma de destino que elija, tendrá que establecer las siguientes variables de entorno antes de la creación para el usuario administrador del controlador de datos. Puede proporcionar estas credenciales a otras personas que necesiten tener acceso de administrador al controlador de datos según sea necesario.

**AZDATA_USERNAME**: nombre de usuario de su elección para el usuario administrador del controlador de datos. Ejemplo: `arcadmin`

**AZDATA_PASSWORD**: contraseña de su elección para el usuario administrador del controlador de datos. La contraseña debe tener al menos 8 caracteres y contener caracteres de tres de los siguientes cuatro conjuntos: mayúsculas, minúsculas, números y símbolos.

### <a name="linux-or-macos"></a>Linux o macOS

```console
export AZDATA_USERNAME="<your username of choice>"
export AZDATA_PASSWORD="<your password of choice>"
```

### <a name="windows-powershell"></a>Windows PowerShell

```console
$ENV:AZDATA_USERNAME="<your username of choice>"
$ENV:AZDATA_PASSWORD="<your password of choice>"
```

Tendrá que conectarse a un clúster de Kubernetes y autenticarse en él, además de tener un contexto de Kubernetes existente seleccionado antes de comenzar la creación del controlador de datos de Azure Arc. La forma en que se conecta a un clúster o servicio de Kubernetes varía; consulte la documentación de la distribución o el servicio de Kubernetes que usa para saber cómo conectarse al servidor de API de Kubernetes.

Puede comprobar que tiene una conexión de Kubernetes y confirmar el contexto actual con los siguientes comandos.

```console
kubectl get namespace
kubectl config current-context
```

### <a name="connectivity-modes"></a>Modos de conectividad

Como se describe en [Modos de conectividad y requisitos](./connectivity.md), el controlador de datos de Azure Arc se puede implementar con el modo de conectividad `direct` o `indirect`. Con el modo de conectividad `direct`, los datos de uso se envían automática y continuamente a Azure. En este artículo, los ejemplos especifican el modo de conectividad `direct` de la siguiente manera:

   ```console
   --connectivity-mode direct
   ```

   Para crear el controlador con el modo de conectividad `indirect`, actualice los scripts del ejemplo tal y como se especifica a continuación:

   ```console
   --connectivity-mode indirect
   ```

#### <a name="create-service-principal"></a>Creación de una entidad de servicio

Si va a implementar el controlador de datos de Azure Arc con el modo de conectividad `direct`, las credenciales de la entidad de servicio son necesarias para la conectividad de Azure. La entidad de servicio se usa para cargar datos y métricas de uso. 

Use estos comandos para crear la entidad de servicio de carga de métricas:

> [!NOTE]
> Para crear una entidad de servicio se necesitan [determinados permisos en Azure](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

Para crear una entidad de servicio, actualice el siguiente ejemplo. Reemplace `<ServicePrincipalName>` por el nombre de la entidad de servicio y ejecute el comando:

```azurecli
az ad sp create-for-rbac --name <ServicePrincipalName>
``` 

Si ha creado la entidad de servicio anteriormente y solo necesita obtener las credenciales actuales, ejecute el siguiente comando para restablecer las credenciales.

```azurecli
az ad sp credential reset --name <ServicePrincipalName>
```

Por ejemplo, para crear una entidad de servicio denominada `azure-arc-metrics`, ejecute el comando siguiente:

```console
az ad sp create-for-rbac --name azure-arc-metrics
```

Salida de ejemplo:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

Guarde los valores `appId`, `password` y `tenant` en una variable de entorno para su uso posterior. 

#### <a name="save-environment-variables-in-windows"></a>Almacenamiento de las variables de entorno en Windows

```console
SET SPN_CLIENT_ID=<appId>
SET SPN_CLIENT_SECRET=<password>
SET SPN_TENANT_ID=<tenant>
SET SPN_AUTHORITY=https://login.microsoftonline.com
```

#### <a name="save-environment-variables-in-linux-or-macos"></a>Almacenamiento de las variables de entorno en Linux o macOS

```console
export SPN_CLIENT_ID='<appId>'
export SPN_CLIENT_SECRET='<password>'
export SPN_TENANT_ID='<tenant>'
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

#### <a name="save-environment-variables-in-powershell"></a>Almacenamiento de las variables de entorno en PowerShell

```console
$Env:SPN_CLIENT_ID="<appId>"
$Env:SPN_CLIENT_SECRET="<password>"
$Env:SPN_TENANT_ID="<tenant>"
$Env:SPN_AUTHORITY="https://login.microsoftonline.com"
```

Una vez creada la entidad de servicio, asígnele el rol adecuado. 

### <a name="assign-roles-to-the-service-principal"></a>Asignación de roles a la entidad de servicio

Ejecute este comando para asignar la entidad de servicio al rol `Monitoring Metrics Publisher` en la suscripción donde se encuentran los recursos de la instancia de base de datos:

#### <a name="run-the-command-on-windows"></a>Ejecución del comando en Windows

> [!NOTE]
> Debe usar comillas dobles para los nombres de rol cuando se ejecuta desde un entorno de Windows.

```azurecli
az role assignment create --assignee <appId> --role "Monitoring Metrics Publisher" --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role "Contributor" --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-on-linux-or-macos"></a>Ejecución del comando en Linux o macOS

```azurecli
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-in-powershell"></a>Ejecución del comando en PowerShell

```powershell
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

```output
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

Con la entidad de servicio asignada al rol adecuado y las variables de entorno establecidas, puede continuar con la creación del controlador de datos. 

## <a name="create-the-azure-arc-data-controller"></a>Creación del controlador de datos de Azure Arc

> [!NOTE]
> Puede usar un valor diferente para el parámetro `--namespace` del comando azdata arc dc create en los ejemplos siguientes, pero asegúrese de usar ese nombre de espacio de nombres en el parámetro `--namespace parameter` de los demás comandos siguientes.

- [Creación en Azure Kubernetes Service (AKS)](#create-on-azure-kubernetes-service-aks)
- [Creación en el motor de AKS en Azure Stack Hub](#create-on-aks-engine-on-azure-stack-hub)
- [Creación en AKS en Azure Stack HCI](#create-on-aks-on-azure-stack-hci)
- [Creación en Red Hat OpenShift en Azure (ARO)](#create-on-azure-red-hat-openshift-aro)
- [Creación en Red Hat OpenShift Container Platform (OCP)](#create-on-red-hat-openshift-container-platform-ocp)
- [Creación en Kubernetes ascendente de código abierto (kubeadm)](#create-on-open-source-upstream-kubernetes-kubeadm)
- [Creación en Elastic Kubernetes Service (EKS) de AWS](#create-on-aws-elastic-kubernetes-service-eks)
- [Creación en Kubernetes Engine Service (GKE) de Google Cloud](#create-on-google-cloud-kubernetes-engine-service-gke)

### <a name="create-on-azure-kubernetes-service-aks"></a>Creación en Azure Kubernetes Service (AKS)

De forma predeterminada, el perfil de implementación de AKS usa la clase de almacenamiento `managed-premium`. La clase de almacenamiento `managed-premium` solo funcionará si tiene máquinas virtuales que se implementaron mediante imágenes de máquina virtual con discos premium.

Si va a usar `managed-premium` como clase de almacenamiento, puede ejecutar el siguiente comando para implementar el controlador de datos. Sustituya los marcadores de posición del comando por el nombre del grupo de recursos, el identificador de la suscripción y la ubicación de Azure.

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Si no está seguro de qué clase de almacenamiento debe utilizar, indique la clase de almacenamiento `default`, que es compatible con cualquier tipo de máquina virtual que esté usando. No le ofrecerá el rendimiento más rápido.

Si desea usar la clase de almacenamiento `default`, puede ejecutar este comando:

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-aks-engine-on-azure-stack-hub"></a>Creación en el motor de AKS en Azure Stack Hub

De forma predeterminada, el perfil de implementación utiliza la clase de almacenamiento `managed-premium`. La clase de almacenamiento `managed-premium` solo funcionará si tiene máquinas virtuales de trabajo que se implementaron mediante imágenes de máquina virtual con discos premium en Azure Stack Hub.

Puede ejecutar el siguiente comando para implementar el controlador de datos con la clase de almacenamiento managed-premium:

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Si no está seguro de qué clase de almacenamiento debe utilizar, indique la clase de almacenamiento `default`, que es compatible con cualquier tipo de máquina virtual que esté usando. En Azure Stack Hub, los discos premium y los discos estándar cuentan con el respaldo de la misma infraestructura de almacenamiento. Por lo tanto, se espera que proporcionen el mismo rendimiento general, pero con distintos límites de IOPS.

Si desea usar la clase de almacenamiento `default`, puede ejecutar este comando.

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-aks-on-azure-stack-hci"></a>Creación en AKS en Azure Stack HCI

De forma predeterminada, el perfil de implementación utiliza una clase de almacenamiento denominada `default` y el tipo de servicio `LoadBalancer`.

Puede ejecutar el siguiente comando para implementar el controlador de datos con la clase de almacenamiento `default` y el tipo de servicio `LoadBalancer`.

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).


### <a name="create-on-azure-red-hat-openshift-aro"></a>Creación en Red Hat OpenShift en Azure (ARO)

Red Hat OpenShift en Azure requiere una restricción del contexto de seguridad.

#### <a name="apply-the-security-context"></a>Aplicación del contexto de seguridad

[!INCLUDE [apply-security-context-constraint](includes/apply-security-context-constraint.md)]

#### <a name="create-custom-deployment-profile"></a>Creación de un perfil de implementación personalizado

Use el perfil `azure-arc-azure-openshift` para Azure RedHat Open Shift.

```console
azdata arc dc config init --source azure-arc-azure-openshift --path ./custom
```

#### <a name="create-data-controller"></a>Creación de un controlador de datos

Puede ejecutar el comando siguiente para crear el controlador de datos:

> [!NOTE]
> Use el mismo espacio de nombres aquí que en los comandos `oc adm policy add-scc-to-user` anteriores. Un ejemplo es `arc`.

```console
azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example
#azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-red-hat-openshift-container-platform-ocp"></a>Creación en Red Hat OpenShift Container Platform (OCP)

> [!NOTE]
> Si usa Red Hat OpenShift Container Platform en Azure, se recomienda usar la versión más reciente disponible.

Antes de crear el controlador de datos en Red Hat OCP, deberá aplicar restricciones de contexto de seguridad específicas. 

#### <a name="apply-the-security-context-constraint"></a>Aplicación de la restricción de contexto de seguridad

[!INCLUDE [apply-security-context-constraint](includes/apply-security-context-constraint.md)]

#### <a name="determine-storage-class"></a>Determinación de la clase de almacenamiento

También necesitará determinar qué clase de almacenamiento se va a usar ejecutando el siguiente comando.

```console
kubectl get storageclass
```

#### <a name="create-custom-deployment-profile"></a>Creación de un perfil de implementación personalizado

Cree un nuevo archivo de perfil de implementación personalizado basado en el perfil de implementación `azure-arc-openshift` ejecutando el siguiente comando. Este comando crea un directorio `custom` en el directorio de trabajo actual y un archivo de perfil de implementación personalizado `control.json` en ese directorio.

Use el perfil `azure-arc-openshift` para OpenShift Container Platform.

```console
azdata arc dc config init --source azure-arc-openshift --path ./custom
```

#### <a name="set-storage-class"></a>Establecimiento de la clase de almacenamiento 

Ahora, establezca la clase de almacenamiento deseada reemplazando `<storageclassname>` en el siguiente comando por el nombre de la clase de almacenamiento que desea usar y que se determinó mediante la ejecución del comando `kubectl get storageclass` anterior.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

#### <a name="set-loadbalancer-optional"></a>Establecimiento de LoadBalancer (opcional)

De forma predeterminada, el perfil de implementación `azure-arc-openshift` usa `NodePort` como el tipo de servicio. Si usa un clúster de OpenShift que está integrado con un equilibrador de carga, puede cambiar la configuración para usar el tipo de servicio `LoadBalancer` con el siguiente comando:

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

#### <a name="verify-security-policies"></a>Directivas de comprobación de seguridad

Cuando usa OpenShift, es posible que desee realizar la ejecución con las directivas de seguridad predeterminadas de OpenShift o que desee bloquear el entorno más de lo habitual. Si lo desea, puede optar por deshabilitar, mediante el uso de los siguientes comandos, algunas características para reducir los permisos que se necesitan al implementar o al ejecutar.

Este comando deshabilita la recopilación de métricas sobre los pods. Si deshabilita esta característica, no podrá ver las métricas sobre los pods en los paneles de Grafana. El valor predeterminado es true.

```console
azdata arc dc config replace -p ./custom/control.json --json-values spec.security.allowPodMetricsCollection=false
```

Este comando deshabilita la recopilación de métricas sobre los nodos. Si deshabilita esta característica, no podrá ver las métricas sobre los nodos en los paneles de Grafana. El valor predeterminado es true.

```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowNodeMetricsCollection=false
```

Este comando deshabilita la posibilidad de realizar volcados de memoria para solucionar problemas.
```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowDumps=false
```

#### <a name="create-data-controller"></a>Creación de un controlador de datos

Ahora ya puede crear el controlador de datos con el siguiente comando.

> [!NOTE]
>   Use el mismo espacio de nombres aquí que en los comandos `oc adm policy add-scc-to-user` anteriores. Un ejemplo es `arc`.

> [!NOTE]
>   El parámetro `--path` debe apuntar al _directorio_ que contiene el archivo control.json y no al propio archivo control.json.


```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-open-source-upstream-kubernetes-kubeadm"></a>Creación en Kubernetes ascendente de código abierto (kubeadm)

De forma predeterminada, el perfil de implementación de kubeadm usa una clase de almacenamiento denominada `local-storage` y el tipo de servicio `NodePort`. Si le resulta aceptable, puede omitir las instrucciones que se indican a continuación para establecer la clase de almacenamiento y el tipo de servicio que desee y ejecutar inmediatamente el comando `azdata arc dc create` siguiente.

Si desea personalizar el perfil de implementación para especificar una clase de almacenamiento o un tipo de servicio específicos, empiece por crear un nuevo archivo de perfil de implementación personalizado basado en el perfil de implementación de kubeadm. Para ello, ejecute el siguiente comando. Este comando creará un directorio `custom` en el directorio de trabajo actual y un archivo de perfil de implementación personalizado `control.json` en ese directorio.

```console
azdata arc dc config init --source azure-arc-kubeadm --path ./custom
```

Puede buscar las clases de almacenamiento disponibles mediante la ejecución del siguiente comando.

```console
kubectl get storageclass
```

Ahora, establezca la clase de almacenamiento deseada reemplazando `<storageclassname>` en el siguiente comando por el nombre de la clase de almacenamiento que desea usar y que se determinó mediante la ejecución del comando `kubectl get storageclass` anterior.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

De forma predeterminada, el perfil de implementación de kubeadm usa `NodePort` como tipo de servicio. Si usa un clúster de Kubernetes que está integrado con un equilibrador de carga, puede cambiar la configuración con el siguiente comando.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

Ahora ya puede crear el controlador de datos con el siguiente comando.

```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-aws-elastic-kubernetes-service-eks"></a>Creación en Elastic Kubernetes Service (EKS) de AWS

De forma predeterminada, la clase de almacenamiento de EKS es `gp2` y el tipo de servicio es `LoadBalancer`.

Ejecute el siguiente comando para crear el controlador de datos mediante el perfil de implementación de EKS proporcionado.

```console
azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

### <a name="create-on-google-cloud-kubernetes-engine-service-gke"></a>Creación en Kubernetes Engine Service (GKE) de Google Cloud

De forma predeterminada, la clase de almacenamiento de GKE es `standard` y el tipo de servicio es `LoadBalancer`.

Ejecute el siguiente comando para crear el controlador de datos mediante el perfil de implementación de GKE proporcionado.

```console
azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Una vez ejecutado el comando, continúe en [Supervisión del estado de creación](#monitoring-the-creation-status).

## <a name="monitoring-the-creation-status"></a>Supervisión del estado de creación

La creación del controlador tardará unos minutos en completarse. Puede supervisar el progreso en otra ventana de terminal con los siguientes comandos:

> [!NOTE]
>  En los siguientes comandos de ejemplo se da por hecho que ha creado un controlador de datos y un espacio de nombres de Kubernetes con el nombre `arc`. Si ha usado otro nombre de controlador de datos o de espacio de nombres, puede reemplazar `arc` por ese nombre.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

También puede comprobar el estado de creación de un pod determinado ejecutando un comando como el siguiente. Esto es especialmente útil para solucionar problemas.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Solución de problemas de creación

Si tiene problemas con la creación, consulte la [guía de solución de problemas](troubleshoot-guide.md).