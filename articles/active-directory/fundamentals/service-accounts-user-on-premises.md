---
title: Protección de cuentas de servicio basadas en el usuario | Azure Active Directory
description: Guía para proteger las cuentas de usuario locales.
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
ms.openlocfilehash: e484bdda33142024f2067649eaa67042fe7776f8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100416747"
---
# <a name="securing-user-based-service-accounts-in-active-directory"></a>Protección de cuentas de servicio basadas en usuario en Active Directory

Las cuentas de usuario locales son el enfoque tradicional para proteger los servicios que se ejecutan en Windows. Use estas cuentas como último recurso cuando el servicio no admita cuentas de servicio administradas globales (gMSA) y cuentas de servicio administradas independientes (sMSA). Consulte la introducción a las cuentas de servicio locales para obtener información sobre cómo seleccionar el mejor tipo de cuenta que se va a usar. Investigue también si puede trasladar el servicio para usar una cuenta de servicio de Azure como una identidad administrada o un principio de servicio. 

Se pueden crear cuentas de usuario locales destinadas a proporcionar un contexto de seguridad para los servicios y los privilegios concedidos necesarios para que los servicios puedan acceder a recursos locales y de red. Requieren la administración manual de contraseñas de forma similar a cualquier otra cuenta de usuario de Active Directory (AD). Los administradores de servicios y dominios deben observar los procesos seguros de administración de contraseñas para mantener estas cuentas seguras.

Cuando use una cuenta de usuario como cuenta de servicio, úsela solo para un único servicio. Asígnele un nombre que deje claro que se trata de una cuenta de servicio y para qué servicio. 

## <a name="benefits-and-challenges"></a>Ventajas y desafíos

Ventajas

Las cuentas de usuario locales son el tipo de cuenta más versátil para usar con los servicios. Todas las directivas que rigen las cuentas de usuario normales pueden controlar las cuentas de usuario que se usan como cuentas de servicio. Dicho esto, úselas solo si no puede usar una MSA. Evalúe también si una cuenta de equipo es una opción mejor. 

Desafíos con las cuentas de usuario locales

Los siguientes desafíos están asociados al uso de cuentas de usuario locales.

| Desafíos| Mitigaciones |
| - | - |
| La administración de contraseñas es un proceso manual que puede debilitar la seguridad y provocar tiempo de inactividad del servicio.| Asegúrese de que la complejidad de la contraseña y el cambio de contraseña se rigen por un proceso sólido que garantiza actualizaciones periódicas con contraseña segura. <br> Coordine el cambio de contraseña con una actualización de contraseña en el servicio, lo que provocará tiempo de inactividad del servicio. |
| La identificación de las cuentas de usuario locales que actúan como cuentas de servicio puede ser difícil.| Documente y mantenga registros de las cuentas de servicio implementadas en su entorno. <br> Realice un seguimiento del nombre de cuenta y de los recursos a los que tienen acceso asignado. <br> Considere la posibilidad de agregar un prefijo svc_ a todas las cuentas de usuario usadas como cuentas de servicio. |


## <a name="find-on-premises-user-accounts-used-as-service-accounts"></a>Búsqueda de cuentas de usuario locales que se utilizan como cuentas de servicio

Las cuentas de usuario locales son como cualquier otra cuenta de usuario de AD. Por lo tanto, puede ser difícil encontrar estas cuentas, ya que no hay ningún atributo único de una cuenta de usuario que la identifique como cuenta de servicio. 

Se recomienda crear una convención de nomenclatura fácilmente identificable para cualquier cuenta de usuario utilizada como cuenta de servicio.

Por ejemplo, agregue "service-" como prefijo y asigne al servicio el nombre: "service-HRDataConnector".

Puede usar algunos de los indicadores siguientes para encontrar estas cuentas de servicio; sin embargo, es posible que no se encuentren todas estas cuentas.

* Cuentas de confianza para la delegación.

* Cuentas con nombres de entidad de seguridad de servicio.

* Cuentas cuya contraseña está configurada para no expirar nunca.

Puede ejecutar los siguientes comandos de PowerShell para buscar las cuentas de usuario locales creadas para los servicios.

### <a name="find-accounts-trusted-for-delegation"></a>Búsqueda de cuentas de confianza para la delegación

