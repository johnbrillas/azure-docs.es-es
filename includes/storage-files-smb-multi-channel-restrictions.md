---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 06db7bcb5698f152dd5062762fdb3d59ae326e22
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102603250"
---
SMB multicanal para los recursos compartido de archivos de Azure tiene actualmente las siguientes restricciones:
- Solo se puede usar con cuentas de FileStorage con redundancia local.
- Solo se admite para clientes de Windows. 
- El número máximo de caracteres es cuatro.
- No se admite SMB directo.
- Los puntos de conexión privados para las cuentas de almacenamiento no se admiten.
- En el caso de las cuentas de Storage con [autenticación basada en identidad](../articles/storage/files/storage-files-active-directory-overview.md) Active Directory Domain Services (AD DS) local o Azure AD DS habilitada para Azure Files, los clientes SMB no podrían usar el Explorador de archivos de Windows para configurar permisos NTFS en directorios y archivos.
    - Use la herramienta [icacls](/windows-server/administration/windows-commands/icacls) de Windows o el comando [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl) para configurar los permisos.

