---
title: Compatibilidad del explorador con la autenticación sin contraseña FIDO2 | Azure Active Directory
description: Las combinaciones de exploradores y sistemas operativos admiten la autenticación sin contraseña FIDO2 para aplicaciones que usan Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/02/2021
ms.author: nichola
author: knicholasa
manager: martinco
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f90edd5729ff5229be09bc3798082c33bdeead2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102632108"
---
# <a name="browser-support-of-fido2-passwordless-authentication"></a>Compatibilidad del explorador con la autenticación sin contraseña FIDO2

Azure Active Directory permite usar [claves de seguridad FIDO2](./concept-authentication-passwordless.md#fido2-security-keys) como un dispositivo sin contraseña. La disponibilidad de la autenticación FIDO2 para las cuentas de Microsoft se [anunció en 2018](https://techcommunity.microsoft.com/t5/identity-standards-blog/all-about-fido2-ctap2-and-webauthn/ba-p/288910). Como se describe en el anuncio, se deben implementar ciertas características y extensiones opcionales en la especificación CTAP de FIDO2 para admitir la autenticación segura con cuentas de Microsoft y Azure Active Directory. En el diagrama siguiente se muestra qué combinaciones de exploradores y sistemas operativos admiten la autenticación sin contraseña mediante claves de autenticación FIDO2 con Azure Active Directory.

## <a name="supported-browsers"></a>Exploradores compatibles

En esta tabla se muestra la compatibilidad con la autenticación de cuentas de Azure Active Directory (Azure AD) y Microsoft (MSA). Los consumidores crean cuentas de Microsoft para servicios como Xbox, Skype o Outlook.com. Entre los tipos de dispositivos admitidos se incluyen **USB**, transmisión de datos en proximidad (**NFC**) y Bluetooth de bajo consumo (**BLE**).

| SO | Chrome | Chrome  | Chrome | Edge | Edge | Edge | Firefox | Firefox | Firefox |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| | USB | NFC | BLE | USB | NFC | BLE | USB | NFC | BLE |
| **Windows**  | ![Chrome admite USB en Windows para las cuentas de AAD.][y] | ![Chrome admite NFC en Windows para las cuentas de AAD.][y] | ![Chrome admite BLE en Windows para las cuentas de AAD.][y] | ![Edge admite USB en Windows para las cuentas de AAD.][y] | ![Edge admite NFC en Windows para las cuentas de AAD.][y] | ![Edge admite BLE en Windows para las cuentas de AAD.][y] | ![Firefox admite USB en Windows para las cuentas de AAD.][y] | ![Firefox admite NFC en Windows para las cuentas de AAD.][y] | ![Firefox admite BLE en Windows para las cuentas de AAD.][y] |
| **macOS**  | ![Chrome admite USB en macOS para las cuentas de AAD.][y] | ![Chrome no admite NFC en macOS para las cuentas de AAD.][n] | ![Chrome no admite BLE en macOS para las cuentas de AAD.][n] | ![Edge admite USB en macOS para las cuentas de AAD.][y] | ![Edge no admite NFC en macOS para las cuentas de AAD.][n] | ![Edge no admite BLE en macOS para las cuentas de AAD.][n] | ![Firefox no admite USB en macOS para las cuentas de AAD.][n] | ![Firefox no admite NFC en macOS para las cuentas de AAD.][n] | ![Firefox no admite BLE en macOS para las cuentas de AAD.][n] |
| **Linux**  | ![Chrome admite USB en Linux para las cuentas de AAD.][y] | ![Chrome no admite NFC en Linux para las cuentas de AAD.][n] | ![Chrome no admite BLE en Linux para las cuentas de AAD.][n] | ![Edge no admite USB en Linux para las cuentas de AAD.][n] | ![Edge no admite NFC en Linux para las cuentas de AAD.][n] | ![Edge no admite BLE en Linux para las cuentas de AAD.][n] | ![Firefox no admite USB en Linux para las cuentas de AAD.][n] | ![Firefox no admite NFC en Linux para las cuentas de AAD.][n] | ![Firefox no admite BLE en Linux para las cuentas de AAD.][n] |



## <a name="unsupported-browsers"></a>Exploradores no admitidos

Las siguientes combinaciones de sistemas operativos y exploradores no son compatibles, pero se está investigando la compatibilidad y las pruebas futuras. Si desea ver otra compatibilidad de sistema operativo y explorador, deje comentarios mediante la herramienta de comentarios del producto en la parte inferior de la página.

| Sistema operativo | Browser |
| ---- | ---- |
| iOS | Safari, Brave |
| macOS | Safari |
| Android | Chrome |
| ChromeOS | Chrome |

## <a name="minimum-browser-version"></a>Versión mínima del explorador

Estos son los requisitos mínimos de la versión del explorador. 

| Browser | Versión mínima |
| ---- | ---- |
| Chrome | 76 |
| Edge | Windows 10, versión 1903<sup>1</sup> |
| Firefox | 66 |

<sup>1</sup>Todas las versiones del nuevo Microsoft Edge basado en Chromium son compatibles con Fido2. La compatibilidad con versiones anteriores de Microsoft Edge se agregó en la 1903.

## <a name="next-steps"></a>Pasos siguientes
[Habilitar el inicio de sesión con clave de seguridad sin contraseña (versión preliminar)](./howto-authentication-passwordless-security-key.md)

<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png
