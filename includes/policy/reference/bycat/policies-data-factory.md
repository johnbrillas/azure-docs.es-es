---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 2d28e331b576c9152593b556864fb2c4b50a3742
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98807194"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las instancias de Azure Data Factory deben cifrarse con una clave administrada por el cliente (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4ec52d6d-beb7-40c4-9a9e-fe753254690e). |Utiliza claves administradas por el cliente (CMK) para administrar el cifrado en reposo de los datos de Azure Data Factory. De manera predeterminada, los datos del cliente se cifran con claves administradas por el servicio, pero las CMK suelen ser necesarias para cumplir estándares de cumplimiento normativo. Las CMK permiten cifrar los datos con una clave de Azure Key Vault creada por el usuario y propiedad de este. Tiene control y responsabilidad totales del ciclo de vida de la clave, incluidos la rotación y administración. Más información sobre el cifrado de CMK en [https://aka.ms/adf-cmk](https://aka.ms/adf-cmk). |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/DataFactory_CustomerManagedKey_Audit.json) |
