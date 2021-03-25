---
title: Desinstalación de Azure AD Connect
description: En este documento se describe cómo desinstalar Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/09/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5eb537a70c69745c8067ffb71cfb895a0875945
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96934144"
---
# <a name="uninstall-azure-ad-connect"></a>Desinstalación de Azure AD Connect

En este documento se describe cómo desinstalar correctamente Azure AD Connect.

## <a name="uninstall-azure-ad-connect-from-the-server"></a>Desinstalación de Azure AD Connect del servidor
Lo primero que debe hacer es quitar Azure AD Connect del servidor en el que se está ejecutando.  Siga estos pasos:

 1. En el servidor que se ejecuta Azure AD Connect, vaya al **Panel de control**.
 2. Click **Desinstalar un programa**
 ![Uninstall a program](media/how-to-connect-uninstall/uninstall-1.png)</br>
 
 3. Seleccione **Azure AD Connect**.
 ![Seleccionar Azure AD Connect](media/how-to-connect-uninstall/uninstall-2.png)</br>
 
 4. Cuando se le indique, haga clic en **Sí** para confirmar.
 5. Esta confirmación abrirá la pantalla de Azure AD Connect.  Haga clic en **Quitar**.
 ![Remove](media/how-to-connect-uninstall/uninstall-3.png)</br>
 
 6. Cuando se haya completado esta acción, haga clic en **Salir**.
 7. ![Salir](media/how-to-connect-uninstall/uninstall-4.png)</br>
 
 8. De nuevo en el **Panel de control** haga clic en **Actualizar** y ya no debería aparecer ningún componente.


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
- [Instalación de Azure AD Connect mediante una base de datos existente de ADSync](how-to-connect-install-existing-database.md)
- [Instalación de Azure AD Connect mediante permisos de administrador delegados de SQL](how-to-connect-install-sql-delegation.md)

