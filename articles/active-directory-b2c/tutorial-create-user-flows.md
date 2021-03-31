---
title: 'Tutorial: Creación de flujos de usuario en Azure Active Directory B2C'
description: Siga este tutorial para aprender a crear flujos de usuario en Azure Portal y habilitar el registro, el inicio de sesión y la edición de perfiles de usuario para las aplicaciones en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 03/22/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c42c6465af8e895d833332be847c134b97ee8ddc
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104781303"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Tutorial: Creación de flujos de usuario en Azure Active Directory B2C

En las aplicaciones, puede que tenga [flujos de usuario](user-flow-overview.md) que permitan a los usuarios registrarse, iniciar sesión o administrar su perfil. Puede crear varios flujos de usuario de diferentes tipos en un inquilino de Azure Active Directory B2C (Azure AD B2C) y usarlos en las aplicaciones según sea necesario. Los flujos de usuario se pueden volver a usar en todas las aplicaciones.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Creación de un flujo de usuario de registro e inicio de sesión
> * Habilitar el autoservicio de restablecimiento de contraseña
> * Creación de un flujo de usuario de edición de perfil


En este tutorial se muestra cómo crear flujos de usuario recomendados mediante Azure Portal. Si busca información acerca de cómo configurar el flujo de las credenciales de contraseña de propietario del recurso (ROPC) en la aplicación, consulte [Configuración del flujo de credenciales de contraseña de propietario del recurso en Azure AD B2C](add-ropc-policy.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!IMPORTANT]
> Hemos cambiado la manera en que hacemos referencia a las versiones del flujo de usuario. Anteriormente, se ofrecían las versiones V1 (para entornos de producción) y V1.1 y V2 (preliminares). Ahora, hemos consolidado los flujos de usuario en las versiones **recomendada** (versión preliminar de la próxima generación) y **estándar** (disponible con carácter general). Todos los flujos de usuario de las versiones preliminares heredadas (V1.1 y V2) van a pasar a estar en desuso el **1 de agosto de 2021**. Para más información, consulte [Versiones de flujos de usuario de Azure Active Directory B2C](user-flow-versions.md).

## <a name="prerequisites"></a>Requisitos previos

[Registre las aplicaciones](tutorial-register-applications.md) que formen parte de los flujos de usuario que desee crear.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Creación de un flujo de usuario de registro e inicio de sesión

Este flujo de usuario de registro y de inicio de sesión controla las experiencias de registro y de inicio de sesión del cliente con una sola configuración. A los usuarios se les lleva por el proceso correcto según el contexto.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

    ![Inquilino de B2C, panel de directorio y suscripción, Azure Portal](./media/tutorial-create-user-flows/directory-subscription-pane.png)

