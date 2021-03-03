---
title: Preguntas más frecuentes sobre Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 02/19/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Este artículo contiene una lista de las preguntas más frecuentes relacionadas con Kubernetes habilitado para Azure Arc.
keywords: Kubernetes, Arc, Azure, contenedores, configuración, GitOps, preguntas más frecuentes
ms.openlocfilehash: dc12294b5d53372be5f2e1dd71436973fefbb194
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101647870"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Preguntas más frecuentes: Kubernetes habilitado para Azure Arc

En este artículo se abordan las preguntas más frecuentes sobre Kubernetes habilitado para Azure Arc.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>¿Cuál es la diferencia entre Kubernetes habilitado para Azure Arc y Azure Kubernetes Service (AKS)?

AKS es la oferta de Kubernetes administrado de Azure. AKS facilita la implementación de un clúster de Kubernetes administrado en Azure al traspasar gran parte de la complejidad y sobrecarga operativa a Azure. Como Azure administra los maestros de Kubernetes, solo administra y mantiene los nodos de agente.

Kubernetes habilitado para Azure Arc permite ampliar las funcionalidades de administración de Azure (como Azure Monitor y Azure Policy) al conectar clústeres de Kubernetes a Azure. Usted mantiene el propio clúster de Kubernetes subyacente.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>¿Es necesario conectar a Azure Arc los clústeres de AKS que se ejecutan en Azure?

No. Todas las características de Kubernetes habilitado para Azure Arc, incluido Azure Monitor y Azure Policy (Gatekeeper), están disponibles en AKS (un recurso nativo en Azure Resource Manager).
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>¿Debo conectar el clúster AKS-HCI y los clústeres de Kubernetes en Azure Stack Hub y Azure Stack Edge a Azure Arc?

Sí, la conexión del clúster AKS-HCI o los clústeres de Kubernetes en Azure Stack Edge o Azure Stack Hub a Azure Arc proporciona clústeres con representación de recursos en Azure Resource Manager. Con esta representación de recursos, se amplían algunas funcionalidades, como la configuración de clúster, Azure Monitor y Azure Policy (Gatekeeper), a los clústeres de Kubernetes conectados.

Si el clúster de Kubernetes habilitado para Azure Arc está en Azure Stack Edge, AKS en Azure Stack HCI (>= actualización de abril de 2021) o AKS en Windows Server 2019 Datacenter (>= actualización de abril de 2021), la configuración de Kubernetes se incluye sin cargo alguno.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>¿Cómo tratar los recursos caducados de Kubernetes habilitado para Azure Arc?

El certificado de Managed Service Identity (MSI) asociado a Kubernetes habilitado para Azure Arc tiene un período de expiración de 90 días. Una vez que este certificado expira, el recurso se considera `Expired` y todas las características, (como la configuración, la supervisión y la directiva), dejan de funcionar en este clúster. Para que el clúster de Kubernetes vuelva a funcionar con Azure Arc:

1. Elimine el recurso y los agentes de Kubernetes habilitado para Azure Arc en el clúster. 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Para volver a crear el recurso de Kubernetes habilitado para Azure Arc, implemente los agentes en el clúster.
    
    ```console
    az connectedk8s connect -n <name> -g <resource-group>
    ```

> [!NOTE]
> `az connectedk8s delete` también eliminará las configuraciones, además del clúster. Después de ejecutar `az connectedk8s connect`, vuelva a crear las configuraciones en el clúster, ya sea manualmente o mediante Azure Policy.

## <a name="if-i-am-already-using-cicd-pipelines-can-i-still-use-azure-arc-enabled-kubernetes-and-configurations"></a>Si ya uso canalizaciones de CI/CD, ¿puedo seguir usando configuraciones y Kubernetes habilitado para Azure Arc?

Sí, todavía puede usar configuraciones en un clúster que recibe implementaciones a través de una canalización de CI/CD. En comparación con las canalizaciones de CI/CD tradicionales, las configuraciones presentan dos ventajas adicionales:

**Conciliación de desfase**

La canalización de CI/CD aplica los cambios solo una vez durante la ejecución de la canalización. Sin embargo, el operador de GitOps del clúster sondea continuamente el repositorio de Git para capturar el estado deseado de los recursos de Kubernetes en el clúster. Si el operador de GitOps encuentra que el estado deseado de los recursos es diferente del estado real de los recursos en el clúster, se reconcilia este desfase.

**Aplicación de GitOps a escala**

Las canalizaciones de CI/CD son útiles para implementaciones controladas por eventos en el clúster de Kubernetes (por ejemplo, una inserción en un repositorio de Git). Sin embargo, si desea implementar la misma configuración en todos los clústeres de Kubernetes, debe configurar manualmente las credenciales de cada clúster de Kubernetes en la canalización de CI/CD. 

En el caso de Kubernetes habilitado para Azure Arc, como Azure Resource Manager administra las configuraciones, puede automatizar la creación de la misma configuración en todos los recursos de Kubernetes habilitado para Azure Arc mediante Azure Policy, en el ámbito de una suscripción o de un grupo de recursos. Esta funcionalidad es incluso aplicable a los recursos de Kubernetes habilitado para Azure Arc creados después de la asignación de directivas.

La característica aplica configuraciones de base de referencia (como directivas de red, enlaces de roles y directivas de seguridad de pods) en todo el inventario de clústeres de Kubernetes para satisfacer los requisitos de cumplimiento y gobernanza.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster a Azure Arc](./quickstart-connect-cluster.md)
* [Creación de configuraciones en el clúster de Kubernetes habilitado para Arc](./use-gitops-connected-cluster.md)
* [Uso de Azure Policy para aplicar configuraciones a escala](./use-azure-policy.md)
