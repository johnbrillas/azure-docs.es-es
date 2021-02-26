---
title: 'Configuraciones y GitOps: Kubernetes habilitado para Azure Arc'
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: En este artículo se proporciona información general conceptual de la funcionalidad de GitOps y configuraciones de Kubernetes habilitado para Azure Arc.
keywords: Kubernetes, Arc, Azure, contenedores, configuración, GitOps
ms.openlocfilehash: 780c3c5c578c8a9b12eb7dda711070790477ac5f
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561232"
---
# <a name="configurations-and-gitops-with-azure-arc-enabled-kubernetes"></a>Configuraciones y GitOps con Kubernetes habilitado para Azure Arc

En relación con Kubernetes, GitOps es la práctica de declarar el estado deseado de las configuraciones de los clústeres de Kubernetes (implementaciones, espacios de nombres, etc.) en un repositorio de Git. A esta declaración le sigue una implementación de sondeo basada en extracción de estas configuraciones de clúster mediante un operador. El repositorio de Git puede contener:
* Manifiestos con formato YAML que describen cualquier recurso de Kubernetes válido, incluidos espacios de nombres, ConfigMaps, implementaciones, DaemonSets, etc.
* Gráficos de Helm para implementar aplicaciones.

[Flux](https://docs.fluxcd.io/), una popular herramienta de código abierto en el espacio de GitOps, se puede implementar en el clúster de Kubernetes para facilitar el flujo de configuraciones de un repositorio de Git a un clúster de Kubernetes. Flux admite la implementación de su operador tanto en el ámbito del clúster como del espacio de nombres. Un operador de Flux implementado con el ámbito del espacio de nombres solo puede implementar objetos de Kubernetes dentro de ese espacio de nombres específico. La capacidad de elegir entre el ámbito del clúster o del espacio de nombres ayuda a lograr patrones de implementación multiinquilino en el mismo clúster de Kubernetes.

## <a name="configurations"></a>Configurations

[ ![Arquitectura de configuración](./media/conceptual-configurations.png) ](./media/conceptual-configurations.png#lightbox)

La conexión entre el clúster y un repositorio de Git se crea como recurso de extensión `Microsoft.KubernetesConfiguration/sourceControlConfigurations` además del recurso de Kubernetes habilitado para Azure Arc (representado por `Microsoft.Kubernetes/connectedClusters`) en Azure Resource Manager. 

Las propiedades del recurso `sourceControlConfiguration` se usan para implementar el operador de Flux en el clúster con los parámetros adecuados, como el repositorio de Git desde el que se extraerán los manifiestos y el intervalo de sondeo en el que se van a extraer. Los datos de `sourceControlConfiguration` se almacenan cifrados en reposo en una base de datos de Azure Cosmos DB para garantizar su confidencialidad.

El `config-agent` en ejecución en el clúster es responsable de:
* El seguimiento de los recursos de extensión `sourceControlConfiguration` nuevos o actualizados en el recurso de Kubernetes habilitado para Azure Arc.
* La implementación de un operador de Flux para inspeccionar el repositorio de Git para cada `sourceControlConfiguration`.
* La aplicación de las actualizaciones realizadas en cualquier `sourceControlConfiguration`. 

Puede crear varios recursos de `sourceControlConfiguration` limitados al espacio de nombres en el mismo clúster de Kubernetes habilitado para Azure Arc para lograr la función de multiinquilino.

> [!NOTE]
> * Dado que `config-agent` supervisa los recursos de extensión `sourceControlConfiguration` nuevos o actualizados para que estén disponibles en el recurso de Kubernetes habilitado para Azure Arc, los agentes requieren conectividad para que el estado deseado se extraiga al clúster. Cuando los agentes no pueden conectarse a Azure, las propiedades de estado deseado declaradas en el recurso `sourceControlConfiguration` de Azure Resource Manager no se aplican en el clúster.
> * Las entradas de clientes confidenciales, como la clave privada, el contenido de hosts conocidos, el nombre de usuario HTTPS y el token/contraseña, no se almacenan durante más de 48 horas en los servicios de Kubernetes habilitados para Azure Arc. Si usa entradas confidenciales para las configuraciones, se recomienda poner en línea los clústeres lo más regularmente posible.

## <a name="apply-configurations-at-scale"></a>Aplicación de configuraciones a escala

Dado que Azure Resource Manager administra las configuraciones, puede usar Azure Policy para automatizar la creación de la misma configuración en todos los recursos de Kubernetes habilitado para Azure Arc en el ámbito de una suscripción o de un grupo de recursos. 

Esta aplicación a escala garantiza que se pueda aplicar una configuración de línea de base común (que contiene configuraciones, como ClusterRoleBindings, RoleBindings y NetworkPolicy) a toda la flota o inventario de clústeres de Kubernetes habilitado para Azure Arc.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster a Azure Arc](./connect-cluster.md)
* [Creación de configuraciones en el clúster de Kubernetes habilitado para Arc](./use-gitops-connected-cluster.md)
* [Uso de Azure Policy para aplicar configuraciones a escala](./use-azure-policy.md)