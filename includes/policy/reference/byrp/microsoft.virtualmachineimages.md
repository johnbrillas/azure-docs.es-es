---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: fde89a08ea1899eff5bcb4778f19a0987b080959
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98802630"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las plantillas de VM Image Builder deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2154edb9-244f-4741-9970-660785bccdaa) |Audite las plantillas de VM Image Builder que no tengan ninguna red virtual configurada. Cuando una red virtual no está configurada, se crea y se usa una IP pública en su lugar que puede exponer recursos directamente a Internet y aumentar la superficie expuesta a ataques potencial. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/VM%20Image%20Builder/PrivateLinkEnabled_Audit.json) |
