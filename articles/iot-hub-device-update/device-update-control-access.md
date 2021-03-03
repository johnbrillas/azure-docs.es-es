---
title: Información sobre la autenticación y autorización de Device Update para IoT Hub | Microsoft Docs
description: Descubra cómo la actualización de dispositivos de IoT Hub usa la funcionalidad RBAC de Azure para autenticar y autorizar usuarios y API de servicio.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 07310a5b6f275d4a35a3649c22aeea68045dde8b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660505"
---
# <a name="azure-role-based-access-control-rbac-and-device-update"></a>Control de acceso basado en rol (RBAC) de Azure y actualización de dispositivos

Device Update utiliza la funcionalidad RBAC de Azure para autorizar y autenticar los usuarios y las API de servicio.

## <a name="configure-access-control-roles"></a>Configuración de roles de control de acceso

Para que otros usuarios y aplicaciones tengan acceso a Device Update, es necesario concederles acceso a este recurso. Estos son los roles que se admiten en Device Update

|   Nombre de rol   | Descripción  |
| :--------- | :---- |
|  Administrador de Device Update | Tiene acceso a todos los recursos de actualización de dispositivos  |
|  Lector de Device Update| Puede ver todas las actualizaciones e implementaciones |
|  Administrador de contenido de Device Update | Puede ver, importar y eliminar actualizaciones  |
|  Lector de contenido de Device Update | Puede ver las actualizaciones  |
|  Administrador de implementaciones de Device Update | Puede administrar la implementación de actualizaciones en los dispositivos|
|  Lector de implementaciones de Device Update| Puede ver la implementación de actualizaciones en los dispositivos |

Puede combinar varios roles para proporcionar el nivel de acceso adecuado. Por ejemplo, un desarrollador puede importar y administrar actualizaciones utilizando el rol Administrador de contenido de Device Update, pero necesita el rol Lector de implementaciones de Device Update para poder ver el progreso de una actualización. Por otro lado, un operador de soluciones que tenga el rol Lector de Device Update puede ver todas las actualizaciones, pero necesita el rol Administrador de implementaciones de Device Update para poder implementar una actualización específica en los dispositivos.


## <a name="authenticate-to-device-update-rest-apis-for-publishing-and-management"></a>Autenticación en las API REST de Device Update para la publicación y administración de contenido

Device Update también utiliza Azure AD como mecanismo de autenticación para publicar y administrar contenido a través de las API de servicio. Para empezar, debe crear y configurar una aplicación cliente.

### <a name="create-client-azure-ad-app"></a>Creación de una aplicación cliente de Azure AD

Para integrar una aplicación o un servicio con Azure AD, [debe registrar primero](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app) una aplicación con Azure AD. La configuración de la aplicación cliente variará en función del flujo de autorización utilizado.  La configuración siguiente es un ejemplo orientativo para los casos en los que se utilizan las API REST de Device Update.

* Configure la autenticación del cliente: "redirect URIs for native or web client" ("redirigir URI de cliente nativo o cliente web").
* Configure los permisos de las API - Device Update para IoT Hub expone:
  * Permisos delegados: "user_impersonation"
  * **Opcional**, conceda consentimiento de administrador.

[Pasos siguientes: Creación de recursos de actualización de dispositivos y configuración de roles de control de acceso](./create-device-update-account.md)
