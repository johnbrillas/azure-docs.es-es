---
title: 'Registro de autoservicio de External Identities: Azure AD'
description: Obtenga información acerca de cómo permitir que los usuarios externos se registren en sus aplicaciones habilitando el registro de autoservicio. Cree una experiencia de inicio de sesión personalizada mediante la personalización del flujo de usuario de registro de autoservicio.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13023ef93cabcf46924cc2cc76dc2d868c4a1ddd
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101653575"
---
# <a name="self-service-sign-up"></a>Registro de autoservicio

Al compartir una aplicación con usuarios externos, es posible que no siempre sepa de antemano quién necesitará tener acceso a la aplicación. Como alternativa al envío de invitaciones directamente a los usuarios, puede permitir que los usuarios externos se registren en aplicaciones específicas habilitando el registro de autoservicio. Puede crear una experiencia de registro personalizada mediante la personalización del flujo de usuario de registro de autoservicio. Por ejemplo, puede proporcionar opciones de registro con Azure AD o proveedores de identidades sociales y recopilar información sobre el usuario durante el proceso de registro.

> [!NOTE]
> Puede asociar flujos de usuarios a las aplicaciones que compila la organización. Los flujos de usuario no se pueden usar para las aplicaciones de Microsoft, como SharePoint o Teams.

## <a name="user-flow-for-self-service-sign-up"></a>Flujo de usuario para el registro de autoservicio

Un flujo de usuario de registro de autoservicio crea una experiencia de registro para los usuarios externos a través de la aplicación que quiera compartir. El flujo de usuario puede asociarse a una o varias de sus aplicaciones. En primer lugar, habilitará el registro de autoservicio para el inquilino y se federará con los proveedores de identidades que quiera permitir que usen los usuarios externos para iniciar sesión. A continuación, debe crear y personalizar el flujo de usuario de registro y asignarle sus aplicaciones.
Puede configurar las opciones de flujo de usuario para controlar el modo en que el usuario se registra en la aplicación:

- Tipos de cuenta utilizados para iniciar sesión, como cuentas de redes sociales (como Facebook) o cuentas de Azure AD.
- Atributos que se recopilan del registro del usuario, como el nombre, el código postal o el país o región de residencia.

Cuando un usuario quiere iniciar sesión en su aplicación, ya sea una aplicación web, móvil, de escritorio o de una sola página (SPA), la aplicación inicia una solicitud de autorización a un punto de conexión proporcionado por el flujo de usuario. El flujo de usuario define y controla la experiencia del usuario. Cuando el usuario completa el flujo de usuario de registro, Azure AD genera un token y, luego, redirige al usuario de vuelta a la aplicación. Una vez completado el registro, se aprovisiona una cuenta de invitado para el usuario en el directorio. Varias aplicaciones pueden usar el mismo flujo de usuario.

## <a name="example-of-self-service-sign-up"></a>Ejemplo de registro de autoservicio

En el ejemplo siguiente, se muestra cómo vamos a traer proveedores de identidades sociales a Azure AD con funcionalidades de registro de autoservicio para usuarios invitados.  
Un asociado de Woodgrove abre la aplicación Woodgrove. Decide que quiere registrarse para obtener una cuenta de proveedor, por lo que selecciona Solicitar cuenta de proveedor, que inicia el flujo de registro de autoservicio.

![Ejemplo de página de inicio de registro de autoservicio](media/self-service-sign-up-overview/example-start-sign-up-flow.png)

Usa el correo electrónico de su elección para registrarse.

![Ejemplo que muestra la selección de Facebook para el inicio de sesión](media/self-service-sign-up-overview/example-sign-in-with-facebook.png)

Azure AD crea una relación con Woodgrove mediante la cuenta de Facebook del asociado y crea una cuenta de invitado nueva para el usuario después de que se registre.

Woodgrove quiere obtener más información sobre el usuario, como el nombre, el nombre de la empresa, el código de registro del negocio y el número de teléfono.

![Ejemplo que muestra los atributos de registro del usuario](media/self-service-sign-up-overview/example-enter-user-attributes.png)

El usuario escribe la información, continúa con el flujo de registro y obtiene acceso a los recursos que necesita.

![Ejemplo que muestra el usuario que ha iniciado sesión](media/self-service-sign-up-overview/example-signed-in.png)

## <a name="next-steps"></a>Pasos siguientes

 Para obtener más información, consulte [Incorporación del registro de autoservicio a una aplicación](self-service-sign-up-user-flow.md).