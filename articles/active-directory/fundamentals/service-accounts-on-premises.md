---
title: Introducción a las cuentas de servicio de Active Directory | Azure Active Directory
description: Introducción a los tipos de cuentas de servicio de Active Directory y cómo protegerlas.
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
ms.openlocfilehash: 55de24975dadf27293f305611c6ba07522e8aa90
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416750"
---
# <a name="introduction-to-active-directory-service-accounts"></a>Introducción a las cuentas de servicio de Active Directory

Un servicio tiene una identidad de seguridad principal que determina los derechos de acceso para los recursos locales y de red. El contexto de seguridad de un servicio de Microsoft Win32 viene determinado por la cuenta de servicio que se usa para iniciar el servicio. Una cuenta de servicio de utiliza para:
* Identificar y autenticar un servicio.
* Iniciar un servicio correctamente.
* Acceder a código o a una aplicación, o bien ejecutarlos.
* Iniciar un proceso. 

## <a name="types-of-on-premises-service-accounts"></a>Tipos de cuentas de servicio locales

En función de su caso de uso, puede usar una cuenta de servicio administrada (MSA), una cuenta de equipo o una cuenta de usuario para ejecutar un servicio. Los servicios se deben probar para confirmar que pueden usar una cuenta de servicio administrada. Si es posible, debe usar una.

### <a name="group-msa-accounts"></a>Cuentas MSA de grupo

Use [cuentas de servicio administradas de grupo](service-accounts-group-managed.md) (gMSA) siempre que sea posible para los servicios que se ejecutan en su entorno local. Las cuentas gMSA proporcionan una solución de identidad única para un servicio que se ejecuta en una granja de servidores o detrás de un equilibrador de carga de red. También se pueden usar para un servicio que se ejecuta en un solo servidor. [Las cuentas gMSA tienen requisitos específicos que se deben cumplir](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts).

### <a name="standalone-msa-accounts"></a>Cuentas MSA independientes

Si no puede usar una cuenta gMSA, use una [cuenta de servicio administrada independiente](service-accounts-standalone-managed.md) (sMSA). Las cuentas sMSA requieren al menos Windows Server 2008 R2. A diferencia de las cuentas gMSA, las cuentas sMSA solo se ejecutan en un servidor. Se pueden usar para varios servicios en ese servidor.

### <a name="computer-account"></a>Cuenta de equipo

Si no puede usar una cuenta MSA, investigue el uso de [cuentas de equipo](service-accounts-computer.md). La cuenta LocalSystem es una cuenta local predefinida que tiene amplios privilegios en el equipo local y actúa como la identidad del equipo en la red.   
‎Los servicios que se ejecutan como una cuenta LocalSystem acceden a los recursos de red mediante las credenciales de la cuenta del equipo con el formato <nombre_de_dominio>\<computer_name>.

NT AUTHORITY\SYSTEM es el nombre predefinido de la cuenta LocalSystem. Se puede usar para iniciar un servicio y para proporcionar el contexto de seguridad para ese servicio.

> [!NOTE]
> Cuando se usa una cuenta de equipo, no se puede saber qué servicio en el equipo está utilizando esa cuenta y, por tanto, no se puede auditar qué servicio está realizando cambios. 

### <a name="user-account"></a>Cuenta de usuario

Si no puede usar una cuenta MSA, investigue el uso de una [cuenta de usuario](service-accounts-user-on-premises.md). Las cuentas de usuario pueden ser cuentas de usuario de dominio o cuentas de usuario local.

Una cuenta de usuario de dominio permite al servicio sacar el máximo partido de las características de seguridad del servicio de Windows y Microsoft Active Directory Domain Services. El servicio tendrá el acceso local y de red concedido para la cuenta. También tendrá los permisos de todos los grupos de los que es miembro la cuenta. Las cuentas de servicio de dominio admiten la autenticación mutua Kerberos.

Una cuenta de usuario local (formato de nombre: ".\NombreDeUsuario") solo existe en la base de datos SAM del equipo host; no tiene ningún objeto de usuario en Active Directory Domain Services. No se puede autenticar una cuenta local con el dominio. Por lo tanto, un servicio que se ejecuta en el contexto de seguridad de una cuenta de usuario local no tiene acceso a los recursos de red (excepto como usuario anónimo). Los servicios que se ejecutan en el contexto de usuario local no admiten la autenticación mutua Kerberos en la que los clientes autentican el servicio. Por estos motivos, las cuentas de usuario locales no suelen ser adecuadas para los servicios habilitados para el uso de directorios.

> [!IMPORTANT]
> Las cuentas de servicio no deben ser miembros de ningún grupo con privilegios, ya que la pertenencia a grupos con privilegios otorga permisos que pueden suponer un riesgo para la seguridad. Cada servicio debe tener su propia cuenta de servicio para fines de auditoría y seguridad.

