---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 7c9064879f639b914e84c851d29e911cca16f6a9
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102429392"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los proveedores de Azure Attestation deben usar puntos de conexión privados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7b256a2d-058b-41f8-bed9-3f870541c40a) |Los puntos de conexión privados proporcionan una manera de conectar proveedores de Azure Attestation a los recursos de Azure sin enviar tráfico a través de la red pública de Internet. Al impedir el acceso público, los puntos de conexión privados ayudan a proteger contra el acceso anónimo no deseado. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Attestation/Attestation_PrivateLink_AuditIfNotExists.json) |
