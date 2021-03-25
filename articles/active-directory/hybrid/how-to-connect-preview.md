---
title: 'Azure AD Connect: Características en versión preliminar | Microsoft Docs'
description: En este tema se describen más detenidamente las características que se encuentran en la versión preliminar en Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/15/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d48cd8f95ecaf3e537a5221d766af150a51aa31
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102517691"
---
# <a name="more-details-about-features-in-preview"></a>Más detalles sobre las características de vista previa
En este tema se describe cómo usar las características que actualmente forman parte de la versión preliminar.

## <a name="azure-ad-connect-sync-v2-endpoint-api"></a>API de punto de conexión de Azure AD Connect Sync V2

Hemos implementado un nuevo punto de conexión (API) para Azure AD Connect que mejora el rendimiento de las operaciones del servicio de sincronización para Azure Active Directory. Al usar el nuevo punto de conexión V2, experimentará mejoras de rendimiento notables en la exportación y la importación respecto a Azure AD. Este nuevo punto de conexión también admite la sincronización de grupos con hasta 250 000 miembros. El uso de este punto de conexión también le permite reescribir grupos unificados de Microsoft 365, sin límite máximo de pertenencia, en la instancia de Active Directory local, cuando está habilitada la escritura diferida de grupos. Para más información, vea [API de punto de conexión de Azure AD Connect Sync V2](how-to-connect-sync-endpoint-api-v2.md).

## <a name="user-writeback"></a>Reescritura de usuarios
> [!IMPORTANT]
> La característica en vista previa de escritura diferida de usuario, se quitó en la actualización de agosto de 2015 a Azure AD Connect. Si la ha habilitado, debería deshabilitarla.
>
>

## <a name="next-steps"></a>Pasos siguientes
Continúe su [Instalación personalizada de Azure AD Connect](how-to-connect-install-custom.md).

Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
