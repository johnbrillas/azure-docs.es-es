---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Litmus | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Litmus.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2020
ms.author: jeedes
ms.openlocfilehash: f2168715d59f9698cba58b7e91bbc897a8cd94f6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98727293"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-litmus"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Litmus

En este tutorial aprenderá a integrar Litmus con Azure Active Directory (Azure AD). Al integrar Litmus con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Litmus.
* Permitir que los usuarios inicien sesión automáticamente en Litmus con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Litmus.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Litmus admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-litmus-from-the-gallery"></a>Incorporación de Litmus desde la galería

Para configurar la integración de Litmus en Azure AD, es preciso agregar Litmus desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Litmus** en el cuadro de búsqueda.
1. Seleccione **Litmus** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-litmus"></a>Configuración y prueba del inicio de sesión único de Azure AD para Litmus

Configure y pruebe el inicio de sesión único de Azure AD con Litmus mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Litmus.

Para configurar y probar el inicio de sesión único de Azure AD con Litmus, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Litmus](#configure-litmus-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Litmus](#create-litmus-test-user)** , para tener un homólogo de B.Simon en Litmus vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Litmus**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://litmus.com/sessions/new`

1. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (sin procesar)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

1. En la sección **Configurar Litmus**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a Litmus mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Litmus**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-litmus-sso"></a>Configuración del inicio de sesión único de Litmus

1. Para automatizar la configuración en Litmus, debe instalar la **extensión del navegador de inicio de sesión seguro de Aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Set up Litmus** (Configurar Litmus) para ir a la aplicación del mismo nombre. Ahí, escriba las credenciales de administrador para iniciar sesión en Litmus. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 6.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar Litmus manualmente, en otra ventana del explorador web, inicie sesión en su sitio empresarial de Litmus como administrador.

1. Haga clic en **Security** (Seguridad) en el panel de navegación izquierdo.

    ![Captura de pantalla que muestra el elemento Security (Seguridad) seleccionado.](./media/litmus-tutorial/security-img.png)

1. En la sección **Configure SAML Authentication** (Configurar autenticación SAML), realice los pasos siguientes:

    ![Captura de pantalla que muestra la sección Configure SAML Authentication (Configurar autenticación de SAML), donde puede especificar los valores descritos.](./media/litmus-tutorial/configure1.png)

    a. Active el conmutador **Enable SAML** (Habilitar SAML).

    b. Seleccione **Generic** (Genérico) como valor de proveedor.

    c. Escriba el nombre en **Identity Provider Name** (Nombre del proveedor de identidad); por ejemplo: `Azure AD`

1. Lleve a cabo los siguiente pasos:

    ![Captura de pantalla que muestra la sección donde puede escribir los valores descritos.](./media/litmus-tutorial/configure3.png)

    a. En el cuadro de texto **SAML 2.0 Endpoint(HTTP)** (Punto de conexión SAML 2.0 [HTTP]), pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    b. Abra el archivo de **Certificado** descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **X.509 Certificate** (Certificado X.509).

    c. Haga clic en **Save SAML settings** (Guardar configuración de SAML).

### <a name="create-litmus-test-user"></a>Creación de un usuario de prueba de Litmus

1. En otra ventana del explorador web, inicie sesión en la aplicación Litmus como administrador.

1. Haga clic en **Accounts** (Cuentas) en el panel de navegación izquierdo.

    ![Captura de pantalla que muestra el elemento Accounts (Cuentas) seleccionado.](./media/litmus-tutorial/accounts-img.png)

1. Haga clic en la pestaña **Add New User** (Agregar nuevo usuario).

    ![Captura de pantalla que muestra el elemento Add New user (Agregar nuevo usuario) seleccionado.](./media/litmus-tutorial/add-new-user.png)

1. En la sección **Add User** (Agregar usuario), realice los pasos siguientes:

    ![Captura de pantalla que muestra la sección Add User (Agregar usuario), donde puede especificar los valores descritos.](./media/litmus-tutorial/user-profile.png)

    a. En el cuadro de texto **Email** (Dirección de correo electrónico), escriba la dirección de correo electrónico del usuario, en este caso, **B.Simon\@contoso.com**.

    b. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso, **B**.

    c. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    d. Haga clic en **Create User**(Crear usuario).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Litmus, donde puede iniciar el flujo de inicio de sesión.

* Vaya directamente a la dirección URL de inicio de sesión de Litmus e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Litmus para la que configuró el inicio de sesión único.

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Litmus en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de Litmus para la que ha configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Litmus, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).