---
title: 'Tutorial: Integración de Azure Active Directory con Egnyte | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Egnyte.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2020
ms.author: jeedes
ms.openlocfilehash: 3d102b492326971ef186ba50a557ad8d1df5b6ec
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92454208"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-egnyte"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Egnyte

En este tutorial, aprenderá a integrar Egnyte con Azure Active Directory (Azure AD). Al integrar Egnyte con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Egnyte.
* Permitir que los usuarios inicien sesión automáticamente en Egnyte con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Egnyte.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Egnyte admite el inicio de sesión único iniciado por **SP**.
* Una vez que haya configurado Egnyte, puede aplicar el control de sesión, que protege a su organización, en tiempo real, frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-egnyte-from-the-gallery"></a>Adición de Egnyte desde la galería

Para configurar la integración de Egnyte en Azure AD, deberá agregar Egnyte desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Egnyte** en el cuadro de búsqueda.
1. Seleccione **Egnyte** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso"></a>Configuración y prueba del inicio de sesión único de Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Egnyte con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Egnyte.

Para configurar y probar el inicio de sesión único de Azure AD con Egnyte, es preciso completar los siguientes bloques de creación:

Para configurar y probar el inicio de sesión único de Azure AD con Egnyte, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Egnyte](#configure-egnyte-sso)**, para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Egnyte](#create-egnyte-test-user)**, para tener un homólogo de B.Simon en Egnyte que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Egnyte**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Egnyte](common/sp-signonurl.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.egnyte.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.egnyte.com/samlconsumer/AzureAD`
    
    > [!NOTE]
    > Estos valores no son reales. Actualice el valor con la dirección URL de inicio de sesión y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Egnyte](https://www.egnyte.com/corp/contact_egnyte.html) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

5. En la sección **Set up Egnyte** (Configurar Egnyte), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

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

En esta sección, va a permitir que B.Simon acceda a Egnyte mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Egnyte**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-egnyte-sso"></a>Configuración del inicio de sesión único de Egnyte

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Egnyte como administrador.

2. Haga clic en **Configuración**.
   
    ![Configuración 1](./media/egnyte-tutorial/ic787819.png "Configuración")

3. En el menú, haga clic en **Configuración**.

    ![Configuración](./media/egnyte-tutorial/ic787820.png "Configuración")

4. Haga clic en la pestaña **Configuración** y, a continuación, haga clic en **Seguridad**.

    ![Seguridad](./media/egnyte-tutorial/ic787821.png "Seguridad")

5. En la sección **Autenticación de inicio de sesión único**, realice los pasos siguientes:

    ![Single Sign On Authentication (Autenticación de inicio de sesión único)](./media/egnyte-tutorial/ic787822.png "Autenticación de inicio de sesión único")   
    
    a. En **Autenticación de inicio de sesión único**, seleccione **SAML 2.0**.
   
    b. En **Proveedor de identidades**, seleccione **AzureAD**.
   
    c. Pegue la **URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **URL de inicio de sesión del proveedor de identidades**.
   
    d. Pegue el **Identificador de Azure AD** que copió de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).
      
    e. Abra el certificado codificado en base 64 descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades**.
   
    f. En **Asignación de usuario predeterminada**, seleccione **Dirección de correo electrónico**.
   
    g. En **Usar valor de emisor específico de dominio**, seleccione **deshabilitado**.
   
    h. Haga clic en **Save**(Guardar).

### <a name="create-egnyte-test-user"></a>Creación de usuario de prueba de Egnyte

Para permitir que los usuarios de Azure AD inicien sesión en Egnyte, deben aprovisionarse en Egnyte. En el caso de Egnyte, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice los siguientes pasos:**

1. Inicie sesión en el sitio de la compañía de **Egnyte** como administrador.

2. Vaya a **Configuración \> Usuarios y grupos**.

3. Haga clic en **Agregar nuevo usuario** y, a continuación, seleccione el tipo de usuario que desea agregar.
   
    ![Usuarios](./media/egnyte-tutorial/ic787824.png "Usuarios")

4. En la sección **Nuevo usuario avanzado**, lleve a cabo estos pasos:
    
    ![New Standard User (Nuevo usuario estándar)](./media/egnyte-tutorial/ic787825.png "Nuevo usuario estándar")   

    a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon\@contoso.com**.

    b. En el cuadro de texto **Nombre de usuario**, escriba el nombre de un usuario; por ejemplo, **Britta Simon**.

    c. Seleccione **Inicio de sesión único** como **Tipo de autenticación**.
   
    d. Haga clic en **Save**(Guardar).
    
    >[!NOTE]
    >El titular de la cuenta de Azure Active Directory recibirá una notificación por correo electrónico.
    >

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Egnyte que proporcione Egnyte para aprovisionar cuentas de usuario de Azure AD.
>

### <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Egnyte en el Panel de acceso, debería iniciar sesión automáticamente en la versión de Egnyte para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)