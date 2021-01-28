---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 6a5c86dc1b10ed79e5dabbf5be7bf3ee2b677dee
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98807690"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El punto de conexión de Bot Service debe ser un URI de HTTPS válido](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6164527b-e1ee-4882-8673-572f425f5e0a) |Los datos se pueden alterar durante la transmisión. Hay protocolos que proporcionan cifrado para solucionar problemas de uso indebido y manipulación. Para asegurarse de que los bots se comunican solo por canales cifrados, establezca el punto de conexión en un URI de HTTPS válido. Esto garantiza que se usa el protocolo HTTPS para cifrar los datos en tránsito; además, suele ser un requisito de cumplimiento de los estándares normativos o del sector. Visite: [https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines](https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines). |audit, deny, disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Services/BotService_ValidEndpoint_Audit.json) |
