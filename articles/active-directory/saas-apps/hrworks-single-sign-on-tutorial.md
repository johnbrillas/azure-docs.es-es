---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con HRworks Single Sign-On | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y HRworks Single Sign-On.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/02/2020
ms.author: jeedes
ms.openlocfilehash: d6b23213d9d986c62a227b3e182a22896d128222
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92442548"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hrworks-single-sign-on"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con HRworks Single Sign-On

En este tutorial, aprenderá a integrar HRworks Single Sign-On con Azure Active Directory (Azure AD). Al integrar HRworks Single Sign-On con Azure AD, podrá:

* Controlar en Azure AD quién tiene acceso a HRworks Single Sign-On.
* Permitir que los usuarios inicien sesión automáticamente en HRworks Single Sign-On con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en HRworks Single Sign-On.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* HRworks Single Sign-On admite el inicio de sesión único iniciado por **SP**

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Incorporación de HRworks Single Sign-On desde la galería

Para configurar la integración de HRworks Single Sign-On en Azure AD, será preciso que agregue HRworks Single Sign-On desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **HRworks Single Sign-On** en el cuadro de búsqueda.
1. Seleccione **HRworks Single Sign-On** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-hrworks-single-sign-on"></a>Configuración y prueba del inicio de sesión único de Azure AD en HRworks Single Sign-On

Configure y pruebe el inicio de sesión único de Azure AD con HRworks Single Sign-On mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de HRworks Single Sign-On.

Para configurar y probar el inicio de sesión único de Azure AD con HRworks Single Sign-On, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de HRworks Single Sign-On](#configure-hrworks-single-sign-on-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de HRworks Single Sign-On](#create-hrworks-single-sign-on-test-user)** , para tener un homólogo de B.Simon en HRworks Single Sign-On que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **HRworks Single Sign-On**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de HRworks Single Sign-On](mailto:nadja.sommerfeld@hrworks.de) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up HRworks Single Sign-On** (Configurar HRworks Single Sign-On), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a conceder a B.Simon acceso a HRworks Single Sign-On utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **HRworks Single Sign-On**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-hrworks-single-sign-on-sso"></a>Configuración del inicio de sesión único de HRworks Single Sign-On

1. Para automatizar la configuración en HRworks Single Sign-On, debe instalar la **extensión del explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Instale la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

1. Después de agregar la extensión al explorador, haga clic en **Set up HRworks Single Sign-On** (Configurar HRworks Single Sign-On) para ir a esta aplicación. En ella, proporcione las credenciales de administrador para iniciar sesión en HRworks Single Sign-On. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 y 4.

    ![Configuración](common/setup-sso.png)

1. Si quiere configurar HRworks Single Sign-On manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de HRworks Single Sign-On como administrador y haga lo siguiente:

1. Haga clic en **Administrador** > **Datos básicos** > **Seguridad** > **Inicio de sesión único** en el lado izquierdo de la barra de menús y realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. Seleccione la casilla **Utilizar inicio de sesión único (SSO)** .

    b. Seleccione **Metadatos XML** como **método de entrada de metadatos**.

    c. Seleccione **Individual NameID identifier** (Identificador de NameID individual) como **Value for NameID** (Valor para NameID).

    d. En el Bloc de notas, abra el archivo XML de metadatos que descargó de Azure Portal, copie el contenido y luego péguelo en el cuadro de texto **Metadatos**.

    e. Haga clic en **Save**(Guardar).

### <a name="create-hrworks-single-sign-on-test-user"></a>Creación de un usuario de prueba de HRworks Single Sign-On

Para permitir que los usuarios de Azure AD inicien sesión en HRworks Single Sign-On, se deben aprovisionar en HRworks Single Sign-On. En HRworks Single Sign-On, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en HRworks Single Sign-On como administrador.

1. Haga clic en **Administrator** (Administrador) > **Persons** (Personas) > **Persons** (Personas) > **New person** (Nueva persona) en el lado izquierdo de la barra de menús.

     ![Captura de pantalla que muestra la página de H R Works con las opciones Persons (Personas) y New person (Nueva persona) seleccionadas.](./media/hrworks-single-sign-on-tutorial/configure02.png)

1. En el menú emergente, haga clic en **Next** (Siguiente).

    ![Captura de pantalla que muestra una lista de países que puede elegir para la persona.](./media/hrworks-single-sign-on-tutorial/configure03.png)

1. En el elemento emergente **Create new person with country for legal terms** (Crear nueva persona con un país para condiciones legales), rellene los detalles correspondientes **First name** (Nombre), **Last name** (Apellidos) y haga clic en **Create** (Crear).

    ![Captura de pantalla que muestra cuadros de texto en los que puede especificar el nombre y el apellido de la persona.](./media/hrworks-single-sign-on-tutorial/configure04.png)

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de HRworks Single Sign-On en el panel de acceso, debería iniciar sesión automáticamente en la versión de HRworks Single Sign-On para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe HRworks Single Sign-On con Azure AD](https://aad.portal.azure.com/)