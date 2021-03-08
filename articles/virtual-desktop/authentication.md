---
title: 'Autenticación de Windows Virtual Desktop: Azure'
description: Métodos de autenticación de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 04a4366bfee6b1d9c5f52d649910163269962684
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101709264"
---
# <a name="supported-authentication-methods"></a>Métodos de autenticación admitidos

En este artículo, le mostraremos una breve descripción de los tipos de autenticación que puede usar en Windows Virtual Desktop.

## <a name="session-host-authentication"></a>Autenticación de host de sesión

Windows Virtual Desktop admite NT LAN Manager (NTLM) y Kerberos para la autenticación del host de sesión. Sin embargo, para utilizar Kerberos, el cliente debe obtener vales de seguridad de Kerberos de un servicio de Centro de distribución de claves (KDC) que se ejecute en un controlador de dominio. Para obtener vales, el cliente necesita una línea de visión directa al controlador de dominio. Para ello, puede usar la red corporativa. También puede usar una conexión VPN a la red corporativa o configurar un [servidor proxy KDC](key-distribution-center-proxy.md).

Estos son los métodos de inicio de sesión admitidos actualmente:

- Cliente de escritorio de Windows
    - Nombre de usuario y contraseña
    - Tarjeta inteligente
    - Windows Hello para empresas (solo confianza de certificados)
- Cliente de Microsoft Store
    - Nombre de usuario y contraseña
- Cliente web
    - Nombre de usuario y contraseña
- Android
    - Nombre de usuario y contraseña
- iOS
    - Nombre de usuario y contraseña
- macOS
    - Nombre de usuario y contraseña

>[!NOTE]
>La tarjeta inteligente y Windows Hello para empresas solo pueden usar Kerberos para iniciar sesión. Para iniciar la sesión con Kerberos se necesita una línea de visión al controlador de dominio o un [servidor proxy KDC](key-distribution-center-proxy.md).

## <a name="hybrid-identity"></a>Identidad híbrida

Windows Virtual Desktop admite [identidades híbridas](../active-directory/hybrid/whatis-hybrid-identity.md) a través de Azure Active Directory (AD), incluidas las federadas mediante Servicios de federación de Active Directory (AD FS). Dado que debe ser posible detectar a los usuarios a través de Azure AD, Windows Virtual Desktop no admite implementaciones de Active Directory independientes con ADFS.

## <a name="single-sign-on-sso"></a>Inicio de sesión único (SSO)

Actualmente Windows Virtual Desktop no admite Servicios de federación de Active Directory (AD FS) para el inicio de sesión único.

La única manera de evitar que le pidan sus credenciales del host de sesión es guardarlas en el cliente. Se recomienda hacer esto solo con dispositivos seguros a fin de evitar que otros usuarios tengan acceso a los recursos.

## <a name="next-steps"></a>Pasos siguientes

¿Tiene curiosidad sobre otras formas de proteger la implementación? Consulte [Procedimientos recomendados de seguridad](security-guide.md).
