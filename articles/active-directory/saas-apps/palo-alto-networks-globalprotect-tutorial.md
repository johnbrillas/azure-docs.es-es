---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Palo Alto Networks - GlobalProtect | Microsoft Docs'
description: Obtenga más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Palo Alto Networks - GlobalProtect.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/10/2020
ms.author: jeedes
ms.openlocfilehash: 72072feebfcf8dba249d2045a399e09714177698
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97963696"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-palo-alto-networks---globalprotect"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Palo Alto Networks - GlobalProtect

En este tutorial, obtendrá más información sobre cómo integrar Palo Alto Networks - GlobalProtect con Azure Active Directory (Azure AD). Al integrar Palo Alto Networks - GlobalProtect con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Palo Alto Networks - GlobalProtect.
* Permitir que los usuarios inicien sesión automáticamente en Palo Alto Networks - GlobalProtect con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Palo Alto Networks - GlobalProtect.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Palo Alto Networks - GlobalProtect admite el inicio de sesión único iniciado por **SP**.
* Palo Alto Networks - GlobalProtect admite el aprovisionamiento de usuarios **Just In Time**.

## <a name="adding-palo-alto-networks---globalprotect-from-the-gallery"></a>Agregar Palo Alto Networks - GlobalProtect desde la galería

Para configurar la integración de Palo Alto Networks - GlobalProtect en Azure AD, es preciso agregar Palo Alto Networks - GlobalProtect desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Palo Alto Networks - GlobalProtect** en el cuadro de búsqueda.
1. Seleccione **Palo Alto Networks - GlobalProtect** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-palo-alto-networks---globalprotect"></a>Configuración y prueba del inicio de sesión único de Azure AD para Palo Alto Networks - GlobalProtect

Configure y pruebe el inicio de sesión único de Azure AD con Palo Alto Networks - GlobalProtect mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Palo Alto Networks - GlobalProtect.

Para configurar y probar el inicio de sesión único de Azure AD con Palo Alto Networks - GlobalProtect, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Palo Alto Networks - GlobalProtect](#configure-palo-alto-networks---globalprotect-sso)** , para configurar las opciones de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Palo Alto Networks - GlobalProtect](#create-palo-alto-networks---globalprotect-test-user)** , para tener un homólogo de B.Simon en Palo Alto Networks - GlobalProtect que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Palo Alto Networks - GlobalProtect**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<Customer Firewall URL>`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico del cliente de Palo Alto Networks - GlobalProtect](https://support.paloaltonetworks.com/support) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up Palo Alto Networks - GlobalProtect** (Configurar Palo Alto Networks - GlobalProtect), copie las direcciones URL que necesite.

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

En esta sección, permitirá que B.Simon use el inicio de sesión único de Azure al concederle acceso a Palo Alto Networks - GlobalProtect.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Palo Alto Networks - GlobalProtect**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-palo-alto-networks---globalprotect-sso"></a>Configuración del inicio de sesión único de Palo Alto Networks - GlobalProtect

1. Abra Palo Alto Networks - GlobalProtect como administrador en otra ventana del explorador.

2. Haga clic en **Dispositivo**.

    ![Configuración del inicio de sesión único de Palo Alto 1](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Seleccione **Proveedor de identidades SAML** en la barra de navegación izquierda y haga clic en "Importar" para importar el archivo de metadatos.

    ![Configuración del inicio de sesión único de Palo Alto 2](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin2.png)

4. Realice las siguientes acciones en la ventana Importar:

    ![Configuración del inicio de sesión único de Palo Alto 3](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. En el cuadro de texto **Profile Name** (Nombre de perfil), proporcione un nombre, por ejemplo, Azure AD GlobalProtect.

    b. En **Metadatos del proveedor de identidades**, haga clic en **Examinar** y seleccione el archivo metadata.xml que ha descargado de Azure Portal.

    c. Haga clic en **Aceptar**

### <a name="create-palo-alto-networks---globalprotect-test-user"></a>Creación de un usuario de prueba de Palo Alto Networks - GlobalProtect

En esta sección, se va a crear un usuario llamado B.Simon en Palo Alto Networks - GlobalProtect. Palo Alto Networks - GlobalProtect admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en Palo Alto Networks - GlobalProtect, se crea otro después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Palo Alto Networks - GlobalProtect, desde donde puede comenzar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Palo Alto Networks - GlobalProtect y comience el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Palo Alto Networks - GlobalProtect en Aplicaciones, se debería iniciar sesión automáticamente en la instancia de Palo Alto Networks - GlobalProtect para la que configurara el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Palo Alto Networks - GlobalProtect, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración y la infiltración de la información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).