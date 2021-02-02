---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 01/25/2021
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: ad0c532c2ac80fd8a3bb3e68431ff7fc274d73e0
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98792431"
---
| Resource | Límite |
| --- | --- |
| Suscripciones por inquilino de Azure Active Directory | Sin límite |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por suscripción |Sin límite |
| [Grupos de recursos](../articles/azure-resource-manager/management/overview.md) por suscripción |980 |
| Tamaño de solicitud de API de Azure Resource Manager |4 194 304 bytes |
| Etiquetas por suscripción<sup>1</sup> |50 |
| Cálculos de etiquetas únicas por suscripción<sup>1</sup> | 80 000 |
| [Implementaciones de nivel de suscripción](../articles/azure-resource-manager/templates/deploy-to-subscription.md) por ubicación | 800<sup>2</sup> |

<sup>1</sup>Puede aplicar hasta 50 etiquetas directamente a una suscripción. Sin embargo, la suscripción puede contener un número ilimitado de etiquetas que se aplican a los grupos de recursos y recursos de la misma. El número de etiquetas por recurso o grupo de recursos se limita a 50. Resource Manager devuelve una [lista de valores y nombres de etiqueta únicos](/rest/api/resources/tags) en la suscripción solo cuando haya 80 000 etiquetas, o menos. Sin embargo, aunque haya más, es posible encontrar un recurso por etiqueta.

<sup>2</sup>Si alcanza el límite de 800 implementaciones, elimine del historial las que no vaya a volver a necesitar. Para eliminar implementaciones de nivel de suscripción, use [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) o [az deployment sub delete](/cli/azure/deployment/sub#az-deployment-sub-delete).