```PowerShell

Get-ADObject -Filter {(msDS-AllowedToDelegateTo -like '*') -or (UserAccountControl -band 0x0080000) -or (UserAccountControl -band 0x1000000)} -prop samAccountName,msDS-AllowedToDelegateTo,servicePrincipalName,userAccountControl | select DistinguishedName,ObjectClass,samAccountName,servicePrincipalName, @{name='DelegationStatus';expression={if($_.UserAccountControl -band 0x80000){'AllServices'}else{'SpecificServices'}}}, @{name='AllowedProtocols';expression={if($_.UserAccountControl -band 0x1000000){'Any'}else{'Kerberos'}}}, @{name='DestinationServices';expression={$_.'msDS-AllowedToDelegateTo'}}

```

### <a name="find-accounts-with-service-principle-names"></a>Búsqueda de cuentas con nombres de entidad de seguridad de servicio

```PowerShell

Get-ADUser -Filter * -Properties servicePrincipalName | where {$_.servicePrincipalName -ne $null}

```

 

### <a name="find-accounts-with-passwords-set-to-never-expire"></a>Búsqueda de cuentas con contraseñas configuradas para que no expiren nunca

```PowerShell

Get-ADUser -Filter * -Properties PasswordNeverExpires | where {$_.PasswordNeverExpires -eq $true}

```


También puede auditar el acceso a recursos confidenciales y archivar los registros de auditoría en un sistema de administración de eventos e información de seguridad (SIEM). Mediante sistemas como Azure Log Analytics o Azure Sentinel, puede buscar y analizar cuentas de servicio.

## <a name="assess-security-of-on-premises-user-accounts"></a>Evaluación de la seguridad de las cuentas de usuario locales

Evalúe la seguridad de las cuentas de usuario locales que se usan como cuentas de servicio mediante los siguientes criterios:

* ¿Cuál es la directiva de administración de contraseñas?

* ¿La cuenta pertenece a algún grupo con privilegios?

* ¿La cuenta tiene acceso de lectura y escritura a recursos importantes?

### <a name="mitigate-potential-security-issues"></a>Mitigación de posibles problemas de seguridad

En la tabla siguiente se muestran los posibles problemas de seguridad y las mitigaciones correspondientes para las cuentas de usuario locales.

| Problemas de seguridad| Mitigaciones |
| - | - |
| Administración de contraseñas|* Asegúrese de que la complejidad de la contraseña y el cambio de contraseña se rigen por un proceso sólido que garantiza actualizaciones regulares con requisitos de contraseña seguros. <br> * Coordine el cambio de contraseña con una actualización de contraseña para minimizar el tiempo de inactividad del servicio. |
| La cuenta pertenece a grupos con privilegios.| Revise las pertenencias a grupos. Quite la cuenta de los grupos con privilegios. Conceda a la cuenta solo los derechos y permisos necesarios para ejecutar su servicio (consulte al proveedor de servicios). Por ejemplo, es posible que pueda denegar el inicio de sesión localmente o denegar el inicio de sesión interactivo. |
| La cuenta tiene acceso de lectura y escritura a recursos confidenciales.| Audite el acceso a recursos confidenciales. Archive los registros de auditoría en un SIEM (Azure Log Analytics o Azure Sentinel) para su análisis. Corrija los permisos de recursos si se detecta un nivel de acceso no deseado. |


## <a name="move-to-more-secure-account-types"></a>Migración a tipos de cuenta más seguros

Microsoft no recomienda que los clientes usen cuentas de usuario locales como cuentas de servicio. Para cualquier servicio que use este tipo de cuenta, evalúe si puede configurarse para usar una cuenta gMSA o sMSA en su lugar.

Además, evalúe si el propio servicio se puede migrar a Azure para poder usar tipos de cuentas de servicio más seguras. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos sobre la protección de cuentas de servicio.

* [Introducción a las cuentas de servicio locales](service-accounts-on-premises.md)

* [Protección de cuentas de servicio administradas de grupo](service-accounts-group-managed.md)

* [Protección de cuentas de servicio administradas independientes](service-accounts-standalone-managed.md)

* [Protección de cuentas de equipo](service-accounts-computer.md)

* [Protección de cuentas de usuario](service-accounts-user-on-premises.md)

* [Control de cuentas de servicio locales](service-accounts-govern-on-premises.md)

 
