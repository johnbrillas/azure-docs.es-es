---
title: Actualización de un clúster de Red Hat OpenShift en Azure
description: Aprenda a actualizar un clúster de Red Hat OpenShift en Azure en el que se ejecute OpenShift 4
ms.service: container-service
ms.topic: article
ms.date: 1/10/2021
author: sakthi-vetrivel
ms.author: suvetriv
keywords: aro, openshift, az aro, red hat, cli
ms.openlocfilehash: 98ab8ff1e321929a9007c28f487d5861f6ac9930
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99064442"
---
# <a name="upgrade-an-azure-red-hat-openshift-aro-cluster"></a>Actualización de un clúster de Red Hat OpenShift en Azure (ARO)

Parte del ciclo de vida del clúster de ARO implica efectuar actualizaciones periódicas a la versión más reciente de OpenShift. Es importante que aplique las versiones de seguridad más recientes o que actualice el software para obtener las últimas características. En este artículo se muestra cómo actualizar todos los componentes de un clúster de OpenShift mediante la consola web de OpenShift.

## <a name="before-you-begin"></a>Antes de empezar

Para este artículo, es necesario usar la versión 2.0.65 de la CLI de Azure o posterior. Ejecute `az --version` para buscar la versión actual. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

En este artículo se da por supuesto que tiene acceso a un clúster existente de Red Hat OpenShift en Azure como usuario con privilegios de `admin`.

## <a name="check-for-available-aro-cluster-upgrades"></a>Comprobación de las actualizaciones disponibles del clúster de ARO

En la consola web de OpenShift, seleccione **Administration** > **Cluster Settings** (administración > configuración de clúster) y abra la pestaña **Details** (detalles).

Si el valor de **Update Status** (estado de actualización) del clúster muestra **Updates Available** (actualizaciones disponibles), puede actualizar el clúster.

## <a name="upgrade-your-aro-cluster"></a>Actualización del clúster de ARO

En la consola web del paso anterior, establezca el valor de **channel** (canal) en el canal correcto para la versión a la que quiere actualizar, como `stable-4.5`.

Elija una versión para actualizar y luego seleccione **Update** (actualizar). Verá que el estado de la actualización cambia a: `Update to <product-version> in progress`. Para revisar el progreso de la actualización del clúster, observe las barras de progreso de los operadores y nodos.

## <a name="next-steps"></a>Pasos siguientes
- [Información para actualizar un clúster de ARO mediante la CLI de OC](https://docs.openshift.com/container-platform/4.6/updating/updating-cluster-between-minor.html)
- Puede encontrar información sobre los avisos y actualizaciones disponibles de OpenShift Container Platform en la [sección de erratas](https://access.redhat.com/downloads/content/290/ver=4.6/rhel---8/4.6.0/x86_64/product-errata) del portal del cliente.
  
