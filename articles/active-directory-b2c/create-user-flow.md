---
title: Creación de un flujo de usuario en Azure Active Directory B2C
description: Aprenda a crear flujos de usuario en Azure Portal para habilitar el registro, el inicio de sesión y la edición de perfiles de usuario en sus aplicaciones de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9bd436b972dfb1549232831b1f07c3726ff459dd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100556494"
---
# <a name="create-a-user-flow-in-azure-active-directory-b2c"></a>Creación de un flujo de usuario en Azure Active Directory B2C

Puede crear varios [flujos de usuario](user-flow-overview.md) de diferentes tipos en el inquilino de Azure Active Directory B2C (Azure AD B2C) y usarlos en las aplicaciones según sus necesidades. Los flujos de usuario se pueden volver a usar en todas las aplicaciones.

> [!IMPORTANT]
> Hemos cambiado la manera en que hacemos referencia a las versiones del flujo de usuario. Anteriormente, se ofrecían las versiones V1 (para entornos de producción) y V1.1 y V2 (preliminares). Ahora, hemos consolidado los flujos de usuario en las versiones **recomendada** (versión preliminar de la próxima generación) y **estándar** (disponible con carácter general). Todos los flujos de usuario de las versiones preliminares heredadas (V1.1 y V2) van a pasar a estar en desuso el **1 de agosto de 2021**. Para más información, consulte [Versiones de flujos de usuario de Azure Active Directory B2C](user-flow-versions.md).

## <a name="before-you-begin"></a>Antes de empezar

