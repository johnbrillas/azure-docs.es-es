---
title: 'Identidad híbrida: comparación de herramientas de integración de directorios | Microsoft Docs'
description: Esta página proporciona una tabla completa con la comparación de las distintas herramientas que se pueden usar para la integración de directorios.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70e5c7e5fa428d5a71ca1a7468bbaab2fc94078e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "101643851"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Identidades híbridas: comparación de las herramientas para la integración de directorios de identidades híbridas
A lo largo de los años, las herramientas para la integración de directorios han crecido y evolucionado.  


- Todavía se admiten [FIM](/previous-versions/windows/desktop/forefront-2010/ff182370(v=vs.100)) y [MIM](/microsoft-identity-manager/microsoft-identity-manager-2016), y permiten principalmente la sincronización entre sistemas locales.   El [conector FIM de Microsoft Azure AD](/previous-versions/mim/dn511001(v=ws.10)) es compatible con FIM y MIM, pero no se recomienda para las nuevas implementaciones, los clientes con orígenes locales, como Notes o SAP HCM, deben usar MIM para rellenar Active Directory Domain Services (AD DS) y, a continuación, usar también la sincronización de Azure AD Connect o el aprovisionamiento en la nube de Azure AD Connect para sincronizar desde AD DS a Azure AD.
- La [sincronización de Azure AD Connect](how-to-connect-sync-whatis.md) incorpora los componentes y la funcionalidad que se publicaron previamente en DirSync y la sincronización de Azure AD, para sincronizar entre bosques de AD DS y Azure AD.  
- El [aprovisionamiento en la nube de Azure AD Connect](../cloud-sync/what-is-cloud-sync.md) es un nuevo agente de Microsoft para sincronizar de AD DS a Azure AD, útil para los escenarios como la fusión y la adquisición en los que los bosques de AD de la empresa adquirida están aislados de los bosques de AD de la empresa principal.

Para más información sobre las diferencias entre la sincronización de Azure AD Connect y el aprovisionamiento en la nube de Azure AD, consulte el artículo [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](../cloud-sync/what-is-cloud-sync.md)

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).