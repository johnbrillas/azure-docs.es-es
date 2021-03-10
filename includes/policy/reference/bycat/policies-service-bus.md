---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 9305caa74728217c030b929e2ac0a01d5dca125b
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102429662"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Todas las reglas de autorización, excepto RootManageSharedAccessKey, se deben eliminar del espacio de nombres de Service Bus](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa1817ec0-a368-432a-8057-8371e17ac6ee) |Los clientes de Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso en las entidades para que las colas y los temas proporcionen acceso solo a la entidad específica. |Audit, Deny, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditNamespaceAccessRules_Audit.json) |
|[Los registros de recursos de Service Bus deben estar habilitados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8d36e2f-389b-4ee4-898d-21aeb69a0f45) |Habilitación de la auditoría de los registros de recursos. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |[4.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json) |
|[Los espacios de nombres prémium de Service Bus deben usar una clave administrada por el cliente para el cifrado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F295fc8b1-dc9f-4f53-9c61-3f313ceab40a) |Azure Service Bus permite cifrar los datos en reposo con claves administradas por Microsoft (opción predeterminada) o claves administradas por el cliente. Si decide cifrar los datos con claves administradas por el cliente, podrá asignar, rotar, deshabilitar y revocar el acceso a las claves que Service Bus utiliza para cifrar los datos en el espacio de nombres. Tenga en cuenta que Service Bus solo admite el cifrado con claves administradas por el cliente en los espacios de nombres prémium. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_CustomerManagedKeyEnabled_Audit.json) |
