---
title: Administración del agente de Container Insights | Microsoft Docs
description: En este artículo se describe cómo administrar las tareas de mantenimiento más comunes con el agente de Log Analytics en contenedores que usa Container Insights.
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: 2a0c32ef797a953eca794e16fe0ace5e967f339f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101713803"
---
# <a name="how-to-manage-the-container-insights-agent"></a>Administración del agente de Container Insights

Container Insights usa una versión contenedorizada del agente de Log Analytics para Linux. Después de la implementación inicial, hay tareas rutinarias u opcionales que debe hacer durante su ciclo de vida. En este artículo se detallan los procedimientos para actualizar el agente manualmente y deshabilitar la recopilación de variables de entorno desde un contenedor determinado. 

## <a name="how-to-upgrade-the-container-insights-agent"></a>Actualización del agente de Container Insights

Container Insights usa una versión contenedorizada del agente de Log Analytics para Linux. Cuando se publica una nueva versión del agente, este se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS) y Red Hat OpenShift en Azure versión 3.x. En el caso de un [clúster de Kubernetes híbrido](container-insights-hybrid-setup.md) y Red Hat OpenShift en Azure versión 4.x, el agente no está administrado y debe actualizarlo manualmente.

Si se produce un error en la actualización del agente de un clúster hospedado en AKS o Red Hat OpenShift en Azure versión 3.x, en este artículo también se describe el proceso para actualizar el agente de forma manual. Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-aks-cluster"></a>Actualización del agente en el clúster de AKS

El proceso para actualizar al agente en clústeres de AKS consta de dos pasos sencillos. El primer paso es deshabilitar la supervisión con Container Insights mediante la CLI de Azure. Siga los pasos descritos en el artículo [Deshabilitar la supervisión](container-insights-optout.md?#azure-cli). El uso de la CLI de Azure nos permite quitar el agente de los nodos del clúster sin afectar a la solución ni a los datos correspondientes que se almacenan en el área de trabajo. 

>[!NOTE]
>Mientras está realizando esta actividad de mantenimiento, los nodos del clúster no reenviarán los datos recopilados y las vistas de rendimiento no mostrarán datos en el período comprendido entre la eliminación del agente y la instalación de la nueva versión. 
>

Para instalar la nueva versión del agente, siga los pasos descritos en [Habilitar de la supervisión con la CLI de Azure](container-insights-enable-new-cluster.md#enable-using-azure-cli) para completar este proceso.  

Después de volver a habilitar la supervisión, es posible que pasen unos 15 minutos hasta que pueda ver actualizada la métrica de estado del clúster. Para comprobar que el agente se ha actualizado correctamente, puede:

* Ejecutar el comando: `kubectl get pod <omsagent-pod-name> -n kube-system -o=jsonpath='{.spec.containers[0].image}'`. En el estado devuelto, observe el valor de **Imagen** para omsagent en la sección *Contenedores* de la salida.
* En la pestaña **Nodos**, seleccione el nodo de clúster y, en el panel **Propiedades** de la derecha, anote el valor de **Etiqueta de imagen del agente**.

La versión del agente que se muestra debe coincidir con la versión más reciente de la página [Historial de versiones](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Actualización de agente en un clúster de Kubernetes híbrido

Siga los pasos que se describen a continuación para actualizar el agente en un clúster de Kubernetes que se ejecute en:

* clústeres de Kubernetes autoadministrados que se hospedan en Azure con el motor de AKS;
* clústeres de Kubernetes autoadministrados que se hospedan en Azure Stack o en el entorno local con el motor de AKS;
* Red Hat OpenShift versión 4.x.

Si el área de trabajo de Log Analytics está en una instancia comercial de Azure, ejecute el siguiente comando:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Si el área de trabajo de Log Analytics está en Azure China 21Vianet, ejecute el siguiente comando:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Si el área de trabajo de Log Analytics está en Azure Gobierno de EE. UU, ejecute el siguiente comando:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-red-hat-openshift-v4"></a>Actualización del agente en Red Hat OpenShift en Azure versión 4

Siga los pasos que se describen a continuación para actualizar el agente en un clúster de Kubernetes que se ejecuta en Red Hat OpenShift en Azure versión 4. 

>[!NOTE]
>Red Hat OpenShift en Azure versión 4 solo admite la ejecución en la nube comercial de Azure.
>

```console
curl -o upgrade-monitoring.sh -L https://aka.ms/upgrade-monitoring-bash-script
export azureAroV4ClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>"
bash upgrade-monitoring.sh --resource-id $ azureAroV4ClusterResourceId
```

Vea **Uso de una entidad de servicio** en [Habilitación de la supervisión en el clúster de Kubernetes habilitado para Azure Arc](container-insights-enable-arc-enabled-clusters.md#enable-using-bash-script) para obtener detalles sobre el uso de una entidad de servicio con este comando.

### <a name="upgrade-agent-on-azure-arc-enabled-kubernetes"></a>Agente de actualización en Kubernetes habilitado para Azure Arc

Ejecute el siguiente comando para actualizar el agente en un clúster de Kubernetes habilitado para Azure Arc.

```console
curl -o upgrade-monitoring.sh -L https://aka.ms/upgrade-monitoring-bash-script
export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
bash upgrade-monitoring.sh --resource-id $azureArcClusterResourceId
```

Vea **Uso de una entidad de servicio** en [Habilitación de la supervisión en el clúster de Kubernetes habilitado para Azure Arc](container-insights-enable-arc-enabled-clusters.md#enable-using-bash-script) para obtener detalles sobre el uso de una entidad de servicio con este comando.


## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Cómo deshabilitar la recopilación de variables de entorno en un contenedor

Container Insights recopila las variables de entorno de los contenedores que se ejecutan en un pod y las presenta en el panel de propiedades del contenedor seleccionado en la vista **Contenedores**. Puede controlar este comportamiento si deshabilita la recopilación de un contenedor específico, ya sea durante la implementación del clúster de Kubernetes, o después, al establecer la variable de entorno *AZMON_COLLECT_ENV*. Esta característica está disponible en la versión del agente: ciprod11292018 y posterior.  

Para deshabilitar la recopilación de variables de entorno en un contenedor nuevo o existente, establezca la variable **AZMON_COLLECT_ENV** con un valor de **False** en el archivo de configuración yaml de implementación de Kubernetes. 

```yaml
- name: AZMON_COLLECT_ENV  
  value: "False"  
```

Ejecute el comando siguiente para aplicar el cambio a los clústeres de Kubernetes que no sean de Red Hat OpenShift en Azure): `kubectl apply -f  <path to yaml file>`. Para editar ConfigMap y aplicar este cambio a los clústeres de Red Hat OpenShift en Azure, ejecute el comando:

```bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Se abrirá el editor de texto predeterminado. Después de establecer la variable, guarde el archivo en el editor.

Para verificar que el cambio de configuración surta efecto, seleccione un contenedor en la vista **Contenedores** en Container Insights y, en el panel de propiedades, expanda **Variables de entorno**.  La sección debe mostrar solo la variable que creó anteriormente: **AZMON_COLLECT_EN=FALSE**. Para todos los otros contenedores, la sección Variables de entorno debe enumerar todas las variables de entorno detectadas.

Para volver a habilitar la detección de las variables de entorno, aplique el mismo proceso anterior y cambie el valor de **False** a **True** y, luego, vuelva a ejecutar el comando `kubectl` para actualizar el contenedor.  

```yaml
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Pasos siguientes

Si experimenta problemas al actualizar el agente, consulte la [guía de solución de problemas](container-insights-troubleshoot.md) para obtener ayuda.
