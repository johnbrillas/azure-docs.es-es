---
title: 'Tutorial: Integración de Azure Active Directory con ArcGIS Online | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y ArcGIS Online.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/08/2021
ms.author: jeedes
ms.openlocfilehash: 7a445eefa31e741562105e89fa105d404ccc0c7e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101646357"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Integración de Azure Active Directory con ArcGIS Online

En este tutorial, aprenderá a integrar ArcGIS Online con Azure Active Directory (Azure AD). Al integrar ArcGIS Online con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a ArcGIS Online.
* Permitir que los usuarios inicien sesión automáticamente en ArcGIS Online con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en ArcGIS Online.

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* ArcGIS Online admite el inicio de sesión único iniciado por **SP**.

## <a name="add-arcgis-online-from-the-gallery"></a>Incorporación de ArcGIS Online desde la galería

Para configurar la integración de ArcGIS Online en Azure AD, deberá agregar ArcGIS Online desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **ArcGIS Online** en el cuadro de búsqueda.
1. Seleccione **ArcGIS Online** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-arcgis-online"></a>Configuración y prueba del inicio de sesión único de Azure AD para ArcGIS Online

Configure y pruebe el inicio de sesión único de Azure AD con ArcGIS Online mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de ArcGIS Online.

Para configurar el inicio de sesión único de Azure AD con ArcGIS Online, realice los pasos siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de ArcGIS Online](#configure-arcgis-online-sso)** : para configurar los valores de inicio de sesión único en el lado de la aplicación.
    1. **[Creación de un usuario de prueba de ArcGIS Online](#create-arcgis-online-test-user)** : permite tener un homólogo de B.Simon en ArcGIS Online que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **ArcGIS Online**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.maps.arcgis.com`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `<companyname>.maps.arcgis.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico para clientes de ArcGIS Online](https://support.esri.com/en/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. Para automatizar la configuración en **ArcGIS Online**, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![imagen](./media/arcgis-tutorial/install-extension.png)

7. Después de agregar la extensión al explorador, haga clic en **Setup ArcGIS Online** (Configurar ArcGIS Online) para ir a la aplicación ArcGIS Online. Desde allí, proporcione las credenciales de administrador para iniciar sesión en ArcGIS Online. La extensión de explorador web configurará automáticamente la aplicación y automatizará los pasos de la sección **Configuración del inicio de sesión único de ArcGIS Online**.

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

En esta sección, va a permitir que B.Simon acceda a ArcGIS Online mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones. seleccione **ArcGIS Online**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-arcgis-online-sso"></a>Configuración del inicio de sesión único de ArcGIS Online

1. Si quiere configurar ArcGIS Online manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de ArcGIS Online como administrador y realice los pasos siguientes:

2. Haga clic en **EDITAR CONFIGURACIÓN**.

    ![Edit Settings](./media/arcgis-tutorial/settings.png "Edit Settings") (Editar configuración)

3. Haga clic en **Seguridad**.

    ![Seguridad](./media/arcgis-tutorial/secure.png "Seguridad")

4. En **Inicios de sesión de la empresa**, haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

    ![Enterprise Logins (Inicios de sesión de la empresa)](./media/arcgis-tutorial/enterprise.png "Enterprise Logins (Inicios de sesión de la empresa)")

5. En la sección **Configurar proveedor de identidades** , realice los pasos siguientes:

    ![Set Identity Provider (Establecer proveedor de identidades)](./media/arcgis-tutorial/identity-provider.png "Establecer proveedor de identidades")

    a. En el cuadro de texto **Nombre**, escriba el nombre de la organización.

    b. En **Metadata for the Enterprise Identity Provider will be supplied using** (Los metadatos para el proveedor de identidades de la empresa se proporcionarán con), seleccione **A File** (Un archivo).

    c. Haga clic en **Choose file**(Elegir archivo) para cargar el archivo de metadatos descargado.

    d. Haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

### <a name="create-arcgis-online-test-user"></a>Creación de un usuario de prueba de ArcGIS Online

Para permitir que los usuarios de Azure AD inicien sesión en ArcGIS Online, tienen que aprovisionarse en ArcGIS Online.  
En el caso de ArcGIS Online, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **ArcGIS** .

2. Haga clic en **INVITAR A MIEMBROS**.

    ![Invite Members (Invitar a miembros)](./media/arcgis-tutorial/invite.png "Invitar a miembros")

3. Seleccione **Agregar miembros automáticamente sin enviar un correo electrónico** y luego haga clic en **SIGUIENTE**.

    ![Add Members Automatically (Agregar miembros automáticamente)](./media/arcgis-tutorial/members.png "Agregar miembros automáticamente")

4. En la página de diálogo **Miembros** , realice los pasos siguientes:

    ![Add and review (Agregar y revisar)](./media/arcgis-tutorial/review.png "Agregar y revisar")

     a. Escriba los valores para **Email** (Correo electrónico), **First name** (Nombre) y **Last name** (Apellidos) de una cuenta de Azure AD válida que quiera aprovisionar.

     b. Haga clic en **AGREGAR Y REVISAR**.
5. Revise los datos que ha escrito y luego haga clic en **AGREGAR MIEMBROS**.

    ![Add members (Agregar miembros)](./media/arcgis-tutorial/add.png "Agregar miembro")

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de ArcGIS Online donde podrá iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de ArcGIS Online e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de ArcGIS Online en Mis aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de ArcGIS Online. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado ArcGIS Online, puede aplicar el control de sesión, que protege la información confidencial de la organización de la filtración y la infiltración en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).