1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Flujos de usuario** y **Nuevo flujo de usuario**.

    ![Página Flujos de usuario del portal con el botón Nuevo flujo de usuario resaltado](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

1. En la página **Crear un flujo de usuario**, seleccione el flujo de usuario **Registrarse e iniciar sesión**.

    ![Página Selección de un flujo de usuario con las opciones Registrarse e Iniciar sesión resaltadas](./media/tutorial-create-user-flows/select-user-flow-type.png)

1. En **Seleccione una versión**, elija **Recomendada** y, luego, seleccione **Crear**. [Más información](user-flow-versions.md) sobre las versiones del flujo de usuario.

    ![Página Creación de flujo de usuario en Azure Portal con las propiedades resaltadas](./media/tutorial-create-user-flows/select-version.png)

1. Escriba un **nombre** para el flujo de usuario. Por ejemplo, *signupsignin1*.
1. En **Proveedores de identidades**, seleccione **Registro por correo electrónico**.
1. En **Atributos y notificaciones de usuario**, elija los atributos y las notificaciones que desea recopilar y enviar al usuario durante el registro. Por ejemplo, seleccione **Mostrar más** y elija los atributos y las notificaciones de **País o región**, **Nombre para mostrar** y **Código postal**. Haga clic en **OK**.

    ![Página de selección de atributos y notificaciones con tres notificaciones seleccionadas](./media/tutorial-create-user-flows/signup-signin-attributes.png)

1. Haga clic en **Crear** para agregar el flujo de usuario. El prefijo *B2C_1* se anexa automáticamente al nombre.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione el flujo de usuario que ha creado para abrir su página de información general y, luego, **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario** y, a continuación, seleccione **Registrarse ahora**.

    ![Página Ejecutar flujo de usuario del portal con el botón Ejecutar flujo de usuario resaltado](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Escriba una dirección de correo electrónico válida, haga clic en **Enviar código de verificación**, escriba el código de verificación que reciba y seleccione **Comprobar código**.
1. Escriba la contraseña nueva y confírmela.
1. Seleccione su país y región, escriba el nombre que desee que aparezca, escriba un código postal y haga clic en **Crear**. El token se devuelve al `https://jwt.ms` y debe mostrarse.
1. Ahora puede ejecutar el flujo de usuario nuevo y debe ser capaz de iniciar sesión con la cuenta que ha creado. El token devuelto incluye las notificaciones que seleccionó para país/región, nombre y código postal.

> [!NOTE]
> Actualmente, la experiencia "Ejecutar flujo de usuario" no es compatible con el tipo de dirección URL de respuesta de SPA que usa el flujo de código de autorización. Para usar la experiencia "Ejecutar flujo de usuario" con estos tipos de aplicaciones, registre una dirección URL de respuesta de tipo "Web" y habilite el flujo implícito tal como se describe [aquí](tutorial-register-spa.md).

## <a name="enable-self-service-password-reset"></a>Habilitar el autoservicio de restablecimiento de contraseña

Para habilitar el [autoservicio de restablecimiento de contraseña](add-password-reset-policy.md) para el flujo de usuario de registro o inicio de sesión:

1. Seleccione el flujo de usuario de registro o de inicio de sesión que creó.
1. En el menú de la izquierda, en **Configuración**, seleccione **Propiedades**.
1. En **Complejidad de la contraseña**, seleccione **Autoservicio de restablecimiento de contraseña**.
1. Seleccione **Guardar**.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione el flujo de usuario que ha creado para abrir su página de información general y, luego, **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar flujo de usuario**.
1. En la página de registro o inicio de sesión, seleccione **¿Olvidó la contraseña?**
1. Compruebe la dirección de correo electrónico de la cuenta que creó anteriormente y, luego seleccione **Continuar**.
1. Ahora tiene la oportunidad de cambiar la contraseña para el usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

## <a name="create-a-profile-editing-user-flow"></a>Creación de un flujo de usuario de edición de perfil

Si desea permitir que los usuarios editen sus perfiles en la aplicación, debe usar un flujo de usuario de edición de perfiles.

1. En el menú de la página de información general del inquilino de Azure AD B2C, seleccione **Flujos de usuario** y **Nuevo flujo de usuario**.
1. En la página **Crear un flujo de usuario**, seleccione el flujo de usuario **Edición de perfiles**. 
1. En **Seleccione una versión**, elija **Recomendada** y, luego, seleccione **Crear**.
1. Escriba un **nombre** para el flujo de usuario. Por ejemplo, *profileediting1*.
1. En **Proveedores de identidades**, seleccione **Inicio de sesión de cuenta local**.
2. En **Atributos de usuario**, elija los atributos que desee que el cliente pueda modificar en su perfil. Por ejemplo, seleccione **Mostrar más** y elija los atributos y las notificaciones de **Nombre para mostrar** y **Puesto**. Haga clic en **OK**.
3. Haga clic en **Crear** para agregar el flujo de usuario. El prefijo *B2C_1* se anexa automáticamente al nombre.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione el flujo de usuario que ha creado para abrir su página de información general y, luego, **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario** y, a continuación, inicie sesión con la cuenta que creó anteriormente.
1. Ahora tiene la oportunidad de cambiar el nombre para mostrar y el puesto del usuario. Haga clic en **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un flujo de usuario de registro e inicio de sesión
> * Creación de un flujo de usuario de edición de perfil
> * Creación de un flujo de usuario de restablecimiento de contraseña

A continuación, obtenga información sobre cómo usar Azure AD B2C para iniciar sesión y registrar usuarios en una aplicación. Siga la aplicación web de ASP.NET vinculada a continuación o vaya a otra aplicación de la tabla de contenido en **Autenticación de usuarios**.

> [!div class="nextstepaction"]
> [Tutorial: Habilitación de la autenticación en una aplicación web mediante Azure Active Directory B2C >](tutorial-web-app-dotnet.md)