## <a name="choose-the-right-type-of-service-account"></a>Elección del tipo de cuenta de servicio correcto


| Criterios| gMSA| sMSA| Cuenta de equipo| Cuenta de usuario |
| - | - | - | - | - |
| La aplicación se ejecuta en un solo servidor| Sí| Sí. Use una cuenta gMSA si es posible.| Sí. Use una cuenta MSA si es posible.| Sí. Use una cuenta MSA si es posible. |
| La aplicación se ejecuta en varios servidores| Sí| No| No. La cuenta está asociada al servidor| Sí. Use una cuenta MSA si es posible. |
| La aplicación se ejecuta detrás de equilibradores de carga| Sí| No| No| Sí. Úsela solo si no puede usar una cuenta gMSA. |
| La aplicación se ejecuta en Windows Server 2008 R2| No| Sí| Sí. Use una cuenta MSA si es posible.| Sí. Use una cuenta MSA si es posible. |
| Se ejecuta en Windows Server 2012| Sí| Sí. Use una cuenta gMSA si es posible.| Sí. Use una cuenta MSA si es posible.| Sí. Use una cuenta MSA si es posible. |
| Requisito para restringir la cuenta de servicio a un solo servidor| No| Sí| Sí. Use una cuenta sMSA si es posible.| No. |


 

### <a name="use-server-logs-and-powershell-to-investigate"></a>Uso de registros de servidor y PowerShell para investigar

Puede usar registros de servidor para determinar qué servidores y en cuántos servidores se ejecuta una aplicación.

Puede ejecutar el siguiente comando de PowerShell para obtener una lista de la versión de Windows Server de todos los servidores de la red. 

```PowerShell

Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' `

-Properties Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

sort-Object -Property Operatingsystem |

Select-Object -Property Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

Out-GridView

```

## <a name="find-on-premises-service-accounts"></a>Búsqueda de cuentas de servicio locales

Se recomienda agregar un prefijo como "svc" a todas las cuentas que se usan como cuentas de servicio. Esta convención de nomenclatura facilita su búsqueda y administración. Considere también el uso de un atributo de descripción para la cuenta de servicio y el propietario de la cuenta de servicio, que puede ser un alias de equipo o el propietario del equipo de seguridad.

La búsqueda de cuentas de servicio locales es fundamental para garantizar su seguridad. Además, puede ser difícil para las cuentas que no son MSA. Se recomienda revisar todas las cuentas que tienen acceso a sus recursos locales importantes y determinar el equipo o las cuentas de usuario que pueden estar actuando como cuentas de servicio. También se pueden usar los siguientes métodos para buscar cuentas.

* Los artículos de cada tipo de cuenta tienen pasos detallados para buscar ese tipo de cuenta. Para obtener los vínculos a estos artículos, consulte la sección Pasos siguientes de este artículo.

## <a name="document-service-accounts"></a>Documentación de las cuentas de servicio

Una vez que haya encontrado las cuentas de servicio en el entorno local, documente la siguiente información acerca de cada cuenta. 

* Propietario. Persona responsable del mantenimiento de la cuenta.

* Propósito. La aplicación que representa la cuenta u otro propósito. 

* Ámbitos de permiso. ¿Qué permisos tiene y cuáles debería tener? ¿Qué ocurre si es miembro de algún grupo?

* Perfil de riesgo. ¿Cuál es el riesgo para su negocio si esta cuenta se pone en peligro? Si el riesgo es alto, use una cuenta MSA.

* Duración prevista y atestación periódica. ¿Cuánto tiempo prevé que esta cuenta esté activa? ¿Con qué frecuencia debe revisar y autenticar el propietario para satisfacer las necesidades continuas?

* Seguridad de contraseñas. Para las cuentas de usuario y de equipo local, donde se almacena la contraseña. Asegúrese de que las contraseñas se mantienen seguras y documente quién tiene acceso. Considere la posibilidad de usar [Privileged Identity Management](../privileged-identity-management/pim-configure.md) para proteger las contraseñas almacenadas. 

  

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre la protección de cuentas de servicio.

* [Introducción a las cuentas de servicio locales](service-accounts-on-premises.md)

* [Protección de cuentas de servicio administradas de grupo](service-accounts-group-managed.md)

* [Protección de cuentas de servicio administradas independientes](service-accounts-standalone-managed.md)

* [Protección de cuentas de equipo](service-accounts-computer.md)

* [Protección de cuentas de usuario](service-accounts-user-on-premises.md)

* [Control de cuentas de servicio locales](service-accounts-govern-on-premises.md)

 