---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 9df619bae59503c91c554d95bd80a42abbc3af18
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100100383"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los proveedores de Azure Attestation deben usar puntos de conexión privados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7b256a2d-058b-41f8-bed9-3f870541c40a) |Los puntos de conexión privados proporcionan una manera de conectar proveedores de Azure Attestation a los recursos de Azure sin enviar tráfico a través de la red pública de Internet. Al impedir el acceso público, los puntos de conexión privados ayudan a proteger contra el acceso anónimo no deseado. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Attestation/Attestation_PrivateLink_AuditIfNotExists.json) |
