---
title: Preguntas más frecuentes sobre Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Este artículo contiene una lista de las preguntas más frecuentes relacionadas con Kubernetes habilitado para Azure Arc.
keywords: Kubernetes, Arc, Azure, contenedores, configuración, GitOps, preguntas más frecuentes
ms.openlocfilehash: 237b2629b833a63552b172636f46a1ac92e321c0
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561250"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Preguntas más frecuentes: Kubernetes habilitado para Azure Arc

En este artículo se abordan las preguntas más frecuentes sobre Kubernetes habilitado para Azure Arc.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>¿Cuál es la diferencia entre Kubernetes habilitado para Azure Arc y Azure Kubernetes Service (AKS)?

AKS es la oferta de Kubernetes administrado de Azure. AKS facilita la implementación de un clúster de Kubernetes administrado en Azure al traspasar gran parte de la complejidad y sobrecarga operativa a Azure. Como Azure administra los maestros de Kubernetes, solo administra y mantiene los nodos de agente.

Kubernetes habilitado para Azure Arc permite ampliar las funcionalidades de administración de Azure (como Azure Monitor y Azure Policy) al conectar clústeres de Kubernetes a Azure. Usted mantiene el propio clúster de Kubernetes subyacente.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>¿Es necesario conectar a Azure Arc los clústeres de AKS que se ejecutan en Azure?

No. Todas las características de Kubernetes habilitado para Azure Arc, incluido Azure Monitor y Azure Policy (Gatekeeper), están disponibles en AKS (un recurso nativo en Azure Resource Manager).
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>¿Debo conectar el clúster AKS-HCI y los clústeres de Kubernetes en Azure Stack Hub y Azure Stack Edge a Azure Arc?

Sí, la conexión del clúster AKS-HCI o los clústeres de Kubernetes en Azure Stack Edge o Azure Stack Hub a Azure Arc proporciona clústeres con representación de recursos en Azure Resource Manager. Con esta representación de recursos, se amplían algunas funcionalidades, como la configuración de clúster, Azure Monitor y Azure Policy (Gatekeeper), a los clústeres de Kubernetes a los que se conecte.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>¿Cómo tratar los recursos caducados de Kubernetes habilitado para Azure Arc?

El certificado de Managed Service Identity (MSI) asociado a su Kuberenetes habilitado para Azure Arc tiene una ventana de expiración de 90 días. Una vez que este certificado expira, el recurso se considera `Expired`, y todas las características, como la configuración, la supervisión y la directiva, dejan de funcionar en este clúster. Siga estos pasos para que el clúster de Kubernetes vuelva a funcionar con Azure Arc:

1. Eliminación del recurso y los agentes de Kubernetes habilitado para Azure Arc en el clúster 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Para volver a crear el recurso de Kubernetes habilitado para Azure Arc, vuelva a implementar los agentes en el clúster.
    
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

Las canalizaciones de CI/CD son adecuadas para implementaciones controladas por eventos en el clúster de Kubernetes, donde el evento podría ser una inserción en un repositorio de Git. Sin embargo, la implementación de la misma configuración en todos los clústeres de Kubernetes requiere que la canalización de CI/CD se configure manualmente con las credenciales de cada uno de estos clústeres de Kubernetes. Por otro lado, en el caso de Kubernetes habilitado para Azure Arc, dado que Azure Resource Manager administra las configuraciones, puede usar Azure Policy para automatizar la aplicación de la configuración deseada en todos los clústeres de Kubernetes en un ámbito de suscripción o grupo de recursos de una sola vez. Esta funcionalidad es incluso aplicable a los recursos de Kubernetes habilitado para Azure Arc creados después de la asignación de directivas.

La característica de configuraciones se usa para aplicar configuraciones de línea de base, como directivas de red, enlaces de roles y directivas de seguridad de pods, en todo el inventario de clústeres de Kubernetes para los requisitos de cumplimiento y gobernanza.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster a Azure Arc](./connect-cluster.md)
* [Creación de configuraciones en el clúster de Kubernetes habilitado para Arc](./use-gitops-connected-cluster.md)
* [Uso de Azure Policy para aplicar configuraciones a escala](./use-azure-policy.md)