- **Registre la aplicación** que desea usar para probar el nuevo flujo de usuario. Para obtener un ejemplo, consulte [Tutorial: Registrar una aplicación web en Azure AD B2C](tutorial-register-applications.md).
- **Agregue proveedores de identidades externos** si desea habilitar el inicio de sesión de usuario con proveedores como Azure AD, Amazon, GitHub, Facebook, LinkedIn, Microsoft o Twitter. Consulte [Incorporación de un proveedor de identidades en su inquilino de Azure Active Directory B2C](add-identity-provider.md).
- **Configure el proveedor de identidades de la cuenta local** para especificar los tipos de identidad (correo electrónico, nombre de usuario, número de teléfono) que desea admitir para las cuentas locales en el inquilino. Después, puede elegir entre los tipos de identidad admitidos cuando cree flujos de usuario individuales. Cuando un usuario completa el flujo de usuario, se crea una cuenta local en el directorio de Azure AD B2C; y el proveedor de identidades de la **cuenta local** autentica la información del usuario. Para configurar el proveedor de identidades de la cuenta local del inquilino, siga estos pasos:

   1. Inicie sesión en [Azure Portal](https://portal.azure.com/). 
   2. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD B2C.
   3. En la barra de búsqueda de la parte superior de Azure Portal, busque y seleccione **Azure AD B2C**.
   4. En **Administrar**, seleccione **Proveedores de identidades**.
   5. En la lista de proveedores de identidades, seleccione **Cuenta local**.
   6. En la página **Configuración de IDP local**, seleccione todos los tipos de identidades que desee admitir. Al seleccionar opciones aquí, estarán disponibles para los flujos de usuario que cree posteriormente:
      - **Teléfono** (versión preliminar): Permite al usuario escribir un número de teléfono, que se comprueba durante el registro y se convierte en el identificador de usuario.
      - **Correo electrónico** (valor predeterminado): Permite al usuario escribir una dirección de correo electrónico, que se comprueba durante el registro y se convierte en el identificador de usuario.
      - **Nombre de usuario**: Permite al usuario crear su identificador de usuario único. A continuación, se recopila una dirección de correo electrónico del usuario y se comprueba.
    7. Seleccione **Guardar**.

## <a name="create-a-user-flow"></a>Creación de un flujo de usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

    ![Inquilino de B2C, panel de directorio y suscripción, Azure Portal](./media/create-user-flow/directory-subscription-pane.png)

3. En Azure Portal, busque y seleccione **Azure AD B2C**.
4. En **Directivas**, seleccione **Flujos de usuario** y **Nuevo flujo de usuario**.

    ![Página Flujos de usuario del portal con el botón Nuevo flujo de usuario resaltado](./media/create-user-flow/signup-signin-user-flow.png)

5. En la página **Creación de un flujo de usuario**, seleccione el tipo de flujo de usuario que desea crear (consulte [Flujos de usuario en Azure AD B2C](user-flow-overview.md) para obtener información general).

    ![Selección de la página de flujo de usuario, con las opciones de registro e inicio de sesión resaltadas](./media/create-user-flow/select-user-flow-type.png)

6. En **Seleccione una versión**, elija **Recomendada** y, luego, seleccione **Crear**. [Más información](user-flow-versions.md) sobre las versiones del flujo de usuario.

    ![Página Creación de flujo de usuario en Azure Portal con las propiedades resaltadas](./media/create-user-flow/select-version.png)

7. Escriba un **nombre** para el flujo de usuario (por ejemplo, *signupsignin1*, *profileediting1*, *passwordreset1*).
8. En **Proveedores de identidades**, elija opciones en función del tipo de flujo de usuario que esté creando:

   - **Cuenta local**. Si desea permitir que los usuarios creen cuentas locales en el inquilino de Azure AD B2C, seleccione el tipo de identificador que desea usar (por ejemplo, correo electrónico, identificador de usuario o teléfono). Únicamente se enumerarán los tipos de identidades que se hayan configurado en los parámetro del [proveedor de identidades de la cuenta local](#before-you-begin).

   - **Proveedores de identidades sociales**. Si desea permitir el inicio de sesión de usuario con los [proveedores de identidades sociales que haya agregado](add-identity-provider.md) (por ejemplo, Azure AD, Amazon, Facebook, GitHub, LinkedIn, Microsoft o Twitter), seleccione los proveedores de la lista.

9. En **Atributos y notificaciones de usuario**, elija los atributos y las notificaciones que desea recopilar y enviar al usuario durante el registro. Seleccione **Mostrar más**. Seleccione los atributos y las notificaciones y, a continuación, seleccione **Aceptar**.

    ![Página de selección de atributos y notificaciones con tres notificaciones seleccionadas](./media/create-user-flow/signup-signin-attributes.png)

10. Seleccione **Crear** para agregar el flujo de usuario. El prefijo *B2C_1* se anexa automáticamente al nombre.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione **Directivas** > **Flujos de usuario** y, a continuación, el flujo de usuario que ha creado. En la página de información general del flujo de usuario, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web que registró en el paso 1. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar flujo de usuario**.
2. En función del tipo de flujo de usuario que esté probando, regístrese con una dirección de correo electrónico válida y siga el flujo de registro, o bien inicie sesión con una cuenta que haya creado anteriormente.

    ![Página Ejecutar flujo de usuario del portal con el botón Ejecutar flujo de usuario resaltado](./media/create-user-flow/sign-up-sign-in-run-now.png)

1. Siga las indicaciones de flujo de usuario. Cuando complete el flujo de usuario, el token se devuelve a `https://jwt.ms` y debería verlo.

> [!NOTE]
> Actualmente, la experiencia "Ejecutar flujo de usuario" no es compatible con el tipo de dirección URL de respuesta de SPA que usa el flujo de código de autorización. Para usar la experiencia "Ejecutar flujo de usuario" con estos tipos de aplicaciones, registre una dirección URL de respuesta de tipo "Web" y habilite el flujo implícito tal como se describe [aquí](tutorial-register-spa.md).

## <a name="next-steps"></a>Pasos siguientes

- [Adición de acceso condicional a flujos de usuario de Azure AD B2C](conditional-access-user-flow.md)
- [Personalización de la interfaz de usuario en un flujo de usuario de Azure AD B2C](customize-ui-with-html.md)
