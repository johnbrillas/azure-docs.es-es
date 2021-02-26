---
title: Uso de Azure Policy para aplicar configuraciones de clúster a escala (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Uso de Azure Policy para aplicar configuraciones de clúster a escala
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.openlocfilehash: b80e50cb4823632f054de3b7f9da71392f8578d7
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560184"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Uso de Azure Policy para aplicar configuraciones de clúster a escala (versión preliminar)

## <a name="overview"></a>Información general

Puede usar Azure Policy para exigir cualquiera de los siguientes recursos para que se aplique `Microsoft.KubernetesConfiguration/sourceControlConfigurations` de forma específica:
*  Recurso `Microsoft.Kubernetes/connectedclusters`.
* Recurso `Microsoft.ContainerService/managedClusters` habilitado para GitOps. 

Para usar Azure Policy, seleccione una definición de directiva existente y cree una asignación de directiva. Al crear la asignación de directiva:
1. Establezca el ámbito de la asignación.
    * El ámbito será un grupo de recursos o una suscripción de Azure. 
2. Establezca los parámetros para el elemento `sourceControlConfiguration` que se creará. 

Una vez que se haya creado la asignación, el motor de Azure Policy identificará todos los recursos `connectedCluster` o `managedCluster` que se encuentren dentro del ámbito y aplicará `sourceControlConfiguration` a cada uno de ellos.

Puede habilitar varios repositorios de Git como orígenes verdaderos para cada clúster mediante el uso de varias asignaciones de directivas. Cada asignación de directiva se configuraría para utilizar un repositorio de Git diferente; por ejemplo, un repositorio para el operador central de TI/clústeres y otros repositorios para los equipos de aplicaciones.

## <a name="prerequisite"></a>Requisito previo

Compruebe tener permisos de `Microsoft.Authorization/policyAssignments/write` en el ámbito (suscripción o grupo de recursos) en el que quiere crear esta asignación de directiva.

## <a name="create-a-policy-assignment"></a>Creación de una asignación de directiva

1. En Azure Portal, vaya a **Directiva**.
1. En la sección **Creación** de la barra lateral, seleccione **Definiciones**.
1. En la categoría "Kubernetes", elija la directiva integrada "Implementar GitOps en el clúster de Kubernetes". 
1. Haga clic en **Asignar**.
1. Establezca el **Ámbito** en el grupo de administración, la suscripción o el grupo de recursos al que se aplicará la asignación de directiva.
    * Si quiere excluir recursos del ámbito de la directiva, establezca **Exclusiones**.
1. Otorgue a la asignación de directiva un **Nombre** y una **Descripción** fáciles de identificar.
1. Asegúrese de que **Cumplimiento de directivas** esté establecido en **Habilitado**.
1. Seleccione **Next** (Siguiente).
1. Establezca los valores de parámetro que se van a usar al crear `sourceControlConfiguration`.
1. Seleccione **Next** (Siguiente).
1. Habilite **Crear una tarea de corrección**.
1. Compruebe que **Crear una identidad administrada** esté activada y de que la identidad tendrá permisos de **Colaborador**. 
    * Para obtener más información, consulte la guía de [Inicio rápido: Creación de una asignación de directiva](../../governance/policy/assign-policy-portal.md) y el [artículo Corrección de recursos no compatibles con Azure Policy](../../governance/policy/how-to/remediate-resources.md).
1. Seleccione **Revisar + crear**.

Después de crear la asignación de directiva, se aplicará `sourceControlConfiguration` para cualquiera de los siguientes recursos ubicados en el ámbito de la asignación:
* Nuevos recursos `connectedCluster`.
* Nuevos recursos `managedCluster` con los agentes de GitOps instalados. 

Para los clústeres existentes, tendrá que ejecutar manualmente una tarea de corrección. Normalmente, esta tarea de asignación de directiva tarda entre 10 y 20 minutos en surtir efecto.

## <a name="verify-a-policy-assignment"></a>Comprobación de una asignación de directiva

1. En Azure Portal, vaya a uno de los recursos `connectedCluster`.
1. En la sección **Configuración** de la barra lateral, seleccione **Directivas**. 
    * La experiencia de usuario del clúster de AKS todavía no se ha implementado.
    * En la lista de directivas, debería ver la asignación de directiva que ha creado antes con **Estado de cumplimiento** establecido en *Conforme*.
1. En la sección **Configuración** de la barra lateral, seleccione **Configuraciones**.
    * En la lista de configuraciones, debería ver el elemento `sourceControlConfiguration` creado por la asignación de directiva.
1. Use `kubectl` para interrogar al clúster. 
    * Debería ver el espacio de nombres y los artefactos creados por `sourceControlConfiguration`.
    * En cinco minutos, debería ver en el clúster los artefactos que se describen en los manifiestos del repositorio de Git configurado.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Azure Monitor para contenedores con clústeres de Kubernetes habilitados para Arc](../../azure-monitor/insights/container-insights-enable-arc-enabled-clusters.md)
