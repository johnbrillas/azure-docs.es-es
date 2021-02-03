---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b631f447a5c43fb1695b6e07ec387c83747a8613
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220549"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las instancias de Container Registry se deben cifrar con una clave administrada por el cliente (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Use claves administradas por el cliente para administrar el cifrado en reposo del contenido de los registros. De manera predeterminada, los datos se cifran en reposo con claves administradas por el servicio, pero las claves administradas por el cliente (CMK) suelen ser necesarias para cumplir estándares de cumplimiento normativo. Las CMK permiten cifrar los datos con una clave de Azure Key Vault creada por el usuario y propiedad de este. Tiene control y responsabilidad totales del ciclo de vida de la clave, incluidos la rotación y administración. Más información sobre el cifrado de CMK en [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK). |Audit, Deny, Disabled |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Las instancias de Container Registry no deben permitir el acceso de red sin restricciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |De manera predeterminada, las instancias de Azure Container Registry aceptan conexiones a través de Internet de hosts de cualquier red. Para protegerlas frente a posibles amenazas, permita el acceso solo desde direcciones IP públicas específicas o intervalos de direcciones. Si el registro no tiene una regla de IP/firewall o una red virtual configurada, aparece en los recursos incorrectos. Obtenga más información sobre las reglas de red de Container Registry aquí: [https://aka.ms/acr/portal/public-network](https://aka.ms/acr/portal/public-network) y aquí [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Las instancias de Container Registry deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |Azure Private Link permite conectar la red virtual a servicios de Azure sin una dirección IP pública en el origen o el destino. La plataforma Private Link controla la conectividad entre el consumidor y los servicios a través de la red troncal de Azure. Al asignar puntos de conexión privados a las instancias de Container Registry en lugar de a todo el servicio, además se protege frente a riesgos de pérdida de datos. Más información en: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
