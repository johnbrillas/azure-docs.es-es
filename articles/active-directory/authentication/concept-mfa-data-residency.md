---
title: Residencia de los datos de Multi-Factor Authentication de Azure AD
description: Conozca los datos personales y de la organización que almacena Multi-Factor Authentication de Azure AD relativos a usted y sus usuarios, y los datos que permanecen en el país o región de origen.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 788a666e8ec509bbd29a8dbd503a60b3dddefd6b
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208359"
---
# <a name="data-residency-and-customer-data-for-azure-ad-multifactor-authentication"></a>Residencia de datos y datos de clientes en Multi-Factor Authentication de Azure AD

Azure AD almacena los datos de cliente en una ubicación geográfica en función de la dirección que proporcione la organización al suscribirse a un servicio de Microsoft Online, como Microsoft 365 y Azure. Para más información sobre dónde se almacenan los datos de cliente, puede consultar la sección [Dónde se encuentran tus datos](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) de Microsoft Trust Center.

Multi-Factor Authentication de Azure AD basado en la nube y el Servidor Multi-Factor Authentication de Azure AD procesan y almacenan algunos datos personales y de la organización. En este artículo se describe qué datos se almacenan y dónde.

El servicio Multi-Factor Authentication de Azure AD tiene centros de datos en EE. UU., Europa y Asia Pacífico. Las siguientes actividades se originan fuera de los centros de datos regionales, excepto donde se indica:

* La autenticación multifactor mediante llamadas telefónicas procede de centros de datos de EE. UU. y se enruta mediante proveedores globales.
* Las solicitudes de autenticación de usuario de uso general de otras regiones, como Europa o Australia, se procesan actualmente en función de la ubicación del usuario.
* Las notificaciones de inserción que usan la aplicación Microsoft Authenticator se procesan actualmente en los centros de datos regionales en función de la ubicación del usuario.
    * Los servicios específicos del fabricante del dispositivo, como las notificaciones de inserción de Apple, pueden estar fuera de la ubicación del usuario.

## <a name="personal-data-stored-by-azure-ad-multifactor-authentication"></a>Datos personales almacenados por Multi-Factor Authentication de Azure AD

Los datos personales son información de nivel de usuario concerniente a una persona específica. Los siguientes almacenes de datos contienen información personal:

* Usuarios bloqueados
* Usuarios omitidos
* Solicitudes de cambio de token de dispositivo de Microsoft Authenticator
* Informes de actividad de Multi-Factor Authentication
* Activaciones de Microsoft Authenticator

Esta información se conserva durante 90 días.

Multi-Factor Authentication de Azure AD no registra datos personales, como el nombre de usuario, el número de teléfono o la dirección IP, pero existe un elemento *UserObjectId* que identifica los intentos de autenticación multifactor de los usuarios. Los datos de registro se almacenan durante 30 días.

### <a name="azure-ad-multifactor-authentication"></a>Multi-Factor Authentication de Azure AD

En el caso de las nubes públicas de Azure (salvo la autenticación de Azure B2C, la extensión NPS y el adaptador de AD FS de Windows Server 2016 o 2019), se almacenan los siguientes datos personales:

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Multi-Factor Authentication     |
| SMS unidireccional                          | En registros de Multi-Factor Authentication     |
| Llamada de voz                           | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

Respecto a Microsoft Azure Government, Microsoft Azure Alemania, Microsoft Azure ofrecido por 21Vianet, la autenticación de Azure B2C, la extensión NPS y el adaptador de AD FS de Windows Server 2016 o 2019, se almacenan los siguientes datos personales:

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| SMS unidireccional                          | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| Llamada de voz                           | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

### <a name="multifactor-authentication-server"></a>Servidor Multi-Factor Authentication

Si implementa y ejecuta el Servidor Multi-Factor Authentication de Azure AD, se almacenan los siguientes datos personales:

> [!IMPORTANT]
> A partir del 1 de julio de 2019, Microsoft dejó de ofrecer el Servidor Multi-Factor Authentication en las implementaciones nuevas. Los nuevos clientes que quieran solicitar la autenticación multifactor a sus usuarios deberán usar Multi-Factor Authentication de Azure AD basado en la nube. Los clientes existentes que hayan activado el Servidor Multi-Factor Authentication antes del 1 de julio podrán descargar la versión más reciente y las actualizaciones futuras, así como generar credenciales de activación como de costumbre.

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| SMS unidireccional                          | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| Llamada de voz                           | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

## <a name="organizational-data-stored-by-azure-ad-multifactor-authentication"></a>Datos de la organización almacenados por Multi-Factor Authentication de Azure AD

Los datos de la organización consisten en información de inquilino que podría exponer una opción de configuración o cómo está configurado el entorno. La configuración de inquilino de las siguientes páginas de Multi-Factor Authentication de Azure Portal puede almacenar datos de la organización, como los umbrales de bloqueo o información que identifica al autor de llamada de las solicitudes de autenticación telefónica entrantes:

* Bloqueo de cuenta
* Alerta de fraude
* Notificaciones
* Configuración de la llamada telefónica

En el caso del Servidor Azure Multi-Factor Authentication, las siguientes páginas de Azure Portal pueden contener datos de la organización:

* Configuración del servidor
* Omisión por única vez
* Reglas de caché
* Estado del Servidor Multi-Factor Authentication

## <a name="multifactor-authentication-logs-location"></a>Ubicación de registros de autenticación multifactor

En la tabla siguiente se muestra la ubicación de los registros de servicio de las nubes públicas.

| Nube pública| Registros de inicio de sesión | Informe de actividad de Multi-Factor Authentication        | Registros de servicio de Multi-Factor Authentication       |
|-------------|--------------|----------------------------------------|------------------------|
| US          | US           | US                                     | US                     |
| Europa      | Europa       | EE. UU.                                     | Europa <sup>2</sup>    |
| Australia   | Australia    | EE. UU.<sup>1</sup>                         | Australia <sup>2</sup> |

<sup>1</sup>Los registros de código OATH se almacenan en Australia

<sup>2</sup>Los registros de llamadas de voz del servicio de autenticación multifactor se almacenan en EE. UU.

En la tabla siguiente se muestra la ubicación de los registros de servicio de las nubes soberanas.

| Nube soberana                      | Registros de inicio de sesión                         | Informe de actividad de autenticación multifactor (incluye datos personales)| Registros del servicio de autenticación multifactor |
|--------------------------------------|--------------------------------------|-------------------------------|------------------|
| Microsoft Azure Alemania              | Alemania                              | US                            | US               |
| Microsoft Azure operado por 21Vianet | China                                | US                            | US               |
| Nube de Microsoft Government           | US                                   | US                            | US               |

Los datos del informe de actividad de Multi-Factor Authentication contienen información personal, como el nombre principal de usuario (UPN) y el número de teléfono completo.

Los registros del servicio Multi-Factor Authentication se usan para operar el servicio.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre qué datos de los usuarios recopilan Multi-Factor Authentication de Azure AD basado en la nube y el Servidor Multi-Factor Authentication de Azure AD, consulte [Recopilación de datos de usuario de Multi-Factor Authentication de Azure AD](howto-mfa-reporting-datacollection.md).
