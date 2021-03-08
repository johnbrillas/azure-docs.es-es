---
title: Actualización de los agentes de Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Control de las actualizaciones de los agentes de Kubernetes habilitado para Azure Arc
keywords: Kubernetes, Arc, Azure, K8s, containers, agent, upgrade
ms.openlocfilehash: 172654f655e594c295a8807b417c32d7849bd4b1
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102121922"
---
# <a name="upgrading-azure-arc-enabled-kubernetes-agents"></a>Actualización de los agentes de Kubernetes habilitado para Azure Arc

Kubernetes habilitado para Azure Arc proporciona funcionalidades de actualización automática y manual para sus agentes. Si deshabilita la actualización automática y, en su lugar, recurre a la actualización manual, la directiva de compatibilidad de versiones será aplicable a los agentes de Arc y al clúster de Kubernetes subyacente.

## <a name="toggle-auto-upgrade-on-or-off-when-connecting-cluster-to-azure-arc"></a>Activación o desactivación de la actualización automática al conectar un clúster a Azure Arc

Kubernetes habilitado para Azure Arc proporciona a sus agentes funcionalidades de actualización automática listas para usarse.

El siguiente comando conecta un clúster a Azure Arc con la actualización automática **habilitada**:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
```

Con la actualización automática habilitada, el agente sondea Azure cada hora para buscar la disponibilidad de una versión más reciente de los agentes. Si el agente encuentra una versión más reciente disponible, desencadena una actualización del gráfico Helm para los agentes de Azure Arc.

Para deshabilitar la actualización automática, especifique el parámetro `--disable-auto-upgrade` al conectar el clúster a Azure Arc. El siguiente comando conecta un clúster a Azure Arc con la actualización automática **deshabilitada**:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest --disable-auto-upgrade
```

> [!TIP]
> Si tiene previsto deshabilitar la actualización automática, consulte la [directiva de compatibilidad de versiones](#version-support-policy) de Kubernetes habilitado para Azure Arc.

## <a name="toggle-auto-upgrade-onoff-after-connecting-cluster-to-azure-arc"></a>Activación o desactivación de la actualización automática después de conectar un clúster a Azure Arc

Después de conectar un clúster a Azure Arc, puede activar o desactivar la funcionalidad de actualización automática con el comando `az connectedk8s update`, como se muestra a continuación:

```console
az connectedk8s update --name AzureArcTest1 --resource-group AzureArcTest --auto-upgrade false
```

## <a name="manually-upgrade-agents"></a>Actualización manual de los agentes

Si ha deshabilitado la actualización automática de los agentes, puede iniciar manualmente las actualizaciones de estos mediante el comando `az connectedk8s upgrade`, como se muestra a continuación:

```console
az connectedk8s upgrade -g AzureArcTest1 -n AzureArcTest --agent-version 1.0.1
```

Kubernetes habilitado para Azure Arc sigue el [esquema de versionamiento semántico](https://semver.org/) estándar de `MAJOR.MINOR.PATCH` para el control de versiones de sus agentes. 

Cada número en la versión indica compatibilidad general con la versión anterior:

* Las **versiones principales** cambian cuando se producen actualizaciones de API incompatibles o la compatibilidad con versiones anteriores deja de funcionar.
* Las **versiones secundarias** cambian cuando los cambios de funcionalidad son compatibles con otras versiones anteriores secundarias.
* Las **versiones de revisión** cambian cuando se hacen correcciones de errores compatibles con versiones anteriores.

## <a name="version-support-policy"></a>Directiva de compatibilidad de versiones

Cuando se crean problemas de compatibilidad, Kubernetes habilitado para Azure Arc aplica la siguiente directiva de compatibilidad de versiones:

* Los agentes Kubernetes habilitado para Azure Arc tienen un período de compatibilidad de "N-2", donde "N" es la última versión secundaria de los agentes. 
  * Por ejemplo, si Kubernetes habilitado para Azure Arc presenta la versión 0.28.a hoy, las versiones 0.28.a, 0.28.b, 0.27.c, 0.27.d, 0.26.e y 0.26.f son compatibles con Azure Arc.

* Los clústeres de Kubernetes que se conectan a Azure Arc tienen un período de compatibilidad de "N-2", donde "N" es la versión secundaria estable más reciente de [Kubernetes ascendente](https://github.com/kubernetes/kubernetes/releases). 
  * Por ejemplo, si Kubernetes presenta la versión 1.20.a hoy, las versiones 1.20.a, 1.20.b, 1.19.c, 1.19.d, 1.18.e y 1.18.f son compatibles.

### <a name="how-often-are-minor-version-releases-of-azure-arc-enabled-kubernetes-available"></a>¿Con qué frecuencia están disponibles las versiones secundarias de Kubernetes habilitado para Azure Arc?

Se publica una versión secundaria de los agentes de Kubernetes habilitado para Azure Arc aproximadamente una vez al mes.

### <a name="what-happens-if-im-using-an-agent-version-or-a-kubernetes-version-outside-the-official-support-window"></a>¿Qué ocurre si uso una versión del agente o una versión de Kubernetes fuera del período de compatibilidad oficial?

"Sin compatibilidad" significa que las versiones que está ejecutando están fuera de las versiones "N-2" admitidas de los agentes y clústeres de Kubernetes ascendentes. Para solucionar el problema de compatibilidad, se le pedirá que actualice el clúster y los agentes a una versión compatible.

## <a name="next-steps"></a>Pasos siguientes

* Siga el inicio rápido para [conectar un clúster de Kubernetes a Azure Arc](./connect-cluster.md).
* ¿Ya tiene un clúster de Kubernetes conectado a Azure Arc? [Cree configuraciones en el clúster de Kubernetes habilitado para Arc](./use-gitops-connected-cluster.md).
* Aprenda a [usar Azure Policy para aplicar configuraciones a escala](./use-azure-policy.md).