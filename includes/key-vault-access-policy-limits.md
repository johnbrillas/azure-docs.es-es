---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 08/27/2020
ms.author: msmbaldwin
ms.openlocfilehash: 9e4d60e501ffc5b61c87a80b8dd13698cca28737
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934549"
---
Key Vault admite hasta 1024 entradas de directiva de acceso, donde cada entrada concede un conjunto de permisos distinto a una entidad de servicio particular: Debido a esta limitación, se recomienda asignar directivas de acceso a grupos de usuarios, siempre que sea posible, en lugar de a usuarios individuales. El uso de grupos facilita la administración de permisos para varias personas de la organización. Para más información, consulte [Administración del acceso a recursos y aplicaciones con grupos en Azure Active Directory](../articles/active-directory/fundamentals/active-directory-manage-groups.md).

Para la información completa sobre el control de acceso a Key Vault, consulte [Seguridad en Azure Key Vault: Administración de identidades y acceso](../articles/key-vault/general/security-overview.md#identity-management).