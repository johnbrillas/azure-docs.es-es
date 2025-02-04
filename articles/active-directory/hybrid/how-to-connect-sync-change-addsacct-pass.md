---
title: 'Azure AD Connect Sync: cambio de la contraseña de la cuenta de AD DS | Microsoft Docs'
description: En este tema se describe cómo actualizar Azure AD Connect después de cambiar la contraseña de la cuenta de AD DS.
services: active-directory
keywords: cuenta de AD DS, cuenta de Active Directory, contraseña
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4077146292db1266d5dbc51cc577f952b2bff191
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85357518"
---
# <a name="changing-the-ad-ds-account-password"></a>Cambio de la contraseña de la cuenta de AD DS
La cuenta de AD DS se refiere a la cuenta de usuario que usa Azure AD Connect para comunicarse con la instancia local de Active Directory. Si cambia la contraseña de la cuenta de AD DS, debe actualizar Azure AD Connect Synchronization Service con la nueva contraseña. En caso contrario, el servicio ya no puede sincronizar correctamente con la instancia local de Active Directory y se producirán los errores siguientes:

* En Synchronization Service Manager, cualquier operación de importación o exportación con un directorio de AD local genera un error **no-start-credentials**.

* En el Visor de eventos de Windows, el registro de eventos de aplicación contiene un error con el **identificador de evento 6000** y mensaje **'El agente de administración "contoso.com" no se pudo ejecutar porque las credenciales no eran válidas'** .


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Cómo actualizar Synchronization Service con la nueva contraseña de la cuenta de AD DS
Para actualizar Synchronization Service con la nueva contraseña:

1. Inicie el Synchronization Service Manager (INICIO → Synchronization Service).
</br>![Sync Service Manager](./media/how-to-connect-sync-change-addsacct-pass/startmenu.png)  

2. Vaya a la pestaña **Conectores**.

3. Seleccione el **Conector AD** correspondiente a la cuenta de AD DS cuya contraseña se ha cambiado.

4. En **Acciones**, seleccione **Propiedades**.

5. En el cuadro de diálogo emergente, seleccione **Connect to Active Directory Forest** (Conectar con el bosque de Active Directory):

6. Escriba la nueva contraseña de la cuenta de AD DS en el cuadro de texto **Password** (Contraseña).

7. Haga clic en **OK** (Aceptar) para guardar la nueva contraseña y cerrar el cuadro de diálogo emergente.

8. Reinicie Azure AD Connect Synchronization Service en el Administrador de Control de servicios de Windows. Esto sirve para asegurarse de que cualquier referencia a la contraseña antigua se quita de la memoria caché.

## <a name="next-steps"></a>Pasos siguientes
**Temas de introducción**

* [Sincronización de Azure AD Connect: comprender y personalizar la sincronización](how-to-connect-sync-whatis.md)

* [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md)
