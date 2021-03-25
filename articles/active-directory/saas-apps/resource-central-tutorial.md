---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Resource Central | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Resource Central.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/13/2021
ms.author: jeedes
ms.openlocfilehash: dbc148fcbcd9c3be86a29df1e08755611a347b07
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100586627"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-resource-central"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Resource Central

En este tutorial, aprenderá a integrar Resource Central con Azure Active Directory (Azure AD). Al integrar Resource Central con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Resource Central.
* Permitir que los usuarios inicien sesión automáticamente en Resource Central con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción de Resource Central habilitada para el inicio de sesión único (SSO).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Resource Central admite el inicio de sesión único habilitado mediante **SP**.

* Resource Central admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-resource-central-from-the-gallery"></a>Adición de Resource Central desde la galería

Para configurar la integración de Resource Central en Azure AD, es necesario agregar la aplicación desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Resource Central** en el cuadro de búsqueda.
1. Seleccione **Resource Central** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-resource-central"></a>Configuración y prueba del inicio de sesión único de Azure AD para Resource Central

Configure y pruebe el inicio de sesión único de Azure AD con Resource Central mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Resource Central.

Para configurar y probar el inicio de sesión único de Azure AD con Resource Central, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
    1. **[Creación de un usuario de prueba de Resource Central](#create-resource-central-test-user)** , para tener un usuario equivalente a B.Simon en la aplicación que esté vinculado a su representación en Azure AD.
1. **[Configuración del inicio de sesión único en Resource Central](#configure-resource-central-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Resource Central**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En **Configuración básica de SAML**, especifique los valores de los siguientes campos:

   1. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<DOMAIN_NAME>/ResourceCentral`

   1. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón:`https://<DOMAIN_NAME>/ResourceCentral`

   1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<DOMAIN_NAME>/ResourceCentral/ExAuth/Saml2Authentication/Acs`

    > [!NOTE]
    > Estos valores no son literales. Actualícelos con los valores de la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte al cliente de Resource Central](mailto:st@aod.vn) para obtener estos valores.  También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En **Configurar Resource Central**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, se crea un usuario llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba `username@companydomain.extension`. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Resource Central mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Resource Central**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el panel **Usuarios y grupos**, seleccione **B.Simon** en la lista de **usuarios** y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol **Acceso predeterminado**.
1. En el panel **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-resource-central-test-user"></a>Creación de un usuario de prueba de Resource Central

En esta sección, se creará un usuario llamado **B.Simon** en **Resource Central**.

1. En Resource Central, seleccione **Seguridad** > **Personas** > **Nuevo**.
  
    :::image type="content" source="./media/resource-central/new-person.png" alt-text="Captura de pantalla que muestra el panel Personas en Resource Central, con el botón Nuevo resaltado.":::

1. En **Datos de la persona**, en **Nombre para mostrar**, escriba el usuario **B.Simon**. En **Dirección SMTP**, escriba el nombre de usuario de Azure AD del usuario. Por ejemplo, `B.Simon@contoso.com`.

    :::image type="content" source="./media/resource-central/person.png" alt-text="Captura de pantalla que muestra el panel Datos de la persona en Resource Central.":::

## <a name="configure-resource-central-sso"></a>Configuración del inicio de sesión único de Resource Central

En esta sección, configurará el inicio de sesión único en **Administrador del sistema de Resource Central**.

1. En Administrador del sistema de Resource Central, seleccione **External Authentication** (Autenticación externa).
1.  En **Enable Configuration** (Habilitar configuración), seleccione **Yes** (Sí).

    ![Captura de pantalla que muestra la opción Enable Configuration (Habilitar configuración) seleccionada en el panel External Authentication (Autenticación externa) de Resource Central.](./media/resource-central/enable.png)

1. En **Authentication Protocol** (Protocolo de autenticación), seleccione **SAML2**. 

   :::image type="content" source="./media/resource-central/protocol.png" alt-text="Captura de pantalla que muestra SAML2 seleccionado para el protocolo de autenticación en Resource Central":::.

1. En la sección **Configuración de SAML2**, especifique los valores de los campos siguientes:

    1. En **Identificador (id. de entidad)** , **Dirección URL de inicio de sesión**, **URL de cierre de sesión** e **Identificador de Azure AD**, escriba las direcciones URL pertinentes:

       :::image type="content" source="./media/resource-central/auth.png" alt-text="Captura de pantalla del panel de configuración de SAML2 en Resource Central.":::

        Copie las direcciones URL del panel **Configurar Resource Central**:

        :::image type="content" source="./media/resource-central/setup.png" alt-text="Captura de pantalla de la configuración de Resource Central en Resource Central":::

   1. En **Dirección URL de retorno**, escriba `https://<DOMAIN_NAME>/ResourceCentral/ExAuth/Saml2Authentication/CallbackHandler`.
  
1. En **Certificado**, cargue el certificado y escriba la contraseña.

   ![Captura de pantalla de la sección de certificado en Resource Central.](./media/resource-central/cert.png)
   
1. Seleccione **Guardar**.

1. Vuelva a **Azure Portal**. En **Certificado de firma de SAML**, cargue el certificado y escriba la contraseña.

   ![Captura de pantalla del panel de importación de certificado en Azure Portal.](./media/resource-central/cert2.png).

1. Seleccione **Agregar**.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD. Para probar el inicio de sesión único, tiene tres opciones:

* En Azure Portal, seleccione **Probar esta aplicación**. El vínculo redirige a la dirección URL de inicio de sesión de Resource Central, donde puede iniciar el inicio de sesión.

* Acceda directamente a la URL de inicio de sesión de Resource Central e inicie el inicio de sesión.

   :::image type="content" source="./media/resource-central/test.png" alt-text="Captura de pantalla de la página web de prueba de inicio de sesión único de Resource Central":::

* Use el portal Aplicaciones de Microsoft. En el portal Aplicaciones, seleccione el icono **Resource Central** para redirigir a la dirección URL de inicio de sesión de Resource Central. Para más información, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Resource Central para el inicio de sesión único con Azure AD, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
