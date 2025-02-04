---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con TextMagic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TextMagic.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.openlocfilehash: f1e6cd222c9ee8f40f81d4db3750956e8e698e3e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97607676"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-textmagic"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con TextMagic

En este tutorial aprenderá a integrar TextMagic con Azure Active Directory (Azure AD). Al integrar TextMagic con Azure AD, se puede:

* Controlar en Azure AD quién tiene acceso a TextMagic.
* Permitir que los usuarios inicien sesión automáticamente en TextMagic con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en TextMagic.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* TextMagic admite SSO iniciado por **IDP**

* TextMagic admite el aprovisionamiento de usuarios **Just-In-Time**

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-textmagic-from-the-gallery"></a>Agregar TextMagic desde la galería

Para configurar la integración de TextMagic en Azure AD, deberá agregar TextMagic desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **TextMagic** en el cuadro de búsqueda.
1. Seleccione **TextMagic** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-textmagic"></a>Configuración y prueba del inicio de sesión único de Azure AD para TextMagic

Configure y pruebe el inicio de sesión único de Azure AD con TextMagic mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de TextMagic.

Para configurar y probar el inicio de sesión único de Azure AD con TextMagic, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en TextMagic](#configure-textmagic-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en TextMagic](#create-textmagic-test-user)** : para tener un homólogo de B.Simon en TextMagic vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **TextMagic**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    En el cuadro de texto **Identificador**, escriba una dirección URL: `https://my.textmagic.com/saml/metadata`

5. La aplicación TextMagic espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación TextMagic espera que **nameidentifier** se corresponda con **user.mail**, por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación TextMagic espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre |   Atributo de origen| Espacio de nombres  |
    | --------------- | --------------- | --------------- |
    | company | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | phone | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up TextMagic** (Configurar TextMagic), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B.Simon acceda a TextMagic mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **TextMagic**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="configure-textmagic-sso"></a>Configuración del inicio de sesión único en TextMagic

1. Para automatizar la configuración en TextMagic, debe instalar la **extensión del explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Set up TextMagic** (Configurar TextMagic) para ir a la aplicación del mismo nombre. En ella, escriba las credenciales de administrador para iniciar sesión en TextMagic. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 5.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar TextMagic manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de TextMagic como administrador y lleve a cabo los siguientes pasos:

4. Seleccione **Configuración de la cuenta** bajo el nombre de usuario.

    ![Captura de pantalla que muestra la opción Account settings (Configuración de la cuenta) seleccionada desde el usuario.](./media/textmagic-tutorial/config1.png)

5. Haga clic en la pestaña **Inicio de sesión único (SSO)** y rellene los campos siguientes:  

    ![Captura de pantalla que muestra la pestaña Inicio de sesión único, donde puede escribir los valores descritos.](./media/textmagic-tutorial/config2.png)

    a. En el cuadro de texto **Identificador de entidad del proveedor de identidades:** , pegue el valor del campo **Identificador de Azure AD** que ha copiado de Azure Portal.

    b. En el cuadro de texto **URL de SSO del proveedor de identidades:** , pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    c. En el cuadro de texto **URL de SLO del proveedor de identidades:** , pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    d. Abra el **certificado codificado en base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado x509 público**.

    e. Haga clic en **Save**(Guardar).

### <a name="create-textmagic-test-user"></a>Creación de un usuario de prueba de TextMagic

La aplicación admite el **aprovisionamiento de usuarios Just-In-Time** y, tras la autenticación, los usuarios se crearán automáticamente en la aplicación. Debe rellenar la información una vez en el primer inicio de sesión para activar la cuenta secundaria en el sistema.
No hay ningún elemento de acción para usted en esta sección.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TextMagic en el panel de acceso y debería iniciar sesión automáticamente en la versión de TextMagic para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe TextMagic con Azure AD](https://aad.portal.azure.com/)