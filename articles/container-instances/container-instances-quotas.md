---
title: Cuotas de servicio y disponibilidad de región
description: Cuotas, límites y disponibilidad de regiones del servicio Azure Container Instances.
ms.topic: article
ms.date: 07/22/2020
ms.openlocfilehash: eaaa8e0b2d72aaea546a1bc351da40932c1deb14
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "87384842"
---
# <a name="quotas-and-limits-for-azure-container-instances"></a>Cuotas y límites de Azure Container Instances

Todos los servicios de Azure incluyen ciertas cuotas y límites predeterminados para los recursos y las características. En este artículo se detallan los límites y las cuotas predeterminados para Azure Container Instances.

La disponibilidad de recursos de proceso, memoria y almacenamiento de Azure Container Instances varía en función de la región y el sistema operativo. Para más información, consulte [Disponibilidad de recursos para Azure Container Instances](container-instances-region-availability.md).

Use la API [Mostrar uso en una lista](/rest/api/container-instances/location/listusage) para revisar el uso de la cuota actual en una región para una suscripción.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Para solicitar un aumento de uno o varios recursos que admiten un aumento de este tipo, envíe una [solicitud de soporte técnico de Azure][azure-support] (seleccione "Cuota" en **Tipo de emisión**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
