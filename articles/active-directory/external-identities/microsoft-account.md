---
title: Proveedor de identidades de cuenta Microsoft (MSA) en Azure AD
description: Use Azure AD para permitir que un usuario externo (invitado) inicie sesión en sus aplicaciones de Azure AD con su cuenta Microsoft (MSA).
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25d3380277d0e653fd5cb03069b7d91cb363dd95
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101692934"
---
# <a name="microsoft-account-msa-identity-provider-for-external-identities-preview"></a>Proveedor de identidades de cuenta Microsoft (MSA) para External Identities (versión preliminar)

> [!NOTE]
> El proveedor de identidades de cuenta Microsoft es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Los usuarios invitados de B2B pueden usar sus propias cuentas personales de Microsoft para la colaboración B2B sin necesidad de configuración adicional. Los usuarios invitados pueden canjear sus invitaciones de colaboración B2B o completar los flujos de usuario de suscripción con su cuenta Microsoft personal.

Las cuentas Microsoft son aquellas que un usuario configura para poder acceder a productos y servicios en la nube de Microsoft orientados al consumidor, como Outlook, OneDrive, Xbox LIVE o Microsoft 365. La cuenta se crea y almacena en el sistema de cuentas de identidad de consumidor de Microsoft que ejecuta Microsoft.

## <a name="guest-sign-in-using-microsoft-accounts"></a>Inicio de sesión de invitado con cuentas Microsoft

La cuenta Microsoft está disponible en la lista de proveedores de identidades de External Identities de manera predeterminada. No se necesita ninguna configuración adicional para permitir que los usuarios invitados inicien sesión con su cuenta Microsoft mediante el flujo de invitación o un flujo de usuario de suscripción autoservicio.

![Cuenta Microsoft en la lista de proveedores de identidades](media/microsoft-account/microsoft-account-identity-provider.png)

### <a name="microsoft-account-in-the-invitation-flow"></a>Cuenta Microsoft en el flujo de invitación

Al [invitar a un usuario](add-users-administrator.md) a una colaboración B2B, puede especificar su cuenta Microsoft como dirección de correo electrónico que va a usar para iniciar sesión.

![Invitación con una cuenta Microsoft](media/microsoft-account/microsoft-account-invite.png)

### <a name="microsoft-account-in-self-service-sign-up-user-flows"></a>Cuenta Microsoft en flujos de usuario de suscripción autoservicio

La cuenta Microsoft es una opción del proveedor de identidades para los flujos de usuario de suscripción autoservicio. Los usuarios pueden suscribirse a las aplicaciones con sus propias cuentas Microsoft. En primer lugar, tiene que [habilitar la suscripción autoservicio](self-service-sign-up-user-flow.md) para el inquilino. Luego puede configurar un flujo de usuario para la aplicación y seleccionar la cuenta Microsoft como una de las opciones de inicio de sesión.

![Cuenta Microsoft en un flujo de usuario de suscripción autoservicio](media/microsoft-account/microsoft-account-user-flow.png)

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación de usuarios de colaboración B2B de Azure Active Directory](add-users-administrator.md)
- [Incorporación del registro de autoservicio a una aplicación](self-service-sign-up-user-flow.md)