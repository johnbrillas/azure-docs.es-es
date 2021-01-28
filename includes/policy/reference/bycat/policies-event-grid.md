---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c49d7a5359de0bb244d4eaaf4540fe876b7cad22
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98807728"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los dominios de Azure Event Grid deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9830b652-8523-49cc-b1b3-e17dce1127ca) |Azure Private Link permite conectar la red virtual a los servicios de Azure sin una dirección IP pública en el origen o el destino. La plataforma de Private Link controla la conectividad entre el consumidor y los servicios en la red troncal de Azure. Al asignar puntos de conexión privados a los dominios de Event Grid en lugar de a todo el servicio, además se protege frente a riesgos de pérdida de datos. Más información en: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridDomains_EnablePrivateEndpoint_Audit.json) |
|[Los temas de Azure Event Grid deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4b90e17e-8448-49db-875e-bd83fb6f804f) |Azure Private Link permite conectar la red virtual a los servicios de Azure sin una dirección IP pública en el origen o el destino. La plataforma de Private Link administra la conectividad entre el consumidor y los servicios en la red troncal de Azure. Mediante la asignación de puntos de conexión privados a los temas en lugar de a todo el servicio, además se protege frente a riesgos de pérdida de datos. Más información en: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridTopics_EnablePrivateEndpoint_Audit.json) |
