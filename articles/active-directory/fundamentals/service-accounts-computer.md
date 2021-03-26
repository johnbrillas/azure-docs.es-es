---
title: Protección de cuentas de equipo | Azure Active Directory
description: Guía para proteger las cuentas de equipos locales.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 2/15/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22faba20cb12ae755f19fe43c295d98f9b364cbe
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100416756"
---
# <a name="securing-computer-accounts"></a>Protección de cuentas de equipo

La cuenta de equipo, o la cuenta LocalSystem, es una cuenta integrada con privilegios muy elevados que tiene acceso a prácticamente todos los recursos del equipo local. Esta cuenta no está asociada a ninguna cuenta de usuario con sesión iniciada. Los servicios que se ejecutan como LocalSystem acceden a los recursos de red al presentar las credenciales del equipo a los servidores remotos. Presenta las credenciales con el formato <nombre_de_dominio>\<computer_name>$. El nombre predefinido de una cuenta de equipo es NT AUTHORITY\SYSTEM. Se puede usar para iniciar un servicio y proporcionar el contexto de seguridad para ese servicio.

![[Imagen 4](.\media\securing-service-accounts\secure-computer-accounts-image-1.png)](.\media\securing-service-accounts\secure-computer-accounts-image-1.png)

## <a name="benefits-of-using-the-computer-account"></a>Ventajas de usar la cuenta de equipo

La cuenta de equipo proporciona las ventajas siguientes.

* **Acceso local sin restricciones**: la cuenta de equipo proporciona acceso completo a los recursos locales de la máquina.

* **Administración automática de contraseñas**: con la cuenta de equipo, ya no es necesario que cambien las contraseñas de manera manual. En su lugar, esta cuenta es miembro de Active Directory y la contraseña de la cuenta se cambia automáticamente. También elimina la necesidad de registrar el nombre de entidad de seguridad de servicio para el servicio.

* **Derechos de acceso limitados fuera de la máquina**: la lista de control de acceso (ACL) predeterminada en Active Directory Domain Services permite un acceso mínimo para las cuentas de equipo. Si este servicio fuese a recibir un ataque, solo tendría acceso limitado a los recursos de la red.

## <a name="assess-security-posture-of-computer-accounts"></a>Evaluación de la posición de seguridad de las cuentas de equipo

Desafíos posibles y mitigaciones asociadas al usar cuentas de equipo. 

| Issues| Mitigaciones |
| - | - |
| Las cuentas de equipo están sujetas a eliminación y recreación cuando el equipo sale del dominio y vuelve a unirse.| Valide la necesidad de agregar un equipo a un grupo de AD y compruebe qué cuenta de equipo se ha agregado a un grupo mediante los scripts de ejemplo proporcionados en esta página.| 
| Si agrega una cuenta de equipo a un grupo, todos los servicios que se ejecutan como LocalSystem en ese equipo reciben los derechos de acceso del grupo.| Sea selectivo con respecto a las pertenencias a grupos de la cuenta de equipo. Evite que las cuentas de equipo sean miembros de los grupos de administradores de dominio, porque el servicio asociado tiene acceso completo a Active Directory Domain Services. |
| Valores predeterminados de red incorrectos para LocalSystem| No suponga que la cuenta de equipo tiene el acceso limitado predeterminado a los recursos de red. En su lugar, compruebe atentamente las pertenencias a grupos de esta cuenta. |
| Servicios desconocidos que se ejecutan como LocalSystem| Asegúrese de que todos los servicios que se ejecutan con la cuenta LocalSystem son servicios de Microsoft o servicios de confianza de terceros. |


## <a name="find-services-running-under-the-computer-account"></a>Búsqueda de los servicios que se ejecutan en la cuenta de equipo

Use el cmdlet de PowerShell siguiente para buscar servicios que se ejecuten en el contexto de LocalSystem.

```powershell

Get-WmiObject win32_service | select Name, StartName | Where-Object {($_.StartName -eq "LocalSystem")}
```

**Búsqueda de cuentas de equipo que son miembros de un grupo específico**

Use el cmdlet de PowerShell siguiente para buscar cuentas de equipo que sean miembros de un grupo específico.

```powershell

```Get-ADComputer -Filter {Name -Like "*"} -Properties MemberOf | Where-Object {[STRING]$_.MemberOf -like "Your_Group_Name_here*"} | Select Name, MemberOf
```

**Búsqueda de cuentas de equipo que son miembros de grupos con privilegios**

Use el cmdlet de PowerShell siguiente para buscar cuentas de equipo que sean miembros de los grupos de administradores de identidades (administradores de dominio, administradores de empresas, administradores)

```powershell
Get-ADGroupMember -Identity Administrators -Recursive | Where objectClass -eq "computer"
```
## <a name="move-from-computer-accounts"></a>Migrar de cuentas de equipo

> [!IMPORTANT]
> Las cuentas de equipo son cuentas con muchos privilegios y deben usarse solo cuando el servicio necesita acceso sin restricciones a los recursos locales de la máquina y no es posible usar una cuenta de servicio administrada (MSA).

* Compruebe con el propietario del servicio si su servicio se puede ejecutar con una MSA. Si es así, use una cuenta de servicio administrada de grupo (gMSA) o una cuenta de servicio administrada independiente (sMSA).

* Use una cuenta de usuario de dominio solo con los privilegios necesarios para ejecutar el servicio.

## <a name="next-steps"></a>Pasos siguientes 

Consulte los artículos siguientes sobre cómo proteger las cuentas de servicio.

* [Introducción a las cuentas de servicio locales](service-accounts-on-premises.md)

* [Protección de cuentas de servicio administradas de grupo](service-accounts-group-managed.md)

* [Protección de cuentas de servicio administradas independientes](service-accounts-standalone-managed.md)

* [Protección de cuentas de equipo](service-accounts-computer.md)

* [Protección de cuentas de usuario](service-accounts-user-on-premises.md)

* [Control de cuentas de servicio locales](service-accounts-govern-on-premises.md)

 

 
