---
title: Uso de Azure Policy para aplicar configuraciones de clúster a escala
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Uso de Azure Policy para aplicar configuraciones de clúster a escala
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.openlocfilehash: 7f85050666c383ba49730bd88ce1f26d55607e7a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101652154"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale"></a>Uso de Azure Policy para aplicar configuraciones de clúster a escala

## <a name="overview"></a>Información general

Azure Policy se puede usar para aplicar configuraciones ( tipo de recurso de `Microsoft.KubernetesConfiguration/sourceControlConfigurations`) a escala en clústeres Kubernetes habilitados para Azure Arc (`Microsoft.Kubernetes/connectedclusters`).

Para usar Azure Policy, seleccione una definición de directiva existente y cree una asignación de directiva. Al crear la asignación de directiva:
1. Establezca el ámbito de la asignación.
    * El ámbito será un grupo de recursos o una suscripción de Azure. 
2. Establezca los parámetros de la configuración que se va a crear. 

Una vez que se crea la asignación, el motor de Azure Policy identifica todos los clústeres de Kubernetes habilitado para Azure Arc que se encuentran dentro del ámbito y aplica la configuración a todos ellos.

Si usa, varias asignaciones de directiva, puede crear varias configuraciones y que cada una de ellas apunte a un repositorio de Git diferente. Por ejemplo, un repositorio para el operador de clúster/TI central y otros para los equipos de la aplicación.

## <a name="prerequisite"></a>Requisito previo

Compruebe que tiene permisos de `Microsoft.Authorization/policyAssignments/write` en el ámbito (suscripción o grupo de recursos) en el que va a crear esta asignación de directiva.

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

Después de crear la asignación de directiva, la configuración se aplica a los nuevos clústeres de Kubernetes habilitado para Azure Arc creados en el ámbito de la asignación de directiva.

En los clústeres existentes, será preciso ejecutar manualmente una tarea de corrección. Normalmente, esta tarea de asignación de directiva tarda entre 10 y 20 minutos en surtir efecto.

## <a name="verify-a-policy-assignment"></a>Comprobación de una asignación de directiva

1. En el Azure Portal, vaya a uno de los clústeres de Kubernetes habilitado para Azure Arc.
1. En la sección **Configuración** de la barra lateral, seleccione **Directivas**. 
    * En la lista de directivas, debería ver la asignación de directiva que ha creado antes con **Estado de cumplimiento** establecido en *Conforme*.
1. En la sección **Configuración** de la barra lateral, seleccione **Configuraciones**.
    * En la lista de configuraciones, debería ver la configuración creada por la asignación de directiva.
1. Use `kubectl` para interrogar al clúster. 
    * Debería ver el espacio de nombres y los artefactos creados por los recursos de las configuraciones.
    * En un plazo de 5 minutos (suponiendo que el clúster tenga conectividad de red a Azure), debería ver que los objetos descritos por los manifiestos en el repositorio de Git, se crean en el clúster.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Azure Monitor para contenedores con clústeres de Kubernetes habilitados para Arc](../../azure-monitor/containers/container-insights-enable-arc-enabled-clusters.md)
